<br>

# `#Time Series Analysis?`

<br>

**1. What is Time Series Data and Time Series Analysis?**
- Definition
- Real Life examples
- Key characteristics of time series data
- Goals of Time series analysis

**2. Time Series Decomposition:**
- Trend
- Seasonality
- Cyclic
  - Economic Cycles
    - Business Cycles
- Residuals
- Types of decomposition model
    - Additive
    - Multiplicative

**3. STL Decomposition (Seasonal and Trend Decomposition using LOESS):**
- How does it differ from classical decomposition?
- Choosing the right decomposition method

**4. Stationarity:**
- Why do we need stationarity?
- Types of stationarity
    - Strict stationarity
    - Weak stationarity
- Testing for weak stationarity
    - Augmented Dickey Fuller (ADF) Test
    - Kwiatkowski–Phillips–Schmidt–Shin (KPSS) test
- Testing for strict stationarity
- Which one to choose?
- Making a time series, Stationary
    - Differencing
        - First order
        - Second order
    - Transformation
        - Logarithmic
        - Power
        - Box–Cox
    - De–trending
        - Linear
        - Moving Average
- Seasonal Adjustment
- Choosing the right method


**5. White Noise and Random Walk:**
- White noise
  - Characteristics
- Random Walk
  - Characteristics
- Identifying white noise and random walk

**6. Time Series Forecasting Models:**
- Autoregressive (AR)
- Moving Average (MA)
- Autoregressive Moving Average (ARMA)
- Autoregressive Integrated Moving Average (ARIMA)
- Seasonal Autoregressive Integrated Moving Average (SARIMA)
- Vector AutoRegressive (VAR)
- Vector Moving Average (VMA)
- Vector AutoRegressive Moving Average (VARMA)
- Vector AutoRegressive Integrated Moving Average (VARIMA)

**7. Smoothing Methods:**
- Importance of Smoothing
- Moving Average
  - Simple Moving Average (SMA)
  - Weighted Moving Average (WMA)
  - Exponential Moving Average (EMA)
- Exponential Smoothing (ES)
  - Selecting alpha
  - Single Exponential Smoothing (SES)
  - Double Exponential Smoothing or Holt’s linear (DES)
  - Triple Exponential Smoothing or Holt-Winters (TES)
- Difference between EMA and ES

**8. Granger causality test:**

**9. Autocorrelation and Partial Autocorrelation Function:**
- ACF
- PACF
- Key difference from ACF
- Order of models (p, d, q, m)
- Identifying models from ACF and PACF

**10. Model Evaluation Metrics:**
- Mean Absolute Error (MAE)
- Mean Squared Error (MSE)
- Root Mean Squared Error (RMSE)
- Mean Absolute Percentage Error (MAPE)
- Akaike Information Criterion (AIC) and Bayesian Information Criterion (BIC)

**11. Data Preprocessing:**
- Handling Missing Values
- Making data stationary
- Handling Outliers
- Resampling

<br>
<br>

# **1. What is Time Series Data and Time Series Analysis?**

<br>
<br>

**Time Series Data:** Ovservations collected over a sequence of time intervals. And this time inverval may be in daily, weekly, secons or monthly basis. For example: in the below image, there have an date that means it's a time series data. And time intervals:

![image](img/img01.png)

Example of time series data:
- Stock Price 
- Sales Data
- Weather Data
- Our Social Media data

**Characteristics:**

`I. Chronological Order: Regular Intervals:`
*   **Simple Meaning:** The data is a list of events recorded at specific, evenly spaced times.
*   **Example:** The temperature reading taken at a weather station **every day at 12:00 PM**. The "regular interval" is one day. Another example is a company's **total sales at the end of every month**. The interval is one month.

`II. Sequential Order: Current Observation Depends on the Past:`
*   **Simple Meaning:** What happens today is influenced by what happened yesterday, last week, or last year. The order of the data points is crucial because they are connected.
*   **Example:** **Stock Prices.** The price of a stock today is heavily influenced by its price yesterday and the days before. If you scramble the order of the days, the data becomes meaningless.

`III. Temporal Components: Trend, Seasonality, Cycle, Noise:`
This is about breaking down the data into different patterns:
*   **Trend:** The long-term overall direction. Is it generally going up or down over a long period?
    *   *Example:* The **steady increase in global average temperature** over 50 years.
