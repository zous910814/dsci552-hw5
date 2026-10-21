# DSCI 552 Homework 5

Name: Chihwei Huang

USC ID: 9265220160

This homework has two problems. Problem 1 builds decision trees on the Acute Inflammations data,
turns them into IF-THEN rules and prunes them with cost-complexity pruning. Problem 2 fits regression
models on the Communities and Crime data: least squares, ridge, the LASSO, PCR and an XGBoost model
with an L1 penalty.

## Files

* `README.md`: this file.
* `requirements.txt`: the Python packages and the versions used.
* `data/diagnosis.data` and `data/diagnosis.names`: the Acute Inflammations data and its description.
* `data/communities.data` and `data/communities.names`: the Communities and Crime data and its description.
* The `data` folder also has the KDnuggets article from footnote 2 as a PDF.
* `notebook/Huang_Chihwei_HW5.ipynb`: all answers, with the code, plots and written discussion.

In the notebook, each question starts with a markdown heading that has its number from the
assignment, such as 1b or 2h.

## How to run

```bash
python -m venv venv
venv/Scripts/activate
pip install -r requirements.txt
jupyter notebook notebook/Huang_Chihwei_HW5.ipynb
```

On macOS or Linux, use `source venv/bin/activate` for the second line.

Then choose Run All in the Cell menu. The notebook reads the data with the relative path `../data`,
so it must run from the `notebook` folder. Jupyter uses this folder by default. A full run takes
3 to 4 minutes. Most of the time goes to the cross-validation for PCR and XGBoost.

## Notes on the data

* `diagnosis.data` is saved in UTF-16 and uses a comma as the decimal mark, for example `35,9`.
  The notebook reads it with `encoding="utf-16"` and `decimal=","`.
* `communities.data` has no header. The notebook takes the column names from the `@attribute`
  lines of `communities.names` and reads `?` as a missing value. It drops the 5 non-predictive
  columns: `state`, `county`, `community`, `communityname` and `fold`.
* Missing values are filled in with the median of each feature in the training set.
  22 LEMAS police features are missing for 84% of the communities.

## Results

| Model for Problem 2 | Test MSE |
| --- | --- |
| XGBoost with L1 penalty, α = 0.46 | 0.01573 |
| Ridge, λ = 3.14 | 0.01748 |
| LASSO, 74 variables | 0.01763 |
| LASSO with standardized features, 76 variables | 0.01771 |
| Least squares | 0.01797 |
| PCR, M = 95 | 0.01830 |

In Problem 1, the label powerset tree has 6 leaves and classifies all 120 patients correctly.
Cost-complexity pruning with cross-validation keeps this tree, with α = 0. Any pruning lowers
the accuracy to 84% or less.
