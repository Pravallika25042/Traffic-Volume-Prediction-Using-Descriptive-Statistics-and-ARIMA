# Traffic Volume Prediction Using Descriptive Statistics and ARIMA

The project follows the complete workflow from raw vehicle-level observations to final traffic prediction and model interpretation.

---

## 🎯 Project Objectives

The major objectives of this project are to:

1. Understand and validate the structure of the traffic dataset.
2. Clean the raw vehicle-level observations.
3. Identify missing values and duplicate records.
4. Analyze traffic volume using descriptive statistics.
5. Study traffic patterns across dates and hours.
6. Analyze vehicle-speed characteristics.
7. Compare speed behaviour across vehicle classes.
8. Prepare hourly traffic data for time-series modelling.
9. Test whether the traffic-volume series is stationary.
10. Determine the required ARIMA differencing order.
11. Analyze temporal dependencies using ACF and PACF.
12. Compare multiple ARIMA model configurations.
13. Select a suitable model using AIC/BIC.
14. Validate the selected model through residual diagnostics.
15. Generate a 24-hour traffic-volume forecast.
16. Evaluate forecasting performance using unseen holdout observations.
17. Interpret the results, limitations, and practical applications of the model.

---

## 🧠 Why This Approach?

The project combines **descriptive statistics with ARIMA-based time-series modelling**.

Descriptive analysis is performed first to understand traffic behaviour, identify variability, detect unusual observations, and examine time-dependent patterns.

ARIMA is then used because traffic-volume observations form a time series in which current observations can depend on previous observations.

The overall analytical approach is:

```
Raw Traffic Data
       ↓
Data Cleaning & Validation
       ↓
Exploratory Data Analysis
       ↓
Descriptive Statistics
       ↓
Traffic Volume Analysis
       ↓
Speed Analysis
       ↓
Time-Series Preparation
       ↓
Stationarity Testing
       ↓
ACF & PACF Analysis
       ↓
ARIMA Model Comparison
       ↓
Residual Diagnostics
       ↓
24-Hour Forecast
       ↓
Holdout Evaluation
       ↓
Final Insights
```

---

## 📊 Dataset

The traffic dataset contains vehicle-level observations collected through field observations and CCTV-based traffic analysis.

The dataset includes traffic-related information such as:

- Date
- Time
- Timestamp
- Vehicle speed
- Vehicle class/type
- Direction
- Heading
- Other traffic-related attributes

The data was cleaned, synchronized, and structured before being used for descriptive analysis and ARIMA-based forecasting.

> **Note:** The original/raw dataset is not included in this repository.

---

## 🛠️ Technologies Used

**Programming Language**
- Python

**Python Libraries**
- Pandas
- NumPy
- Matplotlib
- SciPy
- Statsmodels
- Scikit-learn
- OpenPyXL

**Statistical / Analytical Techniques**
- Data Cleaning
- Data Validation
- Descriptive Statistics
- Exploratory Data Analysis
- Aggregation
- Time-Series Analysis
- Augmented Dickey-Fuller Test
- Autocorrelation Function (ACF)
- Partial Autocorrelation Function (PACF)
- ARIMA
- AIC/BIC Model Comparison
- Ljung-Box Test
- Shapiro-Wilk Test
- Holdout Validation
- MAE, RMSE, MAPE

---

## 🔎 Analysis Workflow

### Block 1 — Data Loading & Initial Exploration
The first stage loads the traffic dataset and examines its structure. The initial exploration focuses on:
- Dataset dimensions
- Column names
- Data types
- Available traffic attributes
- Basic data characteristics

This establishes the foundation for the remaining analysis.

### Block 2 — Data Cleaning & Preprocessing
The raw observations are cleaned and prepared for analysis. The preprocessing stage includes:
- Data-type conversion
- Timestamp preparation
- Missing-value handling
- Standardization of relevant fields
- Preparation of vehicle-level observations

The cleaned data is then used for subsequent traffic and speed analysis.

### Block 3 — Data Quality & Duplicate Validation
The cleaned dataset is checked for:
- Missing values
- Duplicate records
- Invalid observations
- Data consistency

This ensures that downstream statistical analysis is performed on a validated dataset.

