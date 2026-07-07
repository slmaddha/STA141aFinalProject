#  Wildfire Risk Prediction using Machine Learning

Predicting significant wildfire occurrence one month in advance using spatio-temporal machine learning models.

---

## Overview

This project develops a machine learning pipeline to predict the probability of a significant wildfire occurring in a specific geographic region during the following month.

Historical wildfire records from across the United States were transformed into a spatial-temporal panel dataset, allowing wildfire prediction to be framed as a binary classification problem. Multiple machine learning models were evaluated using rolling time-aware cross-validation to compare predictive performance.

The project additionally includes a California-focused analysis using Random Forest to better understand which variables contribute most to wildfire occurrence in one of the country's most fire-prone regions.

---

##  Motivation

Wildfires have become increasingly frequent and destructive throughout the United States, causing billions of dollars in damages and significant environmental impacts each year.

Growing up in Southern California, I experienced the 2018 Woolsey Fire firsthand when my family was forced to evacuate. That experience inspired me to explore whether historical wildfire data alone could be used to predict future wildfire risk and identify the strongest contributing factors.

---

##  Dataset

**Source**

- Kaggle – 1.88 Million US Wildfires Dataset
- Approximately 24 years of wildfire records (1992–2015)

The dataset includes:

- Latitude
- Longitude
- Discovery Date
- Fire Size
- State
- Cause of Fire

For this project:

- A subset of approximately **100,000 wildfire records** was used.
- Only **notable wildfires (≥300 acres)** were retained.
- This resulted in **1,381 significant wildfire events** used for modeling.

---

#  Data Pipeline

```text
Raw Wildfire Records
        │
        ▼
Filter Significant Fires
(Fire Size ≥ 300 acres)
        │
        ▼
Spatial Grid Creation
(1° × 1° Latitude/Longitude)
        │
        ▼
Monthly Panel Dataset
(Grid Cell × Month)
        │
        ▼
Feature Engineering
        │
        ▼
Machine Learning Models
        │
        ▼
Time-Aware Cross Validation
        │
        ▼
Model Evaluation
```

---

#  Data Preprocessing

The wildfire event data was transformed into a **grid-by-month panel dataset** where each observation represents:

- Geographic location
- Calendar month
- Whether a notable wildfire occurred during the following month

The final modeling dataset contained:

| Property | Value |
|-----------|-------|
| Observations | 147,576 |
| Years | 1992–2015 |
| Spatial Resolution | 1° × 1° Grid |
| Temporal Resolution | Monthly |
| Positive Class | 0.89% |

Because wildfire occurrence is extremely rare, the resulting dataset is highly imbalanced.

---

#  Feature Engineering

Several predictive variables were constructed from the historical wildfire records.

### Spatial Features

- Latitude
- Longitude
- State

### Seasonal Features

To capture yearly seasonality:

- Month (sine encoding)
- Month (cosine encoding)

### Historical Fire Features

- Previous fire count (12 months)
- Rolling fire count (3 months)
- Rolling fire count (12 months)
- Rolling burned acreage (3 months)
- Rolling burned acreage (12 months)

---

# Exploratory Data Analysis

Exploratory analysis revealed several important trends.

### Geographic Patterns

Wildfire occurrence was heavily concentrated in:

- California
- Oregon
- Washington
- Arizona
- New Mexico

These findings demonstrated that geographic location is one of the strongest predictors of wildfire risk.

---

### Seasonal Patterns

Wildfire probability follows a strong annual cycle.

Lowest risk:

- December
- January
- February

Highest risk:

- May
- June
- July

Because of this cyclical behavior, month was encoded using sine and cosine transformations rather than treating months as simple integers.

---

# Machine Learning Models

Three supervised learning models were evaluated.

## 1. Logistic Regression

Baseline binary classification model using all engineered predictors.

Advantages

- Fast
- Interpretable
- Strong baseline

---

## 2. XGBoost

Gradient boosted decision trees capable of learning nonlinear relationships.

Hyperparameters:

- Learning Rate: 0.1
- Max Depth: 8
- Trees: 200
- Subsample: 0.8
- Column Sample: 0.8

Advantages

- Handles nonlinear relationships
- Built-in regularization
- Robust to missing values

---

## 3. Neural Network

Single hidden-layer feedforward neural network.

Architecture

- 5 input features
- 8 hidden neurons
- Sigmoid activation
- L2 regularization
- 150 training iterations

---

# Model Validation

Instead of using a random train-test split, this project uses **rolling time-aware cross-validation**.

```text
Train: 1992–1994
Test : 1995

Train: 1992–1995
Test : 1996

Train: 1992–1996
Test : 1997

...

Train: 1992–2013
Test : 2014
```

This evaluation strategy prevents future observations from leaking into the training data and better reflects real-world forecasting.

---

# Results

## Average Model Performance

| Model | Mean AUC |
|---------|---------:|
| Logistic Regression | 0.690 |
| Neural Network | 0.740 |
| **XGBoost** | **0.755** |

---

## Key Findings

 XGBoost consistently achieved the highest predictive performance.

 Spatial location was one of the strongest predictors.

 Seasonal trends significantly improved prediction accuracy.

 Historical wildfire activity strongly influenced future wildfire occurrence.

---

#  California Case Study

A supplementary Random Forest model was developed using approximately **10,089 California wildfire records**.

The purpose of this analysis was to better understand variable importance within California specifically.

### Most Important Variables

| Variable | Importance |
|-----------|-----------:|
| Latitude | 36.29 |
| Land Ownership | 34.85 |
| Longitude | 32.77 |
| Year | 26.90 |
| Fire Cause | 20.89 |
| Month | 16.52 |

The analysis showed that **geographic location** and **land ownership** were the strongest predictors of wildfire occurrence within California.

---

#  Repository Structure

```text
wildfire-risk-prediction/
│
├── README.md
├── data/
├── figures/
├── notebooks/
│   ├── 01_preprocessing.Rmd
│   ├── 02_eda.Rmd
│   ├── 03_modeling.Rmd
│   └── 04_california_analysis.Rmd
├── src/
│   ├── preprocessing.R
│   ├── feature_engineering.R
│   ├── modeling.R
│   └── evaluation.R
├── requirements.txt
└── LICENSE
```

---

# Technologies Used

- R
- tidyverse
- ggplot2
- XGBoost
- nnet
- randomForest
- dplyr
- lubridate

---

# Future Improvements

Potential extensions of this project include:

- Integrating weather variables (temperature, humidity, wind speed)
- Incorporating satellite imagery
- Using finer spatial grid resolutions
- Including vegetation and drought indices
- Applying deep learning models with spatial-temporal architectures
- Deploying the model as an interactive wildfire risk dashboard

---

#  Key Takeaways

- Successfully transformed wildfire event data into a spatial-temporal prediction problem.
- Engineered historical and seasonal features to improve predictive performance.
- Compared interpretable statistical models with modern machine learning techniques.
- Demonstrated that nonlinear models outperform logistic regression for wildfire prediction.
- Highlighted the importance of geographic location and historical wildfire activity in forecasting future wildfire events.

---

#  Author

**Shreya Maddhali**

Computer Science, University of California, Davis

**Course:** STA 141A – Statistical Computing

---

 