*   **Seasonality:** A pattern that repeats at a fixed, known interval (like a calendar).
    *   *Example:* **Sales of ice cream** go up every summer and down every winter. The "season" is one year.
*   **Cycle:** Up-and-down patterns that don't have a fixed calendar interval. They are longer than seasonal patterns and their duration can vary.
    *   *Example:* **Economic booms and recessions.** They happen repeatedly but not on a perfect, predictable calendar schedule (e.g., every 7 years).
*   **Noise (or Residual):** The random, unpredictable "jitter" left over after accounting for the trend, seasonality, and cycles.
    *   *Example:* A sudden, unexpected drop in ice cream sales on a hot day because the local factory had a power outage. It's the random luck or chance element.

`IV. Constant Frequency: Continuous Data Without Any Missing Value:`
*   **Simple Meaning:** The data is "clean" and comes in like clockwork. There are no gaps or jumps in time.
*   **Example:** A sensor that measures room temperature and **successfully saves a reading every single minute without fail**. The "frequency" is one minute, and it's constant.

`V. Dynamic Nature: Affected by External Factors`
*   **Simple Meaning:** The time series doesn't exist in a vacuum. It can be suddenly changed by outside events.
*   **Example:** **Website Traffic.**
    *   Normally, it might have a seasonal pattern (e.g., higher on weekends).
    *   But if a famous influencer shares a link to the site (**external factor**), traffic will suddenly and dramatically spike in a way that had nothing to do with its previous patterns.


`**Time Serics Analysis is a Statistical technique where we find meaninful insights about patterns and trend. Main task is understanding the past and forcast the future. For example: If we work for an ecommerce company, then understand the past data like sales, user engagement etc. We forcast the future like, what action the company should take to incrase their sales.**`

![image](img/img02.png)

`**Mostly, In time serics data we draw **Line Chart**. Above we draw a Line Chart. Then is another chart named **Candle Stick Chart.**`


<br>
<br>

# **2. Time Series Decomposition:**

<br>
<br>

`We previously read Temporal Components(Trend, Seasonality, Cycle, Noise/Residuals). This means we can decompose the time serics data into 4 parts i) Trend ii) Seasonality iii) Cycle iv) Noise/Residuals.`


**i. Trend:** Behaviour of the graph. Upward, downward or remaning same.

![image](img/img03.png)

`In the above graph,from  mid of January to Mid of April the trend is **Downword** and from mid of April to July the trend is **Upword** . `


**ii. Seasonality:** Repeating pattern at fixed intervals. 

![image](img/img04.png)

`Almost similar pattern, like in above image, the month (06-07) and the month (07-08) almost similar pattern. And for the both cases, from beginning of the months(fixed intervals).Like, see of AC increate in summer and decrese in winter.It's a seasonality.`


**iii. Cyclic:** Different from seasonality, will be repeat the pattern but not in fixed time intervals that happen in seasonality. There are two cycle
- Economic 
- Business 

**iv. Residuals/Noise:** Suddden fluctuation.

![image](img/img05.png)

`Marked circle is a residuals or Noise.`

<br>

`**Type of decomposition Model:`

There are two types of decomposition model,
- Additive 
- Multiplicative

The two main types of **Time Series Decomposition** models: **Additive** and **Multiplicative**. The Additive model represents the time series as a sum of its components ($Y_t = T_t + S_t + R_t$), while the Multiplicative model represents it as a product of its components ($Y_t = T_t \cdot S_t \cdot R_t$).

![image](img/img06.png)

`In 1st, image trend is increasing. But, the seasonality is also increase by the time.This type of model is called Multiplicative model. On the other hand, the second image is a additive model, trend is increasing but seasonality is fixed.`

![image](img/img04.png)

`Here, our decomposition model is addtitive in the upwording trend.`

**Classicial Decomposition:**
## [01_Classifical_Decompostion]()

**STL Decomposition using (LOESS):**
- LOESS - Locally Estimated Scatterplot Smoothing
- Difference Between Classicial vs STL Decomposition:
    - Classicial give us fixed seasonal pattern always, on the other hand STL give us the actual seasonal pattern.
    - Classicial easily influence by outliers, but STL does not.
    - Classicial can work with both(additive,multiplicative) but on the other hand STL work only (additive).
## [02_STL_Decomposition]()