### Block 4 — Traffic Volume Analysis
Traffic observations are aggregated to understand traffic volume across the observed period. The analysis examines:
- Daily traffic volume
- Hourly traffic volume
- Traffic variation over time
- High- and low-volume periods

This provides the initial understanding of traffic behaviour before forecasting.

### Block 5 — Descriptive Statistics
Descriptive statistics are used to summarize the traffic-volume distribution. Key measures include:
- Mean, Median
- Minimum, Maximum
- Standard deviation
- Observation count

These statistics help identify the central tendency and variability of traffic volume.

### Block 6 — Traffic Pattern Analysis
Traffic volume is analyzed across different time periods to identify temporal patterns. The analysis examines hourly traffic behaviour and helps identify periods of relatively higher and lower traffic activity. These observations provide context for the later time-series modelling stage.

---

## 🚗 Speed Analysis

### Block 7 — Vehicle Speed Analysis
Vehicle speed was analyzed independently to understand the movement characteristics of the observed traffic.

**Key Results**

| Metric | Result |
|---|---|
| Valid speed observations | 57,422 |
| Mean speed | 48.79 km/h |
| Median speed | 48.72 km/h |
| Standard deviation | 11.08 km/h |
| Minimum speed | 1.64 km/h |
| Maximum speed | 176.32 km/h |

The speed analysis also identified extreme observations and provided a broader understanding of vehicle movement within the observed traffic stream.

### Block 8 — Speed by Time & Vehicle Class
Vehicle speeds were compared across hour of day, vehicle class, and speed ranges.

**Speed by Hour**
- Highest average speed: **14:00 → 51.28 km/h**
- Lowest average speed: **08:00 → 43.21 km/h**

**Speed Range Distribution**
- 40–60 km/h → **64.81%** (majority of vehicles)
- 20–40 km/h → **19.31%**

**High-Speed Observations** (≥ 80 km/h threshold)
- Vehicles ≥ 80 km/h: **190**
- Percentage: **0.33%**

**Extreme Observations**
- Highest recorded speed: **176.32 km/h** — 2025-02-07 14:56:27 — Class: Bicycle
- Lowest recorded speed: **1.64 km/h** — 2025-02-06 14:58:07 — Class: 2 Wheeler

These extreme observations were retained as part of the descriptive analysis rather than automatically removed.

---

## ⏱️ Time-Series Modelling

### Block 9 — Time-Series Preparation
The vehicle-level observations were transformed into an hourly traffic-volume time series.

**Time-Series Summary**
```
Observed hourly observations : 41
Expected hourly periods      : 78
Missing hourly periods       : 37

Start : 2025-02-04 12:00:00
End   : 2025-02-07 17:00:00
```

**Traffic Volume Statistics**
```
Mean   : 1400.54 vehicles/hour
Median : 1515 vehicles/hour
Min    : 5 vehicles/hour
Max    : 2047 vehicles/hour
```

The analysis identified 37 missing hourly periods within the expected 78-hour interval. This limitation is important when interpreting the ARIMA results.

### 📈 Block 10 — Stationarity Analysis
Before fitting ARIMA models, the traffic-volume series was tested for stationarity using the Augmented Dickey-Fuller (ADF) test.

**Original Series**
```
ADF Statistic : -5.2049
p-value       : 0.0000
```

Since p-value < 0.05, the null hypothesis of a unit root was rejected. Therefore, the original traffic-volume series was considered **stationary**.

**Final Differencing Order:** `d = 0` — no differencing was required for the final ARIMA modelling.

### 📊 Block 11 — ACF & PACF Analysis
The Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) were examined to understand the temporal dependence structure of the stationary traffic series.

Approximate 95% confidence limit: **±0.3061**

No ACF or PACF lag exceeded the approximate significance threshold. Instead of selecting ARIMA parameters from a single visual interpretation, multiple candidate models were fitted and statistically compared.

**Candidate Models**
- ARIMA(0,0,0), ARIMA(1,0,0), ARIMA(0,0,1), ARIMA(1,0,1), ARIMA(2,0,0), ARIMA(0,0,2), ARIMA(2,0,1), ARIMA(1,0,2), ARIMA(2,0,2)

---

## 🤖 Block 12 — ARIMA Model Fitting & Comparison

