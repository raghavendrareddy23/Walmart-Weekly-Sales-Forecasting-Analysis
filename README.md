# *Walmart Sales Forecasting Project – Documentation*

### **Project Title: *Walmart Weekly Sales Forecasting & Analysis***

## Objective:

To analyze weekly sales data across multiple Walmart stores and uncover trends, seasonal patterns, and the impact of external factors like fuel price, unemployment, and holidays. Ultimately, the project aims to help forecast sales and identify store-level performance.

### **Dataset Description:**

The dataset contains historical sales data for 45 Walmart stores located across the United States, with weekly sales figures recorded over a span of several years.

####      **Key Columns:**

* `Store`: Store ID

* `Date`: Week of sales (weekly intervals)

* `Weekly_Sales`: Target variable – sales amount

* `Holiday_Flag`: Whether the week contains a major holiday (Super Bowl, Labor Day, etc.)

* `Temperature`, `Fuel_Price`, `CPI`, `Unemployment`: External economic factors

### **Exploratory Data Analysis (EDA):**

####      **Data Overview:**

* No missing values in the dataset.

* Date column converted to `datetime` type for proper time-series analysis.

####      **Key Insights:**

* **Store 4** and **Store 20** are top-performing stores with consistently high weekly sales.

* Weekly sales are fairly stable across most stores, though **Store 38** and **Store 44** show downward trends.

* No missing dates; weekly data is continuous.  
* Weak **negative correlation** between `Unemployment` and `Weekly_Sales` (−0.11), suggesting unemployment does not significantly affect sales overall.

* Visual EDA using boxplot and line plot to detect trends and outliers.

![image](https://github.com/user-attachments/assets/b80cd179-851c-439e-8463-badcb3f157ea)


### **Model Overview:**

To forecast weekly sales at a store level, the project implements the **Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors (SARIMAX)** model. This method is ideal for time series data with:

* **Trend**  
* **Seasonality**  
* **External influences (exogenous features)** — though in this project, it is mainly used without external regressors.

### **Preprocessing Steps:**

1. **Resample Weekly Sales:**

   * `asfreq('W-FRI')` ensures consistent weekly frequency (every Friday).

2. **Box-Cox Transformation:**

   * Applied using `scipy.stats.boxcox` with `λ=0` to stabilize variance (log transformation).

   * Helps the SARIMAX model better handle heteroscedasticity in the sales data.

### **Model Configuration:**

<pre> ```python SARIMAX( endog=df_boxcox, order=(1, 0, 1), # ARIMA(p,d,q) seasonal_order=(1, 0, 1, 52) # Seasonal order with 52-week cycle ) ``` </pre>

* **ARIMA Parameters (p,d,q)**:

  * `p=1`: Autoregressive term

  * `d=0`: No differencing needed (stationary post-transformation)

  * `q=1`: Moving average term

* **Seasonal Order**:

  * `seasonal_order=(1,0,1,52)` captures annual seasonality since sales are weekly.

### **Forecasting Process:**

* The model is trained on a transformed version of the `Weekly_Sales`.

* Predictions are made on the test set.

* Inverse Box-Cox (exponential) is applied to return results to the original scale.

### **Evaluation Metrics:**

Model evaluation is done using:

* **MAE** (Mean Absolute Error)

* **MAPE** (Mean Absolute Percentage Error)

* **MSE**, **RMSE**

* **R² Score**

These metrics are collected per store and stored in a dictionary for comparative analysis across 45 stores.

### **Future Forecast:**

* Forecasts for the next **12 weeks** are generated using `.forecast(steps=12)`.

* These are also inverse-transformed and plotted on the same graph.

### **Visualization:**

The plots include:

* Training data

* Testing data

* Model predictions

* Future 12-week forecast

Each plot is labeled and rotated for readability, tailored for individual stores.

Visualization for store 1:

![image](https://github.com/user-attachments/assets/23116554-f948-46a8-a25a-fd6288db29ca)

### 

### 

### 

### 

### **Key Takeaways:**

* SARIMAX provides a robust framework for sales forecasting when data shows **seasonality**.

* The use of **Box-Cox transformation** significantly improves stationarity and model performance.

* The model can be scaled to multiple stores using a loop.


