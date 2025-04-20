# Descriptive Analytics

A comprehensive overview of key concepts and techniques in descriptive and diagnostic analytics, covering modeling causality, clustering, data preparation, dimensionality reduction, and statistical tests.

---

## 1. Modeling & Causality

### Why Model?

- Gain insights into key variables and their causes and effects
- Construct reasonable arguments about why events can or cannot occur based on the model
- Make qualitative or quantitative predictions about the future

### Cause and Effect

- **David Hume**: Causation is a learned habit of mind—observing conjunctions can mislead (beware spurious correlations). *Highlights how we often mistake consistent associations for true cause-effect relationships, underscoring the need to distinguish correlation from causation.*
- **Herbert Simon’s Principle**: Complex social outcomes emerge from simple actors adapting to environments. *Illustrates that macro-level social patterns can be understood as the aggregate result of individual agents’ localized decisions and interactions.*

### Causality in Practice

- Equations alone are insufficient; domain knowledge and causal reasoning are required.
- **Experiments** are the gold standard but often infeasible in social science, so we approximate with correlational data and computational simulations.

---

## 2. Cluster Analysis

Clustering identifies subgroups of similar observations in an unsupervised fashion (no response variable). This problem is NP‑Hard; heuristic algorithms (e.g., K‑Means) are commonly used.

### General Common Steps

1. Choose appropriate attributes
2. Scale (standardize) the data
3. Screen for outliers
4. Calculate distances (dissimilarity matrix)
5. Select a clustering algorithm (e.g., K‑Means, hierarchical)
6. Obtain one or more cluster solutions
7. Determine the number of clusters present
8. Obtain final clustering solution
9. Visualize the results
10. Interpret the clusters
11. Validate the clusters

### Distance Measures

- **Euclidean distance**: straight‑line (L2 norm)
- **Manhattan distance**: grid‑based path (L1 norm)
- **Correlation-based distances**:
  - **Pearson correlation distance**: quantifies linear relationships by transforming the Pearson correlation coefficient (1 − r), assuming normality—ideal for straight‑line associations but sensitive to outliers.
  - **Spearman correlation distance**: uses rank‑based correlation (1 − ρ), capturing monotonic relationships without assuming normality—robust to outliers and non‑linear but monotonic trends.

### K‑Means Clustering

**Algorithm:**

1. Initialize K centroids (random or seeded)
2. Assign each data point to the nearest centroid
3. Recalculate centroids as the mean of assigned points
4. Repeat steps 2–3 until no reassignments or max iterations reached

**R implementation:** `kmeans(df, centers = K, nstart = 25)`

**Key outputs (in R):**

- `cluster`: cluster assignment for each observation
- `centers`: coordinates of cluster centroids
- `totss`: total sum of squares (overall variance)
- `withinss`: within-cluster sum of squares for each individual cluster
- `tot.withinss`: total within-cluster sum of squares across all clusters
- `betweenss`: between-cluster sum of squares (variance explained by clustering)
- `size`: number of observations in each cluster

**Computing cluster means:**

```r
aggregate(df, by = list(cluster = km$res$cluster), mean)
```

*Returns the centroid of each cluster (i.e., the average value of each feature/variable for observations in that cluster), which you can use to profile and interpret the characteristics of each cluster.*

**Visualization:**

```r
factoextra::fviz_cluster(km, data = df)
```

**Quality metrics:**

- **Inertia** (total within‑cluster sum of squares)
- **Silhouette score** (cohesion vs. separation)
- **BSS/TSS ratio** = (Between SS / Total SS) × 100% (higher is better)\
  *Tells us the percentage of total variance captured by between-cluster differences; values near 100% indicate strong cluster separation, while values near 0% indicate poor separation. A ratio over 100% (BSS > TSS) suggests anomalies or calculation errors, whereas an unusually low ratio means within-cluster variance dominates and clusters may not be meaningful.*

### Finding a Good Number of Clusters

- Elbow method: plot total within‑groups SS vs. K and look for a bend.
- R functions: `wssplot(df, nc = 4, seed = 1234)`; `factoextra::fviz_nbclust(df, kmeans)`

### K‑Means: Issues & Best Practices