All candidate ARIMA models were fitted to the traffic-volume series and compared using AIC, BIC, and Log Likelihood.

**Model Comparison**

| Model | AIC | BIC |
|---|---|---|
| ARIMA(2,0,1) | 627.634 | 636.202 |
| ARIMA(1,0,2) | 628.482 | 637.049 |
| ARIMA(2,0,2) | 629.628 | 639.909 |
| ARIMA(0,0,2) | 629.961 | 636.815 |
| ARIMA(0,0,0) | 630.378 | 633.805 |
| ARIMA(0,0,1) | 631.460 | 636.600 |
| ARIMA(2,0,0) | 631.720 | 638.574 |
| ARIMA(1,0,0) | 631.820 | 636.961 |
| ARIMA(1,0,1) | 633.122 | 639.977 |

**Selected Model: ARIMA(2,0,1)** — lowest AIC = 627.634

The BIC-selected model was ARIMA(0,0,0) (BIC = 633.805). However, ARIMA(2,0,1) was selected for the forecasting workflow based on the minimum AIC and was subsequently subjected to residual diagnostics and holdout evaluation.

### 🧪 Why ARIMA(2,0,1)?
The final model was not selected solely from the ACF/PACF plots. The selection process followed:

```
Stationarity Testing
        ↓
d = 0
        ↓
Generate Candidate ARIMA Models
        ↓
Fit All Candidate Models
        ↓
Compare AIC/BIC
        ↓
Select Lowest-AIC Model
        ↓
Residual Diagnostics
        ↓
Holdout Evaluation
```

ARIMA(2,0,1) had the lowest AIC among the tested candidates. The model was then validated rather than being accepted solely because it had the lowest information criterion.

---

## 🔬 Block 13 — Residual Diagnostics & Model Validation

The selected ARIMA(2,0,1) model was evaluated using residual diagnostics.

**Residual Statistics**
```
Mean residual : -66.7394
Std residual  : 460.5410
Minimum       : -1387.7079
Maximum       : 822.0795
```

**Ljung-Box Test**
```
Q statistic : 2.2547
p-value     : 0.9940
```
The high p-value indicates that the residuals did not show significant remaining autocorrelation.

**Shapiro-Wilk Test**
```
Statistic : 0.9556
p-value   : 0.1105
```
The residuals were approximately normally distributed based on the test.

**Diagnostic Conclusion**
- ✓ Residual autocorrelation check passed
- ✓ Residual normality check passed

These results indicate that the selected model adequately captured the main temporal structure present in the observed series.

---

## 🔮 Block 14 — 24-Hour Traffic Volume Forecast

The selected ARIMA(2,0,1) model was used to generate a 24-hour traffic-volume forecast.

**Forecast Period**
```
Last observed time : 2025-02-07 17:00:00
Forecast start     : 2025-02-07 18:00:00
Forecast end       : 2025-02-08 17:00:00
Forecast horizon   : 24 hours
```

**Forecast Summary**

| Metric | Forecast |
|---|---|
| Average predicted traffic | 1,425.79 vehicles/hour |
| Minimum predicted traffic | 1,265.55 vehicles/hour |
| Maximum predicted traffic | 1,469.88 vehicles/hour |

**Predicted Peak:** 2025-02-07 21:00 → 1,469.88 vehicles/hour
**Lowest Forecast:** 2025-02-07 18:00 → 1,265.55 vehicles/hour

The forecast initially increases after 18:00 and reaches its predicted maximum around 21:00 before gradually stabilizing around approximately 1,430 vehicles/hour. The forecast also includes confidence intervals, which become important when interpreting uncertainty in the predictions.

---

## 📏 Block 15 — Forecast Evaluation

A holdout evaluation was performed to assess how well the ARIMA(2,0,1) model predicts unseen observations.

**Train/Test Split**
```
Total observations : 41
Training : 33 observations
Testing  : 8 observations
```

**Training Period:** 2025-02-04 12:00:00 → 2025-02-07 09:00:00
**Testing Period:** 2025-02-07 10:00:00 → 2025-02-07 17:00:00

**Evaluation Metrics**

