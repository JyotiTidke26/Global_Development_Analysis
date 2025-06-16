# 🌍 Project Title: Global Development Analysis:
## A Data-Driven Analysis of Economic, Health, and Climate Progress (2000–2020)

This project investigates how countries have progressed in terms of sustainable development by examining a wide range of economic, health, and environmental indicators from 2000 to 2020. It applies modern machine learning techniques to:

- Cluster countries based on shared development trajectories.
- Predict development outcomes with high accuracy using XGBoost.
- Identify key drivers of progress using interpretable AI tools.
---
## Project Motivation
- **Multidimensional Focus**: Integrates economic, health, infrastructure, and environmental data to provide a holistic view of development.
- **Data-Driven Insights**: Identifies trends, outliers, and clusters of countries that share similar progress paths.
- **ML for Development Policy**: Uses clustering and regression to support targeted, evidence-based policymaking.
- **Open-Source Contribution**: Encourages transparency, reproducibility, and collaborative research.
---
## Dataset Overview
- **Source**: [Global Development Indicators (Kaggle)](https://www.kaggle.com/datasets/michaelmatta0/global-development-indicators-2000-2020/data)  
  Compiled from World Bank, WHO, UN, and climate data.
- **Time Frame**: Annual data from 2000 to 2020
- **Entities**: Countries, regions, and global aggregates
---
## Indicators Summary

| **Category**             | **Example Indicators**                                                                                   | **Description**                                                                                     |
|--------------------------|----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| **Economic**             | GDP per capita, Inflation, Unemployment, Population growth                                               | Captures macroeconomic performance and labor market dynamics.                                       |
| **Foreign Investment**   | FDI (% of GDP)                                                                                           | Reflects economic openness and investment climate.                                                   |
| **Environment & Energy** | CO₂ emissions, Energy use per capita, Renewable energy share                                             | Measures sustainability, environmental impact, and transition to green energy.                      |
| **Infrastructure**       | Electricity access, Internet usage, Mobile subscriptions                                                 | Indicates digital and physical infrastructure supporting economic activity.                         |
| **Health**               | Life expectancy, Child mortality, Hospital beds, Physicians per 1,000 people                             | Represents healthcare access, outcomes, and system capacity.                                         |
| **Education**            | Secondary school enrollment, Education-to-health ratio                                                   | Reflects investment in and access to education.                                                     |
| **Indexes**              | Human Development Index, Governance quality, Digital readiness, Climate vulnerability                    | Composite metrics summarizing broader development outcomes.                                         |
| **Demographic & Context**| Region, Income group, Is pandemic period, Currency unit, Population, Growth rate                         | Contextual metadata for stratified analysis.                                                        |
| **Time Features**        | Year, Years since 2000, Years since 1900                                                                  | Used for longitudinal modeling and trend analysis.                                                  |

---

## Project Workflow

### 1. Data Collection and Inspection
- Harmonized country names and classifications
- Separated country, region, and global aggregate entries
- Removed low-utility columns and ensured data type consistency

### 2. Exploratory Data Analysis (EDA)
- Analyzed intra- and inter-domain correlations (economic, health, environmental)
- Explored trends, variability, and shifts over the 20-year period

### 3. Preprocessing and Cleaning
- **Missing Values**:
  - Dropped columns with >30% missing data
  - Linear interpolation within countries
  - Region-level median imputation for remaining gaps

- **Outlier Detection**:
  - Applied IQR-based detection for Tier 2 and Tier 3 indicators
  - Retained flagged outliers to preserve real-world variability

- **Tiered Structure**:
  - Tier 2: Raw, direct indicators (e.g., GDP, emissions)
  - Tier 3: Composite or thematic metrics (e.g., HDI, resilience score)

---

## Predicting Human Development Index (HDI) Using XGBoost

### Why HDI?
The Human Development Index (HDI) is a globally recognized measure combining health, education, and income. It provides a single, interpretable score representing national progress.

### Why XGBoost?
- Strong performance on tabular data
- Handles multicollinearity and missing values effectively
- Provides model explainability via SHAP values and feature importance

---

### Model Training and Evaluation

- **Hyperparameter Tuning**: Performed via RandomizedSearchCV
  Best parameters:  
  - n_estimators: 300  
  - max_depth: 7  
  - learning_rate: 0.1  
  - subsample: 0.7  
  - colsample_bytree: 1.0  

- **Test Set Performance**:

| Metric   | Value  |
|----------|--------|
| MAE      | 0.0033 |
| RMSE     | 0.0062 |
| R² Score | 0.9995 |

- The model explains **99.95%** of the variance in HDI, indicating exceptional performance.

---

### Residual Analysis
- Residuals centered around zero with no pattern
- No evidence of heteroscedasticity or overfitting

### Interpretability with SHAP
- Identified top contributors to HDI such as life expectancy, school enrollment, CO₂ emissions per capita, and GDP per capita
- Helps identify country-specific development levers

---

## Country Clustering Using KMeans

### Objective
To group countries based on shared development profiles, using a combination of raw indicators and composite scores.

### Features Used

- **Tier 2**: GDP, life expectancy, internet usage, CO₂ emissions, etc.
- **Tier 3**: HDI, global resilience score, green transition index, etc.

---

### Preprocessing Pipeline
1. Converted all features to numeric
2. Interpolated missing values within countries; filled remaining using regional medians
3. Winsorized numeric features (5th–95th percentiles)
4. Applied RobustScaler to reduce outlier impact

---

### Clustering Methodology

- **Optimal K Determination**:
  - Elbow method showed diminishing returns after k=2
  - Silhouette scores confirmed k=2 was optimal (score = 0.5195)

- **Dimensionality Reduction**:
  - Applied PCA for visualization
  - Cluster separation was visually confirmed in 2D space

---

### Cluster Profiles

#### Cluster 0
- Lower GDP, life expectancy, internet usage, and HDI
- Mostly low- and lower-middle-income countries
- Examples: Zimbabwe, Chad, Papua New Guinea

#### Cluster 1
- Higher development across all indicators
- Includes high- and upper-middle-income countries
- Examples: USA, Germany, Brazil, India, South Africa

---

## Outputs

- clustered_countries.csv: Country-cluster assignments with PCA coordinates
- Cluster profiles and medians for each feature
- Lists of countries by cluster for review

---

## Next Steps and Future Enhancements

- **Interpretability**:
  - Use SHAP for clustering (via surrogate models) for deeper insights
- **Visualization**:
  - Add radar plots for comparing cluster profiles
  - Partial dependence plots for key HDI predictors
- **Clustering Alternatives**:
  - Explore DBSCAN for anomaly detection
  - Use Agglomerative Clustering to capture nested similarities
- **Temporal Modeling**:
  - Analyze how clusters and HDI prediction performance evolve over time
- **Model Robustness**:
  - Validate XGBoost on regional subsets for cross-context generalization
- **Feature Engineering**:
  - Investigate interaction terms and nonlinear effects via SHAP interaction values
- **Hyperparameter Optimization**:
  - Consider Bayesian optimization for deeper model tuning

---

## Clone the repository
git clone https://github.com/JyotiTidke26/Global_Development_Analysis.git
cd Global_Development_Analysis

## Create virtual environment
python -m venv venv
source venv/bin/activate

## Install dependencies
pip install -r requirements.txt

## License

This project is licensed under the MIT License.
You are free to use, modify, distribute, and build upon this project for personal or commercial purposes — with proper attribution.
This project is open-source and welcomes collaboration, feedback, and contributions.

---
