# 📄 Final Report: Smart Factory Energy Prediction

## 🎯 Objective

The goal of this project is to build a predictive model to estimate the **equipment energy consumption** of a smart factory based on sensor data collected across various zones and environmental conditions. This system aims to assist facility managers in reducing energy costs by providing actionable insights and accurate forecasts.

---

## 🧠 Approach to the Problem

### 1. Data Loading and Exploration
- Loaded the dataset containing **16,857 rows** and **29 columns**.
- The target variable is `equipment_energy_consumption` (in Wh).
- Examined the structure and distribution of sensor readings, including:
  - Zone temperatures and humidity levels
  - Outdoor weather conditions
  - Lighting energy usage
  - Randomly generated variables

### 2. Missing Value Analysis
- Around **4.5–5.3% missing values** were found across multiple features.
- Handled missing data through imputation strategies (e.g., forward fill or interpolation).

### 3. Feature Engineering
- Extracted time-based features from `timestamp` (hour, sin/cos of hour, day of week).
- Created temporal features:
  - `lag_1`, `rolling_mean_3`, `rolling_std_3` to capture past energy patterns.
- Applied **polynomial interaction-only transformations (degree 3)**, resulting in a large feature space (~11,000+ features).
- Excluded `random_variable1` and `random_variable2` due to near-zero correlation with the target (-0.015 and -0.010 respectively).

### 4. Modeling & Evaluation
- Trained and evaluated multiple models:
  - **Linear Regression**
  - **Random Forest**
  - **Gradient Boosting**
  - **XGBoost**
- Performed **GridSearchCV** to tune Gradient Boosting parameters.

---

## 🔍 Key Insights from the Data

- Strong temporal patterns were found: `rolling_mean_3`, `lag_1`, and `hour_sin` were top predictors.
- **Zone8 and Zone6 temperature/humidity** significantly influenced equipment energy consumption.
- Lighting energy had a minor but noticeable impact.
- Mutual information and correlation analysis helped isolate impactful features and discard irrelevant ones.

---

## 📊 Model Performance Evaluation

| Model                   | R² Score | RMSE     | MAE     |
|------------------------|----------|----------|---------|
| Linear Regression       | 0.4410   | 115.51   | 45.70   |
| Random Forest           | 0.2365   | 134.99   | 47.23   |
| Gradient Boosting       | 0.3681   | 122.81   | 45.56   |
| XGBoost                 | 0.0616   | 149.66   | 52.11   |
| **Tuned Gradient Boosting** | **0.3921** | **120.46** | **45.41** |

**Best Model:** Tuned Gradient Boosting  
**Best Parameters:** `learning_rate=0.05`, `max_depth=3`, `n_estimators=100`

---

## ✅ Recommendations for Reducing Energy Consumption

1. **Leverage Time-of-Day Patterns**
   - Energy usage peaks during certain hours — suggest adjusting operations to distribute load during off-peak hours.

2. **Optimize Conditions in Influential Zones**
   - Improve climate control efficiency in **Zone 8** and **Zone 6**, as their temperature/humidity correlate with higher consumption.

3. **Lighting System Review**
   - Even though less impactful, unnecessary lighting usage adds up. Use motion-sensor systems or dimming strategies.

4. **Predictive Load Management**
   - Use rolling and lag features in a live model to predict high-energy periods and take preemptive actions (e.g., staggered machine startups).

5. **Model Simplification for Deployment**
   - Current model uses many engineered features. For production use, apply dimensionality reduction or retrain with the most important ~20 features for efficiency.

---

## 🔧 Limitations & Future Work

- The large feature space from polynomial interactions might lead to overfitting. Further validation on unseen data is required.
- Future enhancements may include:
  - Using **LSTM or other time-series models** to better capture sequential dependencies.
  - Incorporating **external operational data** (e.g., machine runtime logs, shift schedules) for better accuracy.

---

**Prepared by:**  
Ashay Vairat  
B.Tech CSE, 2026  
For SmartManufacture Inc. — May 2025
