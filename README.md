# IITG.ai_task_Sruteesh
# ⚡ Predicting Next Hour's electricity demand:
Use of classical Machine Learning models(tree, linear and kernel based), to predict next hour's demand with proper feature engineering and prevention of data leakage from past.
Most important things used are rolling statistics and cyclic encoding(things learnt from the last section of resources)

## 🚀 Project Highlights
   * **The initial missing values of solar and wind energy for some years were filled with 0**. Checking their initial production to be starting with 0. It was assumed that these features were not generating throughout these years.
     
   * **Merging of natural and demand dataframes on basis of time**. Followed by removal timestamps that belonged to future of test set(entire 2024)
     
   * **Resolution of irregular half-hour timestamps in between**:
           All timestamps having a a difference of 30 min with previous timestamp were considered(eg: 1, 1:30, 2). The missing features in these timestamps were taken as a weighted average of itself and previous timestamp in ratio of 2:1. Hence only past data is used to resolve. Logic considered was timestamp at 1:00 contains data from 12:00 to 1:00. And time stamp at 12:30 contains from 11:30 to 12:30.  Half of its data is also contained in previous timestamp. Hence 2:1.
     
   * **Duplicate entries were resolved by replacing them with the mean.**
     
   * **Missing features were imputed by use of rolling mean of past 24hrs** :The key feature of this task, is the refrainment from using regular imputation methods like interpolation. Instead we use rolling statistics(rolling mean, median).
     
   * **Anomaly detection using method of rolling z-scores and removal using rolling median of past 24hrs**: But we only considered rolling mean and rolling standard deviation the day to avoid any future influence. Considered threshold at 2.875 as it captures most of the outliers.
     
## Feature Engineering:
  * **Calendar Features and Cyclic Encoding**: Hour of day, day of week, day of year, month, year, week, season. Followed by cyclic encoding of features like hour, day of week, day of year, month.
  * **Lags and Rolling aggregates in demand_mw**: 1hr lag, 1 day lag, 6hrs and 1 week lags. Followed by rolling means and standard of 6hrs and 24hrs.
  * **Features created from corelation matrix**
    
  * **Integration of economic factors from economic.csv using feature importance**:  Logical mechanism was the use of Lasso regression with alpha=1 to filter out top features follwed by use of xgboost fit and its feature importance to take features like Urban Population and Foreign Investment that had decent importance. Along with those, I integrated some features I felt that were related like GDP, Population percent having electricity access, Manufacturing value,etc.

## Train test split and Modelling:
  * **Entire 2024 considered as test data**
  * **📈 Performance Summary**
       The models were evaluated using Mean Absolute Percentage Error (MAPE). Given the one-step-ahead nature of the forecasting, the accuracy was high
    |Model | MAPE (%)|
    | :--- | :--- |
    |**Random Forest** | **2.23%**|
    |**XGBoost** | **2.26%**|
    |**Linear Regression** | **2.74%**|
    |**Lasso Regression** | **2.78%**|
    |**Linear SVR** | **2.88%**|

## Feature Importance:
  * **Xgboost**: hour(encoded), demand, generation, solar, rollingstd(6h): XGBoost is prioritizing Seasonality and Cycles. By putting "Hour" at the very top, it is recognizing that electricity demand is primarily a function of the time of day. It also recognized that the solar power generation is gradually increasing.

  * **Random Forest**: RF has demand, generation followed by hour and rolling mean. This shows the fundamental difference between bagging, boosting. Its more prioritising current state and trend of the day.

  * **Lasso(L1) Regression**: It focuses on demand, rolling mean, rolling hour and day lag. It also removed several features that could possibly lead to overfitting and multicollinearity.

    
   