- Sensitive to initial centroids: run multiple starts or use smart seeding (e.g., farthest‑first).
- Must manually choose K; rule of thumb: √n observations but context matters.
- Real‑world data is noisy; outliers can distort clusters.

### Anscombe’s Quartet

Four datasets with nearly identical summary statistics but different distributions, illustrating the necessity of graphical analysis before modeling. Each set has eleven (x,y) points, highlighting how outliers and patterns can be obscured by statistics alone.

<img src="https://github.com/user-attachments/assets/3f04719b-4d7a-4743-8590-f85a36b8a6f6" 
     alt="output" 
     width="500" />

### Other Clustering Techniques

<img src="https://github.com/user-attachments/assets/98e4516e-692e-4489-b505-ffb6221061f6" 
     alt="output" 
     width="500" />

---

## 3. Data Preparation

### Cleaning & Standardization

- Remove or impute missing values (e.g., `df <- na.omit(df)` or statistical imputation).
- Handle outliers appropriately.
- Standardize variables (zero mean, unit variance) to ensure comparability (`df <- scale(df)`).

---

## 4. Dimensionality Reduction

Reducing dimensionality simplifies models, speeds computation, and aids visualization.

### Data Reduction Overview

- Goal: transform an n × p dataset into n × k (k < p) by selecting or combining attributes.
- Benefits: lower time/space complexity, simpler and more robust models, easy 2D/3D plots.

### Principal Component Analysis (PCA)

- Identifies orthogonal axes (principal components) that capture the most variance.
- **Procedure:** eigenvalue decomposition of the correlation/covariance matrix.
- **R implementation:** `principal(data, nfactors = k, rotate = "none")` in the psych package.

**Outputs:**

- **Loadings (PCx):** correlation of observed variables with principal components
- **SS (sum of eigenvalues):** variance explained by each component
- **ProportionVar:** proportion of total variance explained

**Scree plot:**

```r
scree(df, factors = TRUE)
```

Plot eigenvalues in descending order; retain components left of the elbow.

#### PCA Eigenvectors

- Each eigenvector defines a principal axis; associated eigenvalue gives its variance length.
- Feature vector = (eigenvectors selected) used to project original data.

### PCA Advantages & Disadvantages

- **Advantages:** handles multicollinearity; reduces noise
- **Disadvantages:** may bias estimates by dropping low-variance components; assumes linear relationships; outliers can distort axes.

### PCA: Practical Issues

- Linearity assumption may fail on complex manifolds.
- Selecting the number of components requires theory and variance thresholds.
- PCA focuses on variance, not necessarily predictive power.

---

## 5. Statistical Tests

### Chi‑Squared Test

- Evaluates discrepancy between observed and expected categorical frequencies.
- **Use case:** goodness‑of‑fit for binned data.
- **Limitations:** requires sufficiently large counts; applicable only to categorical distributions.

---

## 6. Observation

> “In the end, [predictive modeling] is not a substitute for intuition, but a complement.”\
> — Ian Ayres, *Supercrunchers*\
> *This emphasizes that data-driven models should augment, not replace, human judgment, blending rigorous analysis with expert insight.*

---

## 7. Discussion Questions and Answers

1. **Clustering on graphs:** How would you adapt clustering (e.g., K‑Means) to nodes with multiple attributes and edges? What preprocessing or distance measures are needed?\
   *Answer:* Combine node feature vectors (attributes) with structural metrics (e.g., degree, centrality) or use graph embeddings (e.g., node2vec) to capture both attributes and link structure, then apply clustering. Alternatively, use graph‑specific methods like spectral clustering or community detection that leverage adjacency directly.

2. **Descriptive vs. Diagnostic Analytics:**

   - Descriptive: What happened? (Event identification)
   - Diagnostic: Why did it happen? (Cause analysis)\
     *Answer:* Descriptive analytics summarizes past data into reports or visualizations; diagnostic analytics goes deeper to identify root causes by analyzing correlations, patterns, or causal models.

3. **Predictions:** What are the differences between qualitative predictions (descriptive categories) and quantitative predictions (numeric forecasts)?\
   *Answer:* Qualitative predictions classify outcomes into categories (e.g., high/medium/low risk), while quantitative predictions estimate continuous numerical values (e.g., sales volume, probability scores).  