| Metric | Result |
|---|---|
| MAE | 335.20 vehicles |
| RMSE | 385.89 vehicles |
| MAPE | 22.61% |
| Approx. Accuracy | 77.39% |
| Mean Forecast Error | -34.59 vehicles |

The model achieved an approximate forecast accuracy of **77.39%** on the eight-observation holdout period. The relatively small test set means that this evaluation should be interpreted cautiously.

---

## 💡 Block 16 — Final Traffic Volume Insights

**Historical Traffic**
```
Mean traffic volume   : 1400.54 vehicles/hour
Median traffic volume : 1515.00 vehicles/hour
Minimum               : 5 vehicles/hour
Maximum               : 2047 vehicles/hour
```
The mean being below the median indicates that relatively low-volume observations influenced the distribution.

**Stationarity**
The ADF test indicated that the original traffic-volume series was stationary (p-value = 0.0000), so ARIMA differencing order `d = 0`.

**Final Model:** ARIMA(2,0,1) — selected based on lowest AIC = 627.634. The model subsequently passed the primary residual diagnostic checks.

**Model Validation**
```
Ljung-Box p-value    : 0.9940
Shapiro-Wilk p-value : 0.1105
```
The residuals showed no significant autocorrelation and approximate normality.

**Forecast Performance**
```
MAE  = 335.20 vehicles
RMSE = 385.89 vehicles
MAPE = 22.61%
```
Approximate holdout accuracy: **77.39%**

---

## 📌 Key Project Findings

**1. Traffic Volume**
Traffic volume varied substantially during the observed period, ranging from 5 to 2,047 vehicles/hour. The historical average was approximately 1,401 vehicles/hour.

**2. Vehicle Speed**
The average observed vehicle speed was 48.79 km/h, with a median of 48.72 km/h. Most vehicles were concentrated in the 40–60 km/h speed range.

**3. High-Speed Vehicles**
Only 0.33% of observations were at or above 80 km/h.

**4. Stationarity**
The traffic series was stationary according to the ADF test. Therefore, `d = 0` was used in the final ARIMA models.

**5. Model Selection**
Among the tested candidate models, ARIMA(2,0,1) achieved the lowest AIC.

**6. Residual Diagnostics**
The selected model passed the primary residual checks (Ljung-Box p-value = 0.9940, Shapiro-Wilk p-value = 0.1105).

**7. Forecast**
The 24-hour forecast predicted traffic broadly around 1,400–1,470 vehicles/hour after the initial forecast period. The predicted peak was 1,469.88 vehicles/hour at 21:00.

**8. Forecast Accuracy**
Holdout testing produced MAE = 335.20 vehicles, RMSE = 385.89 vehicles, MAPE = 22.61%. This indicates that ARIMA provides a useful statistical baseline, but the model still has meaningful prediction error.

---

## ⚠️ Limitations

The results should be interpreted in the context of the available data.

**Limited Time-Series Length**
Only 41 observed hourly observations were available for ARIMA modelling.

**Missing Hourly Periods**
The expected time interval contained 78 hourly periods, but only 41 periods were observed. Therefore, 37 hourly periods were missing.

**Small Holdout Dataset**
Only 8 observations were available for holdout evaluation. Therefore, the reported forecast performance should not be interpreted as a definitive estimate of long-term model accuracy.

**Moderate Forecast Error**
The MAPE was 22.61%, indicating moderate prediction error.

**Limited Seasonal Analysis**
The available time period is too short to reliably model longer-term seasonal behaviour such as weekly or monthly traffic patterns.

**Forecast Uncertainty**
The forecast confidence intervals were relatively wide, reflecting uncertainty associated with the limited historical data.

---

## 🚀 Future Scope

The project can be extended by:
- Collecting longer continuous traffic histories
- Reducing missing hourly periods
- Incorporating weather conditions
- Incorporating road and traffic-control information
- Including holidays and special events
- Investigating seasonal models such as SARIMA
- Comparing ARIMA with machine-learning models
- Comparing ARIMA with LSTM-based forecasting
- Including external explanatory variables
- Building a Power BI traffic analytics dashboard
- Creating an automated forecasting pipeline
- Developing a production-ready traffic prediction system

A longer and more complete historical dataset would allow more reliable seasonal analysis and stronger model validation.

