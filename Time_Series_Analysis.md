# Time Series Analysis Tutorial: Forecasting with ARIMA

## Objective:
In this tutorial, we will perform time series analysis using the ARIMA (AutoRegressive Integrated Moving Average) model. We will analyze historical data, decompose the time series, and use ARIMA to forecast future values.

## Dataset:
We'll use a public dataset containing monthly air passenger data from 1949 to 1960, which is a classic time series dataset.

---

## Step 1: Importing Libraries and Loading the Dataset

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from statsmodels.tsa.seasonal import seasonal_decompose
from statsmodels.tsa.arima_model import ARIMA
from sklearn.metrics import mean_squared_error
import numpy as np

# Load dataset
url = 'https://raw.githubusercontent.com/jbrownlee/Datasets/master/airline-passengers.csv'
df = pd.read_csv(url, parse_dates=['Month'], index_col='Month')

# Display first few rows
df.head()
```

- `pandas`: For data handling.
- `seaborn` and `matplotlib`: For visualizations.
- `ARIMA`: For model building and forecasting.

---

## Step 2: Exploratory Data Analysis (EDA)

### Plotting the Time Series Data

```python
# Plotting the time series
plt.figure(figsize=(10,6))
plt.plot(df, label='Airline Passengers')
plt.title('Monthly Airline Passenger Numbers (1949-1960)')
plt.xlabel('Year')
plt.ylabel('Number of Passengers')
plt.legend()
plt.show()
```

### Decomposing the Time Series

```python
# Decompose the time series
decomposition = seasonal_decompose(df, model='multiplicative')
decomposition.plot()
plt.show()
```

- **Trend**: Shows the long-term increase or decrease in the data.
- **Seasonality**: Shows recurring patterns or cycles.
- **Residual**: The noise or irregular component.

---

## Step 3: ARIMA Model

### Understanding ARIMA Parameters:
- **p**: The number of lag observations.
- **d**: The degree of differencing (to make the series stationary).
- **q**: The size of the moving average window.

### Checking for Stationarity (Differencing)

```python
# Differencing the data to make it stationary
df_diff = df.diff().dropna()

# Plot the differenced data
plt.figure(figsize=(10,6))
plt.plot(df_diff, label='Differenced Data')
plt.title('Differenced Monthly Airline Passenger Numbers')
plt.xlabel('Year')
plt.ylabel('Differenced Number of Passengers')
plt.legend()
plt.show()
```

### Building the ARIMA Model

```python
# Fitting the ARIMA model
model = ARIMA(df, order=(5,1,0))  # (p,d,q)
model_fit = model.fit(disp=0)

# Summary of the model
print(model_fit.summary())
```

---

## Step 4: Forecasting Future Values

```python
# Forecast the next 12 months
forecast, stderr, conf_int = model_fit.forecast(steps=12)

# Plot the forecast
plt.figure(figsize=(10,6))
plt.plot(df, label='Original Data')
plt.plot(pd.date_range(df.index[-1], periods=12, freq='M'), forecast, label='Forecast', color='red')
plt.fill_between(pd.date_range(df.index[-1], periods=12, freq='M'), 
                 conf_int[:, 0], conf_int[:, 1], color='pink', alpha=0.3)
plt.title('ARIMA Forecast')
plt.legend()
plt.show()
```

---

## Step 5: Model Evaluation

To evaluate the performance of the ARIMA model, we calculate the Mean Squared Error (MSE) on the forecasted values.

```python
# Splitting data for evaluation (train-test split)
train_size = int(len(df) * 0.8)
train, test = df[0:train_size], df[train_size:]

# Refit ARIMA model on train data
model = ARIMA(train, order=(5,1,0))
model_fit = model.fit(disp=0)

# Forecast the test period
forecast, stderr, conf_int = model_fit.forecast(steps=len(test))

# Calculate Mean Squared Error
mse = mean_squared_error(test, forecast)
print(f'Mean Squared Error: {mse}')
```

---

## Conclusion:
- **ARIMA** is a powerful model for time series forecasting.
- It's essential to decompose the series to understand the underlying components.
- Stationarity is a key requirement for ARIMA models, and differencing helps achieve this.
- After building the model, always validate with train-test splits to ensure accuracy.

---

## Next Steps:
- Experiment with different values of `p`, `d`, and `q` to optimize the model.
- Try using more advanced models like **SARIMA** (Seasonal ARIMA) for datasets with strong seasonal trends.
