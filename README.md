# Interim Report for CS5228 Mini Project

## What We Have Done So Far

We have completed data preprocessing and exploratory data analysis (EDA) on the Telecom Customer Churn training subset (`churn-bigml-80.csv`, 2666 samples, 20 columns). Data loading and cleaning confirmed no missing values and no duplicate rows. We performed schema and sanity checks: categorical variables (International plan, Voice mail plan, Churn) conform to expected values, with the only minor deviation being that Churn is read as a boolean rather than a string. All count-like columns are non-negative. After inspecting distributions and box plots, we concluded that no outlier treatment was necessary.

For encoding, we applied one-hot encoding to Area code (408, 415, 510) and kept International plan and Voice mail plan as binary (later mapped to 0/1). We normalized only the continuous numerical features using `StandardScaler` fitted on the training data, then applied the same scaler to the test split, in line with the project requirements. Processed datasets were saved as `full_processed.csv`, `train_processed.csv`, and `test_processed.csv` (80–20 split: 2132 train, 534 test).

During EDA, we visualized the distribution of numerical features (using histograms and box plots) and analyzed correlations between features via scatter plots (e.g., calls vs. minutes, calls vs. charge, minutes vs. charge). Through this, we identified multicollinearity between the minutes and charge variables across the day, eve, night, and international groups. **However, we intentionally opted not to drop any of these features during the preprocessing stage. This decision is rooted in the understanding that high correlation does not necessarily imply redundancy, and multicollinearity does not always negatively affect overall model performance.** Finally, we examined the distribution of the target variable (Churn) and found that the class label is highly unbalanced (e.g., False ~2278, True ~388 in the training subset).

---

## What We Are Going to Do

**Unsupervised Learning (Customer Segmentation)**
We will use the training subset and ignore the churn label to perform customer segmentation. We will apply K-Means and DBSCAN as specified. For K-Means, we will determine the optimal number of clusters using the Elbow Method and/or Silhouette Score, fit the model, and interpret the usage patterns and similarities within each cluster. For DBSCAN, we will search for suitable hyperparameters (e.g., `eps`, `min_samples`) before profiling the resulting clusters. We will also analyze how churn (True/False) is distributed across these clusters to see if the unsupervised segments naturally align with churn risk.

**Supervised Learning (Churn Prediction)**
We will select and train three classification models. **To ensure our results are not compromised by the multicollinearity identified in EDA, we will prioritize models that are inherently robust to such issues. Furthermore, we will use cross-validation and rigorous performance comparisons to empirically verify whether dropping certain highly correlated fields is actually necessary.** **Additionally, to address the unbalanced class labels, we plan to implement techniques such as Label Smoothing or utilize robust algorithms like XGBoost to prevent our models from becoming biased toward the majority class.** We will evaluate model performance using accuracy, precision, recall, F1-score, and confusion matrices on both the training and test sets. Hyperparameters will be optimized using only the training subset (via grid search or random search), and we will briefly report on this optimization process. To satisfy the requirement of identifying the “most significant 2 and 3 features,” we will leverage feature importance metrics, correlation-with-churn analyses, or model coefficients.

**Generative AI Integration**
For the Generative AI stage, we will use a structured data generation Gen-AI method to create up to 500 synthetic samples from the training data. We will evaluate the quality of this generated data by:

1. Comparing histograms of columns D, E, G, H, and I with the original test set (`churn-bigml-20.csv`).
2. Testing our trained classifiers on the synthetic data and comparing the resulting accuracy, precision, and recall with the metrics achieved on the original test set (without retraining).

Finally, we will summarize our model interpretations and business insights (highlighting key drivers of churn and potential retention strategies), discuss the role of Gen-AI in the study, and compile our findings into the required final poster and conclusion.
