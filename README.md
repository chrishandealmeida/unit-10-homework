# Unit 10 - A Yen for the Future

------------------

## Time Series Forecasting


### **Objective:**
Use time series analysis and modelling on historical Canadian Dollar-Yen exchange rate data to determine if there is any predictable behaviour.

### **Initial Time-Series Plotting**
Historical CAD-JPY exchange rate data in CSV format was read and trimmed to begin on January 1st, 1990.  

The resulting dataframe was plotted to see if any patterns could be visually observed:

![](images/cad-jpy-price.png)

Observation: No discernable long-term patterns could be seen.  However, high volatility in the CAD-JPY exchange rate was observed.  After the massive drop in CAD-JPY at the beginning of our dataframe (1992-1996), CAD had short-term spikes against JPY followed by sharp drops.  This seemed to repeat in ~4 year cycles with the exception of the 8 year unbroken run up of CAD from 2000-2008.


### **Decomposition Using a Hodrick-Prescott Filter**

The CAD-JPY exchange rate price data was decomposed into trend and noise using a Hodrick-Prescott Filter. 

The resulting dataframe was plotted to identify long-term and short-term patterns:

![](images/cad-jpy-trend-noise.png)

Observation: There was a long-term downward trend in CAD-JPY from 2015 to 2020.   In the short-term, there were periods of sharp increase in CAD-JPY, followed by periods of sharp decrease.  This is particularly true from late 2016 to early 2017 where CAD-JPY spiked, followed by a sharp decline into mid-2017.  This pattern appears to repeat somewhat about 4 or 5 more times in ~0.5-1 year cycles thereafter.

Plot of the Settle Noise:
![](images/cad-jpy-settle-noise.png)

### **Forecasting Returns using an ARMA model**
CAD-JPY returns were calculated as the first derivative of the price. 

After dropping the nans, the returns were used to estimate an ARMA model:

![](images/cad-jpy-arma.png)

The 5 Day Returns Forecast was plotted:

![](images/5-day-returns-forecast.png)

Because the p-value is greater than 0.05, the model is not a good fit.

### **Forecasting the Exchange Rate Price using an ARIMA Model**

The raw CAD-JPY exchange rate price was used to estimate an ARIMA model:

![](images/cad-jpy-arima.png)

The 5 Day Price Forecast was plotted:

![](images/5-day-price-forecast.png)

According to the ARIMA mode, CAD-JPY is forecasted to drop in the near term.  That means JPY is forecased to increase in value.

### **Forecasting volatility with GARCH**

The previously calculated returns data were used in a GARCH model to forecast near-term volatility:

![](images/cad-jpy-garch.png)

The last day of the dataset was used to create an annualized 5 day forecast of volatility:

![](images/5-day-volatility-forecast.png)

The 5 day forecast of volatility was plotted:

![](images/5-day-volatility-forecast-graph.png)

According to the model forecast, volatility will increase in the near term.

### **Conclusions**

I would buy yen now since the model predicted CAD-JPY to fall in the near term, meaning JPY is forecasted to increase.

The volatility is expected to increase, therefore the risk of yen is expected to increase.

I would not feel confident in using these models for trading because the p-value was above 0.05 which means the model is not a good fit.

## Linear Regression Forecasting

### **Data Preparation**
The historical CAD-JPY price data was used to create a dataframe.  The daily returns were then derived and nans were dropped.  The lagged returns were then calculated and the a train/test split was created.
### **Linear Regression Model**
Using sklearn, a linear regression model was created and fitted to the training data.

### **Make Predictions Using the Testing Data**
Y values were predicted using the test dataset.

The actual y data and predicted y data were put together in the same dataframe.  The first 20 predictions vs. the true values were plotted:

![](images/actual-vs-predicted-y-values.png)
### **Out-of-Sample Performance**
The model was evaluated using out-of-sample data.  The out-of-sample root mean squared error came out to 0.6445805658569028.

### **In-Sample Performance**

The model was evaluated using in-sample data.  The in-sample root mean squared error came out to 0.841994632894117.

### **Conclusions**
This model performs better on out-of-sample since it has a lower RMSE (0.64) compared to the in-sample data RMSE (0.84).