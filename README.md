# Air Quality and Health Risk Prediction

**WGU D682 · AI Optimization for Computer Scientists · Task 1**

Predicts air pollution (PM2.5), health risk score and severity score from city environmental data. One command trains and tests a model for each target and saves all the evidence.

## How it works

- **Finds targets for you:** `--list-columns` lists every column and suggests what could be predicted.
- **Cleans the data:** fills missing values and turns text categories into numbers, without crashing on new categories.
- **Uses time:** turns timestamps into year, month, day, weekday and hour.
- **Tunes itself:** gradient boosting, with the best settings found by automatic search.
- **No cheating:** trains on earlier data and tests on later data, so the model never learns from the future (`TimeSeriesSplit`).
- **Scores honestly:** RMSE, MAE and MAPE on data held back from training.

## Results

| Target | RMSE | MAE | MAPE |
|---|---|---|---|
| PM2.5 | 4.86 | 4.25 | 13.6% |
| Health risk score | 0.22 | 0.18 | 1.9% |
| Severity score | 0.14 | 0.11 | 4.0% |

Each target's `artifacts/` folder has the metrics, every prediction, the features that mattered most, and charts.

## Run it

```
pip install -r requirements.txt
python dqn1_model.py --data "DQN1 Dataset.xlsx" --list-columns
python dqn1_model.py --data "DQN1 Dataset.xlsx" --target "PM2.5,health_risk_score" --datecol sunriseEpoch
```

## Files

- `dqn1_model.py`: the full pipeline
- `requirements.txt`: dependencies
- `artifacts/`: results for each target

**Built with:** Python, pandas, scikit-learn, matplotlib
