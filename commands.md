# Commands

<!-- code block -->
### Edge Positioning
```bash
freqtrade edge \
--strategy-path user_data/strategies/berlinguyinca \
-c user_data/configs/basic-config.json \
--strategy BinHV45
```

### Download Data
```bash
freqtrade download-data \
--timeframe 5m \
-c user_data/configs/basic-config.json \
--timerange 20241001- \
--prepend
```

### Backtest
```bash
freqtrade backtesting \
--strategy-path user_data/strategies/mystrategies \
-c user_data/configs/basic-config.json \
--strategy MultiIndicatorStrategy
```
```bash
freqtrade plot-profit  \
-p LTC/BTC \
--export-filename user_data/backtest_results/backtest-.json
```

```bash
freqtrade backtesting \
--strategy-path user_data/strategies/mystrategies \
-c user_data/configs/basic-config.json \
--timerange 20240101-20241224 \
--strategy GodStra
```

### test pairlist
```bash
freqtrade test-pairlist \
-c user_data/configs/basic-config.json \
-c user_data/configs/config_freqai.json
```


freqtrade backtesting \
--strategy-path user_data/strategies/ \
-c user_data/configs/basic-config.json \
--strategy GodStra


freqtrade download-data --days 100 --timeframe 12h

freqtrade backtesting \
--strategy-path user_data/strategies/berlinguyinca \
--strategy BinHV45 \
--timerange 20230301-

freqtrade backtesting \
--strategy FreqaiExampleStrategy \
--strategy-path freqtrade/templates \
--config user_data/config_freqai.json \
--freqaimodel LightGBMRegressor \
--timerange 20210501-20210701


freqtrade test-pairlist

freqtrade trade \
--config user_data/configs/config_freqai.json \
--strategy FreqaiExampleStrategy \
--freqaimodel LightGBMRegressor


freqtrade trade \
--strategy-path user_data/strategies/berlinguyinca \
--strategy ReinforcedQuickie \
--config config.json \

freqtrade trade \
--strategy-path user_data/strategies/berlinguyinca \
--strategy ReinforcedSmoothScalp \
--config user_data/configs/config_scalp.json \
--db-url sqlite:///user_data/tradesv3.dryrun.sqlite

freqtrade trade \
--freqaimodel ReinforcementLearner \
--strategy MyRLStrategy \
--config config.json

freqtrade backtesting \
--timeframe 5m \
--strategy-path user_data/strategies/berlinguyinca \
--strategy-list Strategy001 Strategy002 Strategy003 Strategy004 Strategy005 \
--export trades

freqtrade trade \
--strategy-path user_data/strategies \
--strategy FreqaiExampleStrategy \
--config user_data/configs/config_freqai2.json \
--freqaimodel LightGBMRegressor

freqtrade backtesting \
--config user_data/configs/config_freqai2.json \
--strategy-path user_data/strategies \
--strategy FreqaiExampleHybridStrategy \
--freqaimodel CatboostClassifier
--timerange 20230101-20230601

freqtrade backtesting \
--config user_data/configs/config_freqai3.json \
--timerange 20230101-20230601 \
--freqaimodel ReinforcementLearner \
--strategy-path user_data/strategies \
--strategy FreqaiRLStrategy

freqtrade hyperopt \
-s ReinforcedSmoothScalp \
--strategy-path user_data/strategies/berlinguyinca \
--hyperopt-loss SharpeHyperOptLossDaily \
--config user_data/configs/config_scalp.json \
--eps \
--timerange 20230601-


freqtrade trade \
--config user_data/configs/config_freqai-trade.json \
--strategy FreqaiExampleStrategy \
--freqaimodel LightGBMRegressor \
--strategy-path freqtrade/templates

freqtrade backtesting \
--config user_data/configs/config_freqai-trade.json \
--strategy FreqaiExampleStrategy \
--freqaimodel LightGBMRegressor \
--strategy-path user_data/strategies \
--timerange 20230501-20230601

freqtrade trade -c user_data/configs/basic-config.json \
-c user_data/configs/config-diamond.json \
--strategy-path user_data/strategies \
--strategy Diamond \
--db-url sqlite:///user_data/trades/diamond-tradesv3.dryrun.sqlite

