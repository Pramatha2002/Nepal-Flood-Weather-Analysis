# Nepal Flood & Weather Analysis: Next-Day River Discharge and Weather Prediction

Machine learning project that makes two next-day predictions at 10 river stations in Nepal:

1. **River discharge (m³/s)**, using Linear Regression, Decision Tree and Random Forest.
2. **Weather class (Cold, Cool, Warm or Hot)**, using Logistic Regression, Decision Tree and Random Forest.

Both parts are compared with a simple baseline ("tomorrow = today").

## Why this project

Nepal's rivers rise sharply during the monsoon. Predicting river flow and weather one day ahead can support river monitoring and earlier flood-risk warnings.

## Dataset

- 13,390 daily records from 10 river stations, 1 January 2023 to 31 August 2026
- 19 columns: station details, daily weather (rainfall, temperature, humidity, wind, soil moisture) and river discharge
- No missing values or duplicates
- Source: Nepal Flood and Weather Dataset 2023-2026 (Kaggle, tejal5kunjir)

## Workflow

**Part 1: River discharge**
1. Explore the data, then create the target `next_day_discharge`
2. Add rain lags, rolling rain totals, calendar features and encoded station details (51 features)
3. Split by date: 80% earliest dates for training, 20% latest for testing
4. Train three models and evaluate with MAE, MSE, RMSE and R²

**Part 2: Weather class**
1. Label each day Cold, Cool, Warm or Hot by comparing its temperature with its own station's normal (quartiles from the training dates)
2. Create the target `next_day_weather` and add yesterday's and 3-day average temperature (53 features)
3. Use the same date split as Part 1
4. Train three classifiers and evaluate with accuracy, macro F1 and a confusion matrix
5. Forecast the next day's class for each station

## Results (test set)

**River discharge**

| Model | MAE | RMSE | R² |
|---|---|---|---|
| Baseline (tomorrow = today) | 1.972 | 14.23 | 0.942 |
| Random Forest | 2.495 | 17.39 | 0.913 |
| Decision Tree | 3.217 | 24.16 | 0.831 |
| Linear Regression | 6.221 | 16.36 | 0.923 |

**Weather class**

| Model | Accuracy | F1 (macro) |
|---|---|---|
| Baseline (tomorrow = today) | 0.850 | 0.834 |
| Logistic Regression | 0.843 | 0.831 |
| Decision Tree | 0.798 | 0.781 |
| Random Forest | 0.824 | 0.810 |

**Key findings**
- Random Forest is the best model for discharge, and Logistic Regression is the best for weather class, but the baseline is slightly better in both parts because tomorrow is very similar to today.
- Today's discharge accounts for about 82% of the discharge model's importance; for weather, today's temperature and the recent trend matter most.
- Discharge errors are small on typical days but large on extreme floods (MAE 109.2 m³/s on the 10 highest days vs 2.49 overall).
- Warm is the hardest weather class, and mistakes are mostly between neighbouring classes.

## Limitations and future work

- Only about three years of data, so few monsoon seasons for training
- Extreme floods are rare, so the models learn them poorly
- Next steps: more years of data, 2 to 3 day forecasts, gradient boosting, and upstream stations

## Repository contents

```
├── Nepal_Flood_Discharge_Prediction.ipynb   # analysis and modelling notebook (both parts)
├── Nepal_Flood_Project_Presentation.pptx    # project presentation
└── README.md
```

## How to run

1. Open the notebook in Google Colab or Jupyter.
2. Upload the dataset CSV and update the file path in the first data-loading cell.
3. Run all cells from top to bottom.

## Tech stack

Python, pandas, NumPy, Matplotlib, Seaborn, scikit-learn
