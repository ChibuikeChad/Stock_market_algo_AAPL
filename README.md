
---

# Stock Market Price Prediction using R

## Overview

This R script aims to predict stock market prices for a given financial instrument (in this case, Apple Inc., stock symbol 'AAPL') using time series analysis. The prediction involves exploring historical stock prices, creating visualizations, building a forecasting model, and evaluating its accuracy.

## Steps

### 1. Importing Required Packages

- **quantmod:** Used for importing financial data.
- **tseries:** Provides functions for time series analysis.
- **timeSeries:** Offers functionality for managing time series data.
- **forecast:** Enables time series forecasting.

```R
# Example: if (!require(quantmod)) install.packages("quantmod")
library(quantmod)
# ... Similar code for other packages ...
```

### 2. Importing Dataset

- The `getSymbols` function is used to fetch historical stock prices from Yahoo Finance for Apple Inc. ('AAPL') between January 1, 2018, and January 1, 2023.

```R
getSymbols('AAPL', from = '2018-01-01', to = '2023-01-01')
```

### 3. Data Exploration and Visualization

- Visualizes the last 12 months of stock data using candlestick charts and Bollinger Bands.

```R
chartSeries(AAPL, subset = 'last 12 months', type = 'auto')
addBBands()
```

- Creates subplots to visualize different attributes of the stock data.

```R
par(mfrow = c(2, 3))
# ... Plotting individual attributes ...
```

### 4. Exploring Linear Relations

- Uses Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) to explore linear relations in the time series data.

```R
par(mfrow = c(1, 2))
Acf(Predic_Price, main = 'ACF for differenced Series')
Pacf(Predic_Price, main = 'PACF for differenced Series', col = '#cc0000')
```

### 5. Augmented Dickey-Fuller Test

- Performs Augmented Dickey-Fuller Test to check for stationarity.

```R
adf_test_result <- adf.test(Predic_Price)
print(adf_test_result)
```

### 6. Prediction of Return

- Calculates daily returns and splits the data into training and testing sets.

```R
return_AAPL <- 100 * diff(log(Predic_Price))
AAPL_return_train <- return_AAPL[1:(0.9 * length(return_AAPL))]
AAPL_return_test <- return_AAPL[(0.9 * length(return_AAPL) + 1):length(return_AAPL)]
```

### 7. ARIMA Model Fitting

- Utilizes the ARIMA model for forecasting returns.

```R
arima_model <- auto.arima(AAPL_return_train, seasonal = FALSE)
fit <- Arima(AAPL_return_train, order = c(1, 0, 0))
```

### 8. Forecasting and Evaluation

- Generates predictions for the testing set and evaluates the forecast accuracy.

```R
preds <- predict(fit, n.ahead = (length(return_AAPL) - (0.9 * length(return_AAPL))))$pred
test_forecast <- forecast(fit, h = 15)
accuracy_result <- accuracy(test_forecast$mean, AAPL_return_test)
```

### 9. Visualizing Forecast

- Plots the forecasted values using the `forecast` package.

```R
par(mfrow = c(1, 1))
plot(test_forecast, main = "ARIMA Forecast for Apple Stock")
```

## Conclusion

This script provides a comprehensive analysis of historical stock prices for Apple Inc., including visualization, stationarity testing, and ARIMA model-based forecasting. Users can adapt and extend this script for other financial instruments or modify parameters for different time frames.
