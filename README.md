# Walmart Sales Forecasting with Apache Spark

A Big Data group project that uses PySpark to process Walmart's weekly sales data across 45 stores, analyze sales performance, and predict weekly sales with machine learning.

## Problem

Walmart operates stores of different types and sizes, and each store produces a large volume of weekly sales data across many departments. Reading patterns by hand is slow, and store performance differs a lot between locations. This project builds a distributed data pipeline that answers three questions:

* Which stores, store types, and departments drive the most sales?
* How do holidays and seasons affect sales?
* Can weekly sales be predicted from store, department, calendar, and economic features?

## Dataset

[Walmart Sales Forecasting (Kaggle)](https://www.kaggle.com/datasets/anggundwilestari/walmart-sales-forecasting)

* train: 421,570 weekly sales records by store and department
* features: 8,190 rows of temperature, fuel price, CPI, unemployment, and 5 markdown columns
* stores: 45 stores with type (A, B, C) and size
* test: provided in the Kaggle dataset but not used in this analysis

Only `dataset/stores.csv` is included here because the other files are large. Download the rest from the Kaggle link above.

## Tech Stack

Python, Apache Spark (PySpark, Spark ML), Pandas, Matplotlib, Seaborn, Google Colab

## Pipeline

1. Environment setup: PySpark on Google Colab
2. Data loading: train, features, and stores loaded as Spark DataFrames
3. Data cleaning: removed 1,285 rows with negative weekly sales (returns or errors) and parsed dates, leaving 420,285 rows after joining
4. Feature engineering: year, month, week, and quarter, holiday flag, total markdown, store type encoding, and previous-week sales per store and department (window function)
5. Descriptive analytics: sales by store, type, department, month, and holiday
6. Modeling: Linear Regression and Random Forest Regressor with an 80/20 split
7. Visualization and export of the processed dataset to Parquet

## Key Findings

* Weekly sales are heavily right-skewed: mean 16,030 versus median 7,659, with a maximum of 693,099.
* Store type matters. Type A stores average 20,145 per record, Type B 12,288, and Type C 9,547.
* The top stores by total sales are 20, 4, 14, 13, and 2.
* Holiday weeks average 17,093 versus 15,950 for regular weeks.
* Departments 92, 95, and 38 have the highest average weekly sales.

!\[Top stores](images/1\_top\_stores.png)
!\[Monthly trend](images/2\_monthly\_trend.png)
!\[Sales by store type](images/3\_store\_type.png)

## Model Results

Random Forest (50 trees, max depth 10) outperformed Linear Regression on every metric.

* Linear Regression: RMSE 7,294.32, MAE 2,501.03, R² 0.8970
* Random Forest: RMSE 5,623.24, MAE 2,488.35, R² 0.9388

!\[Actual vs predicted](images/4\_actual\_vs\_predicted.png)
!\[Feature importance](images/5\_feature\_importance.png)

Previous-week sales dominates the Random Forest feature importance at about 81%, followed by department (11%) and store size (3%). This makes sense for weekly retail data, where last week is a strong predictor of this week.

## Limitations

* The train/test split is random (`randomSplit`), not chronological. Combined with the lag feature, the model can see neighboring weeks of the same store and department during training, so the reported R² is likely optimistic compared with real forecasting. A time-based split (for example, train on earlier dates and test on the latest weeks) would give a more honest estimate.
* Only two models were compared and no hyperparameter tuning was done.
* The Kaggle test file was not used for evaluation.

## Next Steps

* Switch to a time-based train/test split and re-evaluate
* Try gradient boosting (XGBoost or GBTRegressor) and cross-validation
* Build a Tableau or Power BI dashboard on the exported Parquet data

## How to Run

1. Open `walmart\_sales\_forecasting\_pyspark.ipynb` in Google Colab.
2. Download the dataset from Kaggle and upload `train`, `features`, and `stores` CSV files when the upload cell runs. The notebook expects the original Kaggle file names, for example `stores - Walmart Sales Forecast.csv`. `dataset/stores.csv` is renamed, so rename it back or edit the path.
3. Run the cells in order. Outputs are already saved in the notebook if you only want to read the results.

## My Contribution

This was a group project for a Big Data course. My part: Architecture Design, Infrastructure Decision Analysis, Documentation \& Report Writing, Project Coordination

