# Part B: Business Case Analysis

## B1. Problem Formulation

### (a) Problem Formulation

This problem can be formulated as a **supervised machine learning regression problem**.

**Target Variable:**
- Number of items sold (sales volume) per store per month

**Input Features:**
- Promotion type (Flat Discount, BOGO, Free Gift, etc.)
- Store location type (urban, semi-urban, rural)
- Store size
- Monthly footfall
- Local competition density
- Customer demographics
- Historical sales data
- Seasonality (month/time)

**ML Problem Type:**
- Regression (continuous target variable)

**Justification:**
The objective is to predict a numeric outcome (sales volume) based on multiple influencing factors.

### (b) Why Items Sold is Better than Revenue

- Revenue is affected by **pricing strategies** (discounts reduce revenue but may increase volume)
- Different promotions impact **price differently**, making revenue inconsistent
- Sales volume directly reflects **customer demand and response**

**Broader Principle:**
Choose a target variable that:
- Aligns with business objective  
- Is stable and interpretable  
- Minimizes external distortions  

### (c) Alternative Modelling Strategy

Instead of a single global model:

**1. Cluster-Based Models**
- Group stores (location, demographics)
- Train separate models

**2. Hierarchical Models**
- Capture global + store-level effects

**3. Store-Specific Features**
- Include store ID / embeddings

**Justification:**
Different stores behave differently; segmentation improves accuracy.

## B2. Data and EDA Strategy

### (a) Data Joining and Aggregation

**Joins:**
- Transactions + Store attributes (`store_id`)
- Promotions (`promotion_id`)
- Calendar (`date`)

**Final Grain:**
- **Store-Month level** (one row per store per month)

**Aggregations:**
- Total items sold
- Total revenue
- Number of transactions
- Average basket size
- Promotion indicators
- Footfall (avg/sum)
- Promotion days count

### (b) EDA Strategy

**1. Sales Distribution**
- Histogram / boxplot
- Detect skewness & outliers

**2. Promotion Effectiveness**
- Avg sales by promotion type
- Identify best-performing campaigns

**3. Time Series Analysis**
- Monthly trends & seasonality

**4. Store Comparison**
- Urban vs rural performance

**5. Correlation Analysis**
- Feature relationships (heatmap)

### (c) Handling Imbalance

**Problem:**
- 80% data without promotion → model bias

**Solutions:**
- Stratified sampling
- Oversampling / undersampling
- Weighted loss functions
- Evaluate separately on promotion cases

## B3. Model Evaluation and Deployment

### (a) Train-Test Split

**Approach:**
- Time-based split  
  - Train: First ~2–2.5 years  
  - Test: Last 6–12 months  

**Why not random split?**
- Breaks time order  
- Causes data leakage  

**Metrics:**
- RMSE  
- MAE  
- MAPE  

### (b) Explaining Recommendations

**Using Feature Importance:**

- December:
  - High seasonal demand → Loyalty Points effective

- March:
  - Lower demand → Flat Discount works better

**Communication:**
- Use SHAP / feature importance plots
- Explain impact of seasonality, store type, customer behavior

### (c) Deployment Strategy

**1. Save Model**
- Use pickle / joblib

**2. Prediction Pipeline**
- Monthly data ingestion
- Apply preprocessing
- Generate predictions

**3. Recommendation Logic**
- Predict sales for each promotion
- Select best promotion per store

**4. Monitoring**
- Track MAE / RMSE
- Compare predicted vs actual

**5. Retraining**
- Periodic (quarterly) or performance-based

**6. Automation**
- Use Airflow / cron jobs
