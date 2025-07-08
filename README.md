# Time Series Analysis (ID: 20241314)

This repository contains the R notebook (`20241314.ipynb`) and outputs for the quarterly `austres` series analysis, covering the following 1-8 Questions.

## Overview of Questions & Results

1. **Q1: Exploratory Analysis & Train/Test Split**  
   - Loaded `austres` (1971–1994, quarterly).  
   - Plotted series, ACF (up to lag 30), seasonal subseries and combined TS display.  
   - Split into training (first 62 obs, ~70%) and test (last 27 obs) sets.

2. **Q2: Stationarity Tests & Differencing**  
   - Original series is non‐stationary (ADF p>0.05, KPSS p<0.05).  
   - Seasonal (lag 4) or non‐seasonal (d=1) alone insufficient.  
   - Combined differencing (d=1, D=1) yields ADF p<0.05 and KPSS p>0.05 → stationary.

3. **Q3: Log‐transform ARIMA (M2)**  
   - Transformed via `log()`, applied d=1 & D=1 differencing.  
   - ACF/PACF suggested MA(1) × SMA(1)[4] → **ARIMA(0,1,1)(0,1,1)[4]**.  
   - In‐sample RMSE≈0.00275 on log‐scale, Ljung–Box p≈0.3045, Shapiro–Wilk p<0.05 (non‐normal).

4. **Q4: Box–Cox ARIMA (M3)**  
   - Optimal λ≈–0.261 via `BoxCox.lambda()`.  
   - After d=1 & D=1, ACF suggested MA(3), PACF spike at lag 4 → **ARIMA(0,1,3)(1,1,0)[4]**.  
   - AICc≈–1162.26, RMSE≈0.00149 (on transformed scale), Ljung–Box p≈0.1081.

5. **Q5: Auto‐ARIMA (M4) & Model Comparison**  
   - `auto.arima()` chose model `…` (see metrics table).  
   - Comparison of AIC, AICc, BIC:

     | Model |   AIC   |   AICc   |   BIC   |
     |:-----:|:-------:|:--------:|:-------:|
     | M1    |  432.09 |   433.65 | 444.76  |
     | M2    | −679.78 | −679.33  | −673.65 |
     | M3    | −1163.43| −1162.26 | −1153.22|
     | M4    |  (… )   |   (…)    |   (…)   |

   - Residual diagnostics (Ljung–Box & Shapiro–Wilk) show all pass autocorrelation tests but residuals deviate from normality.

6. **Q6: Forecast on Test Set**  
   - Generated 27‐step forecasts for each model.  
   - Back‐transformed M2 (exp) and M3 (InvBoxCox) to original scale.  

7. **Q7: Forecast Accuracy on Test Set**  
   - Computed MSE, RMSE, MAE, MPE, MAPE:

     | Model |   MSE    |  RMSE   |   MAE   |  MPE  |  MAPE  |
     |:-----:|:--------:|:-------:|:-------:|:-----:|:-------|
     | M1    | 11924.97 | 109.20  |  96.78  | 0.563 | 0.563  |
     | M2    |  5678.12 |  75.35  |  63.81  | 0.346 | 0.375  |
     | M3    |  4026.55 |  63.46  |  48.36  | 0.033 | 0.281  |
     | M4    |  3850.36 |  62.05  |  50.68  | 0.089 | 0.296  |

   - **Best by MAPE: M3**.

8. **Q8: 10‐step‐ahead Forecast & Plot (M3)**  
   - Produced and plotted 10‐quarter forecasts with 80%/95% intervals.
   - Forecast continues the gentle upward trend; intervals widen over time.

## Reproducing the Analysis

1. **Dependencies**  
   ```r
   install.packages(c("forecast", "tseries"))
