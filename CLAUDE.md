# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Freqtrade is a Python crypto trading bot. This is a personal fork on the `my-develop` branch (tracking `upstream/develop`). Day-to-day work here is strategy development, backtesting, hyperopt, and dry-run trading — not core-engine changes. PRs against upstream go to `develop`, never `stable`.

User-specific assets live under `user_data/`: configs in `user_data/configs/`, strategies in `user_data/strategies/` (with subfolders like `berlinguyinca/`, `mystrategies/`, `futures/`), and dry-run DBs in `user_data/trades/`. `commands.md` is the user's personal cheat-sheet of invocations actually used in this repo.

## Commands

Setup (editable install with all extras + pre-commit):
```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements-dev.txt && pip install -e .[all]
pre-commit install
```

Lint / format / type-check (must pass before committing; mirrors `.pre-commit-config.yaml`):
```bash
ruff check .       # lint
ruff format .      # format (line-length 100)
mypy freqtrade     # type-check (package only; tests are ignored)
pre-commit run -a  # run every hook at once
```

Tests (pytest, `asyncio_mode=auto`, runs distributed by default):
```bash
pytest                                              # whole suite
pytest tests/test_<file>.py                         # one file
pytest tests/test_<file>.py::test_<name>            # one test
pytest -n auto                                      # parallel (xdist)
pytest --cov=freqtrade --cov-report=html           # coverage
```

Running the bot (entry point `freqtrade = freqtrade.main:main`). Configs and `--db-url` are stacked per strategy; `-c` is repeatable and later files override earlier ones:
```bash
freqtrade trade        -c user_data/configs/basic-config.json --strategy-path user_data/strategies --strategy <Name> --db-url sqlite:///user_data/trades/<Name>-dryrun.sqlite
freqtrade backtesting  -c user_data/configs/basic-config.json --strategy-path user_data/strategies --strategy <Name> --timerange 20240101-20241224
freqtrade hyperopt     -c user_data/configs/donchian-config.json --strategy <Name> --hyperopt-loss MultiMetricHyperOptLoss -e 500 --spaces all
freqtrade download-data -c user_data/configs/basic-config.json --timeframe 5m --timerange 20241001- --prepend
freqtrade test-pairlist -c user_data/configs/basic-config.json
freqtrade list-strategies --recursive-strategy-search
freqtrade new-strategy --strategy MyStrategy --template full
```
See `commands.md` for many more real examples (FreqAI, edge, plotting, per-strategy trade invocations).

## Architecture

Live trade flow: `freqtrade/main.py` (arg parsing + dispatch to `args["func"]`) → `commands/trade_commands.py::start_trading` → `worker.py::Worker` (config load, throttled main loop, bot state machine) → `freqtradebot.py::FreqtradeBot.process()` runs once per candle: refresh markets/wallets → refresh OHLCV via `DataProvider` → `strategy.analyze()` → `exit_positions()` → `enter_positions()` → commit to DB.

Key subsystems (`freqtrade/`):
- **strategy/interface.py** — `IStrategy` ABC. Strategies implement `populate_indicators`, `populate_entry_trend`, `populate_exit_trend`, plus optional hooks (`custom_stoploss`, `custom_exit`) and attributes (`minimal_roi`, `stoploss`, `timeframe`, `order_types`, `protections`).
- **exchange/** — `Exchange` wraps CCXT; per-exchange subclasses (`Binance`, `Bybit`, `Kraken`, …) hold venue-specific quirks.
- **persistence/** — SQLAlchemy models (`Trade`, `Order`, `PairLock`). Live mode persists to SQLite/Postgres; backtesting uses in-memory `LocalTrade`. Thread-safe via `scoped_session`.
- **data/dataprovider.py** — unified OHLCV/ticker/orderbook access; pulls from exchange (live) or serves from memory (backtest).
- **plugins/pairlist/** + `pairlistmanager.py` — chain of `IPairList` handlers (StaticPairList, VolumePairList, filters…) that build the whitelist.
- **plugins/protections/** + `protectionmanager.py` — chain of `IProtection` handlers that lock pairs/trading (cooldown, stoploss_guard, max_drawdown…).
- **optimize/** — `backtesting.py` (`Backtesting`) and `hyperopt/`. Backtesting reuses the *same* `IStrategy` code as live; only the data source and order-fill simulation differ.
- **rpc/** — `RPCManager` fans out to Telegram, FastAPI REST (`api_server/`, port 8080), webhooks, Discord.
- **freqai/** — optional ML/RL layer (LightGBM/XGBoost/Catboost/RL), invoked from a strategy's populate methods.

### Central pattern: dynamic plugin loading via IResolver
`resolvers/iresolver.py` is the backbone for all pluggable components. `IResolver` subclasses (`StrategyResolver`, `ExchangeResolver`, `PairListResolver`, `ProtectionResolver`, `FreqaiModelResolver`) use `importlib` to discover and instantiate classes by name from a search path (built-in package dir → `user_data/` → config-specified `--*-path`). Consequence: **there is no hardcoded registry** — to add a strategy/pairlist/protection, drop a `.py` file implementing the right base class into the appropriate `user_data/` dir (or pass `--strategy-path`) and it's auto-discovered.

### Config cascading
`configuration/configuration.py::Configuration.load_config()` merges multiple JSON config files (later overrides earlier), overlays env vars and CLI args, applies defaults, and validates against the JSON schema in `config_schema/`. A strategy file can override config attributes (`minimal_roi`, `stoploss`, `timeframe`). Precedence: **CLI args > strategy file > config file(s) > defaults**.

## Conventions

- Line length 100 everywhere (ruff/black/isort/flake8). isort uses the `black` profile; `freqtrade_client` is first-party.
- Docstrings on all public methods, double-quoted, reST format (`:param:`, `:return:`, `:raises:`).
- New features need unit tests in `tests/` (type errors there are intentionally ignored by mypy).
