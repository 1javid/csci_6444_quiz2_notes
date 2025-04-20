## Predictive Analytics Overview

**Definition (SAS):**

> Predictive analytics is the use of data, statistical algorithms and machine learning techniques to identify the likelihood of future outcomes based on historical data. The goal is to go beyond knowing what has happened to providing a best assessment of what will happen in the future. [[Source\]](https://www.sas.com/en_us/insights/analytics/predictive-analytics.html)\
> With a robust advanced analytic system, the “predictions” might be significantly better.

**Truth‑Telling (Michael Wu, Lithium Technologies):**

> The purpose of predictive analytics is *not* to tell you what will happen in the future. It can only forecast what *might* happen, because all predictive analytics are probabilistic in nature.\
> It doesn't necessarily forecast a single future but rather multiple possible futures, based on chosen algorithms and parameters.

---

## Types of Analytic Models

### Predictive Models

- Analyze data to identify possible outcomes (binary outputs like yes/no or probabilities across intervals)
- Different from business intelligence (which focuses on current state)
- **Examples:**
  - Customer churn probability based on complaint frequency, renewal date, competitor pricing
  - Clustering for targeted marketing audiences
  - Political campaign focus-group identification
  - Collaboration between car manufacturers and insurance firms

### Decision Models

- Map complex scenarios to possible decisions yielding net benefit
- Often part of *prescriptive analytics* (build and evaluate multiple plans/actions)
- Used for risk mitigation and situational understanding
- **Examples:**
  - Selecting Medicare Part D provider amid rising premiums
  - Celebrity marriage outcome prediction

### Association Models

- Predict which items/events co‑occur and the strength of their relationship (association rules)
- Causality is key: rules link conditions to conclusions (e.g., Milk ← Bread & Cheese)
- **Examples:**
  - Predicting additional service subscriptions
  - Upselling auto packages based on customer motivation

### Sentiment Analysis

- Infers sentiment (positive/negative/neutral) from text (tweets, blogs, emails)
- Input: plain text + polarity dictionary
- Output: sentiment score (e.g., –1 to +1)
- Predicts missing data (sentiment label), not future events [[Michael Wu, Lithium Technologies]]

---

## Data Preparation

### Factors in R

In R, a **factor** is a data structure used to handle **categorical variables**. Factors:

- Represent discrete categories (levels) rather than numeric values
- Can be created from character or integer vectors via `factor()`
- Are essential for modeling categorical predictors in functions like `lm()` or `glm()`

**What can and cannot be factors:**

- **Can be:** any discrete, non‑ordered categories (e.g., gender, region, product type)
- **Cannot be:** truly continuous measurements (e.g., temperature) unless discretized first

### Normalization Techniques

Normalization scales numeric data to a common range, improving algorithm performance and convergence.

- **Min–Max Scaling:** transforms data to a [0,1] range using
  \(x' = \frac{x - \min(x)}{\max(x) - \min(x)}\)

  - Sensitive to outliers (min/max values)

- **Z‑Score (Standard) Scaling:** centers data to mean 0 and unit variance using
  \(x' = \frac{x - \mu}{\sigma}\)

  - Less affected by outliers; preserves distribution shape

---

## K-Nearest Neighbors (K-NN)

### Strengths

1. Simple and effective
2. No assumptions about underlying data distribution
3. Fast training phase
4. Versatile: supports classification, regression, and nearest‑neighbor search

### Weaknesses

1. No explicit model; feature‑class relationships are opaque
2. Requires selecting an appropriate *k*
3. Slow classification on large datasets
4. Nominal/missing data need preprocessing
5. Computation cost grows with number of examples and features

*K-NN uses Euclidean distance to measure similarity.*

### Choosing *k*

- Balances **bias** and **variance**:
  - Small *k* → low bias, high variance (overfitting)

  - Large *k* → high bias, low variance (underfitting)

<img src="https://github.com/user-attachments/assets/13d25319-55bc-4d90-b723-2d74353eecf3" 
     alt="output" 
     width="500" />
     
### Improving KNN Results

1. Vary number of clusters if using clustering for label generation
2. Experiment with train/test split ratios
3. Tune package parameters (e.g., distance metric)
4. **Feature scaling**: apply min–max or z‑score normalization before KNN
5. Use cross‑validation and confusion matrices (false negatives/positives) to choose optimal *k*

---

## Regression Techniques

### Regression Overview

Analyzes relationships among variables, typically predicting a dependent variable from one or more independents.

### Linear Regression

- Models the linear relationship: \(y = a + bx + \varepsilon\)
- **Assumptions:** predictors are independent and normally distributed; errors have constant variance
- **Parameters:** slope (*b*) and intercept (*a*)

### Logistic Regression

**Purpose:** Predict binary outcomes (success/failure)\
**Key points:**

- Generalized linear model (David Cox, 1958)
- Uses logistic function to map inputs (–∞,+∞) to [0,1] probability
- Assumes normally distributed errors (mean 0, constant variance)

#### How It Works

1. Start with initial coefficient estimates
2. Apply **maximum likelihood estimation** to find coefficients maximizing goodness‑of‑fit
3. Iteratively update coefficients until convergence of log‑likelihood

#### Testing Results

- **Overall model fit:** Chi-square test vs. reduced (constant‑only) model; significance at α = 0.05 indicates better fit than null hypothesis
- **Parameter tests:** Likelihood ratio tests by removing one predictor at a time to assess individual contributions

#### Visual Indicators

- **Normality of residuals:** residuals should align on 45° line if errors are normal
- **Homoscedasticity:** random band around horizontal line in residuals vs. fitted values
- **Residuals vs. Leverage:** identifies outliers, high‑leverage points, and influential observations

---

## Probability Distributions

- **Normal Distribution:** continuous, defined by mean (μ) and variance (σ²); many phenomena approximate normality
- **Binomial Distribution:** probability of *k* successes in *n* Bernoulli trials; approximates normal for large *n* if not too skewed
- **Poisson Distribution:** probability of *k* events in fixed period given average rate λ; models skewed count data; negative binomial used when variance > mean

---

## Performance Measures for Classification Models

Let TP, FP, TN, FN denote true positives, false positives, true negatives, and false negatives.

- **Accuracy:** \(\frac{TP + TN}{TP + TN + FP + FN}\)
- **Error Rate:** \(\frac{FP + FN}{TP + TN + FP + FN}\)
- **Recall (Sensitivity):** \(\frac{TP}{TP + FN}\)
- **Specificity:** \(\frac{TN}{TN + FP}\)
- **Precision:** \(\frac{TP}{TP + FP}\)
- **F₁ Score:** \(2 \times \frac{Precision \times Recall}{Precision + Recall}\)

---

## Support Vector Machines (SVM)

- **Purpose:** Classification and regression via finding optimal separating hyperplane
- **Concepts:**
  - **Kernel function:** transforms data for better separability
  - **Support vectors:** data points closest to the hyperplane
  - **Margin (ρ):** width between classes; maximize margin for robustness

SVMs perform well on linearly separable data; fewer support vectors imply a simpler model (Ockham’s razor).

---

## Discussion Questions

1. **For what problems would regression be most appropriate? When should you use simulation?**  
   Regression is ideal for modeling continuous outcomes and quantifying relationships; use simulation when analytical models are impractical or to explore variability under uncertainty.

2. **What should you do if k‑means and k‑NN do not converge to reasonable clusters?**  
   Preprocess data (e.g., scale features, remove outliers), adjust k or initialization, try different distance metrics, or switch to more robust clustering/classification methods.

3. **After varying parameters for `lm()` and `glm()` and using `predict()`, how would you validate the ‘predictions’ against real-world data?**  
   Use train/test splits or cross-validation, then compare predictions to actual outcomes using RMSE for regression or accuracy and AUC for classification.

---

## Overtraining vs Overfitting

- **Overfitting:** The model captures noise and idiosyncrasies in the training data, resulting in poor performance on unseen data.
- **Generalization Bound:** The error on new data is bounded by the ratio of support vectors to training examples: ( # Support Vectors ) ⁄ ( # Training Examples ).
- **Occam’s Razor (Simplicity Principle):** Given equal training performance, simpler models with fewer support vectors or smoother decision boundaries tend to generalize better.

---

