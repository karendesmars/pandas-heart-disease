# Heart Disease Classification

**Dataset:** [Heart Disease (Cleveland)](https://archive.ics.uci.edu/ml/datasets/heart+Disease), UCI Machine Learning Repository, 303 rows, 13 clinical features
**Tools:** Python, Pandas, scikit-learn (LogisticRegression, KNeighborsClassifier, RandomForestClassifier, RandomizedSearchCV, GridSearchCV)
**Goal:** Predict whether a patient has heart disease from clinical parameters.

---

## Project Structure

```
pandas-heart-disease/
├── data/
│   └── heart-disease.csv                      # Raw dataset (303 rows, 14 columns)
├── notebooks/
│   └── heart_disease_classification.ipynb     # Full classification workflow
└── README.md
```

---

## Approach

| # | Step | Focus |
|---|---|---|
| 1 | Data exploration | Load, check nulls, class balance, distributions by sex/age/chest pain type |
| 2 | Baseline models | Compare LogisticRegression, KNN, and RandomForest with default hyperparameters |
| 3 | Hyperparameter tuning | Manual tuning for KNN, RandomizedSearchCV for LogisticRegression and RandomForest, then GridSearchCV for LogisticRegression |
| 4 | Evaluation beyond accuracy | ROC curve, AUC, confusion matrix, classification report |
| 5 | Cross-validated metrics | Accuracy, precision, recall, f1-score averaged over 5 folds on the full dataset |
| 6 | Feature importance | LogisticRegression coefficients, one per clinical feature |

See `notebooks/heart_disease_classification.ipynb` for the full step-by-step workflow.

---

## Key Findings

- **Logistic Regression is the strongest baseline model**, ahead of Random Forest and clearly ahead of KNN: 88.5% accuracy vs 83.6% (Random Forest) and 68.9% (KNN) on the held-out test set, with default hyperparameters.
- **Tuning helps Random Forest but not Logistic Regression.** RandomizedSearchCV improves Random Forest from 83.6% to 86.9%. For Logistic Regression, both RandomizedSearchCV and GridSearchCV land back on the same 88.5% score as the default model. This shows that tuning does not always improve a model: here, the default Logistic Regression hyperparameters were already close to optimal for this dataset.
- **The final tuned model reaches 88.5% test accuracy** (precision 0.89, recall 0.88, macro average), below the 95% target set in the Evaluation section of the notebook. This target was not reached with the current approach; the notebook's own Experimentation section lists next steps (more data, other model families such as XGBoost or CatBoost).
- **Cross-validated recall (92.7%) is notably higher than cross-validated precision (82.2%).** In practical terms, the model misses fewer real heart disease cases (fewer false negatives) at the cost of more false alarms (more false positives). For a screening context, this trade-off direction is reasonable: missing a real case is usually more costly than a false alarm that leads to further medical checks.
- **`sex`, `thal`, `cp`, `ca`, `exang`, `oldpeak`, and `slope` have the strongest coefficients** in the Logistic Regression model, while `age` and `chol` (cholesterol) have close to zero effect. This is counter to the common assumption that age and cholesterol are the main heart disease risk factors - in this specific dataset, other clinical features carry more weight once combined in the same model.
- **Caveat on the `sex` coefficient.** In this dataset, being male (`sex=1`) is associated with a *lower* predicted probability of heart disease, which goes against general population-level findings on heart disease risk. This is most likely a property of this specific dataset (303 patients referred for cardiac evaluation, not a general population sample) rather than a finding that generalizes. It should not be read as a medical conclusion.