---

## 🎓 Learning Outcomes

This project provided practical experience in:

**Data Analytics**
- Data cleaning, data validation, missing-value analysis, duplicate detection, data aggregation, exploratory data analysis, descriptive statistics

**Traffic Analytics**
- Traffic volume analysis, hourly traffic analysis, vehicle-speed analysis, vehicle-class analysis, speed distribution analysis, identification of extreme observations

**Time-Series Analysis**
- Datetime handling, time-series indexing, resampling, missing-period analysis, stationarity testing, ADF test, ACF, PACF, ARIMA

**Model Development**
- Candidate model generation, ARIMA parameter selection, AIC/BIC comparison, model fitting, residual diagnostics, Ljung-Box testing, Shapiro-Wilk testing

**Model Evaluation**
- Train/test splitting, holdout forecasting, MAE, RMSE, MAPE, forecast accuracy interpretation

**Business / Engineering Interpretation**
- Translating statistical results into traffic insights, identifying model limitations, communicating uncertainty, understanding the role of forecasting in transportation planning

---

## 📚 Project Methodology Summary

```
1. Load raw traffic observations
              ↓
2. Inspect dataset structure
              ↓
3. Clean and validate data
              ↓
4. Analyze missing values and duplicates
              ↓
5. Perform descriptive statistics
              ↓
6. Analyze traffic volume patterns
              ↓
7. Analyze vehicle speeds
              ↓
8. Analyze speed by hour and vehicle class
              ↓
9. Aggregate traffic into hourly time series
              ↓
10. Check stationarity using ADF
              ↓
11. Determine differencing order
              ↓
12. Analyze ACF and PACF
              ↓
13. Fit candidate ARIMA models
              ↓
14. Compare models using AIC/BIC
              ↓
15. Select ARIMA(2,0,1)
              ↓
16. Perform residual diagnostics
              ↓
17. Generate 24-hour forecast
              ↓
18. Perform holdout evaluation
              ↓
19. Interpret results and limitations
```

---

## ▶️ How to Run the Project

**1. Clone the repository**
```bash
git clone <your-repository-url>
```

**2. Navigate to the project**
```bash
cd traffic-volume-prediction-arima
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Open the notebook**
```bash
jupyter notebook Traffic_Volume_Prediction.ipynb
```

**5. Run the notebook sequentially**

Run the blocks in order from Block 1 → Block 16. The later ARIMA modelling stages depend on the cleaned and prepared data generated in the earlier blocks.

---

## 📌 Important Note About the Dataset

The raw traffic dataset is intentionally not included in the repository.

If the notebook is being shared publicly, ensure that the dataset does not contain personally identifiable information, confidential information, or restricted organizational data. The notebook documents the complete analytical workflow while keeping the original dataset separate.

---

## 🏁 Final Conclusion

This project demonstrates a complete end-to-end workflow for traffic-volume analysis and short-term forecasting.

The analysis began with vehicle-level traffic observations and progressed through data cleaning, validation, descriptive statistics, traffic-volume analysis, speed analysis, time-series preparation, stationarity testing, ACF/PACF analysis, ARIMA model comparison, residual diagnostics, forecasting, and holdout evaluation.

The observed traffic-volume series was found to be stationary, resulting in a differencing order of `d = 0`. Among the evaluated candidate models, ARIMA(2,0,1) achieved the lowest AIC and was selected for the forecasting workflow. The model passed the primary residual diagnostic checks and produced a 24-hour traffic-volume forecast.

Holdout evaluation produced MAE = 335.20 vehicles, RMSE = 385.89 vehicles, MAPE = 22.61%, with an approximate accuracy of **77.39%**.

The project demonstrates that ARIMA can serve as an interpretable statistical baseline for short-term traffic-volume forecasting. However, the limited number of observed hourly records and the presence of missing periods restrict the reliability and generalizability of the current model.

A longer and more complete traffic history, combined with additional explanatory variables and seasonal modelling techniques, would provide a stronger foundation for operational traffic forecasting.

---

## 👩‍💻 Project Focus

Traffic Analytics | Descriptive Statistics | Time-Series Analysis | ARIMA Forecasting | Python | Statistical Modelling | Transportation Analytics