freqtrade trade -c user_data/configs/basic-config.json \
-c user_data/configs/config_freqai3.json \
--strategy-path user_data/strategies \
--strategy FreqaiRLStrategy \
--freqaimodel ReinforcementLearner \
--db-url sqlite:///user_data/trades/FreqaiRLStrategy-tradesv3.dryrun.sqlite

freqtrade trade -c user_data/configs/basic-config.json \
--config user_data/configs/config_freqai-trade.json \
--strategy FreqaiExampleStrategy \
--freqaimodel LightGBMRegressor \
--strategy-path user_data/strategies \
--db-url sqlite:///user_data/trades/FreqaiLightGBMStrategy-tradesv3.dryrun.sqlite

freqtrade trade -c user_data/configs/basic-config.json \
-c user_data/configs/config_binhv45.json \
--strategy-path user_data/strategies/berlinguyinca \
--strategy BinHV45 \
--db-url sqlite:///user_data/trades/BinHV45-tradesv3.dryrun.sqlite

freqtrade trade -c user_data/configs/basic-config.json \
-c user_data/configs/config_binhv27.json \
--strategy-path user_data/strategies/berlinguyinca \
--strategy BinHV27 \
--db-url sqlite:///user_data/trades/BinHV27-tradesv3.dryrun.sqlite


freqtrade trade -c user_data/configs/basic-config.json \
-c user_data/configs/config_scalp.json \
--strategy-path user_data/strategies/berlinguyinca \
--strategy ReinforcedSmoothScalp \
--db-url sqlite:///user_data/trades/ReinforcedSmoothScalp-tradesv3.dryrun.sqlite

freqtrade trade -c user_data/configs/basic-config.json \
-c user_data/configs/config_quickie.json \
--strategy-path user_data/strategies/berlinguyinca \
--strategy Quickie \
--db-url sqlite:///user_data/trades/Quickie-tradesv3.dryrun.sqlite

freqtrade trade -c user_data/configs/basic-config.json \
-c user_data/configs/config_godstra.json \
--strategy-path user_data/strategies \
--strategy GodStra \
--db-url sqlite:///user_data/trades/godstra-tradesv3.dryrun.sqlite

freqtrade trade -c user_data/configs/basic-config.json \
-c user_data/configs/config_macd.json \
--strategy-path user_data/strategies/berlinguyinca \
--strategy MACDStrategy \
--db-url sqlite:///user_data/trades/MACDStrategy-tradesv3.dryrun.sqlite

freqtrade trade -c user_data/configs/basic-config.json \
-c user_data/configs/config_supertrend.json \
--strategy-path user_data/strategies \
--strategy Supertrend \
--db-url sqlite:///user_data/trades/supertrend-tradesv3.dryrun.sqlite

freqtrade trade -c user_data/configs/basic-config.json \
-c user_data/configs/config_freqai4.json \
--strategy-path user_data/strategies \
--strategy FreqaiExampleHybridStrategy \
--freqaimodel XGBoostClassifier \
--db-url sqlite:///user_data/trades/FreqaiExampleHybridStrategy-tradesv3.dryrun.sqlite

ADXMomentum ASDTSRockwellTrading AdxSmas AverageStrategy AwesomeMacd BbandRsi BinHV27 BinHV45 CCIStrategy CMCWinner
ClucMay72018 CofiBitStrategy CombinedBinHAndCluc DoesNothingStrategy EMASkipPump Freqtrade_backtest_validation_freqtrade1
Low_BB MACDStrategy MACDStrategy_crossed MultiRSI Quickie ReinforcedAverageStrategy ReinforcedQuickie ReinforcedSmoothScalp
Scalp Simple SmoothOperator SmoothScalp TDSequentialStrategy TechnicalExampleStrategy


AwesomeStrategy GodStra Strategy002 Bandtastic Heracles Strategy003 BreakEven HourBasedStrategy Strategy004 CustomStoplossWithPSAR
InformativeSample Strategy005 hlhb Diamond  MultiMa  Supertrend FixedRiskRewardLoss PatternRecognition SwingHighToSky mabStra
PowerTower UniversalMACDmulti_tf Strategy001
Strategy001_custom_exit sample_strategy

freqtrade hyperopt
-c user_data/configs/basic-config.json \
--strategy-path user_data/strategies/futures \
--strategy FReinforcedStrategy \
--db-url sqlite:///user_data/trades/FreqaiExampleHybridStrategy-tradesv3.dryrun.sqlite
