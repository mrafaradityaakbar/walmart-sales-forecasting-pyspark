# \# Walmart Sales Forecasting with Apache Spark

# 

# !\[Python](https://img.shields.io/badge/Python-3.x-blue?logo=python\&logoColor=white)

# !\[Apache Spark](https://img.shields.io/badge/Apache%20Spark-PySpark-E25A1C?logo=apachespark\&logoColor=white)

# !\[Platform](https://img.shields.io/badge/Platform-Google%20Colab-F9AB00?logo=googlecolab\&logoColor=white)

# 

# A Big Data group project that uses PySpark to process Walmart's weekly sales data across 45 stores, analyze sales performance, and predict weekly sales with machine learning.

# 

# \---

# 

# \## 📌 Problem

# 

# Walmart operates stores of different types and sizes, and each store produces a large volume of weekly sales data across many departments. Reading patterns by hand is slow, and store performance differs a lot between locations. This project builds a distributed data pipeline that answers three questions:

# 

# \* Which stores, store types, and departments drive the most sales?

# \* How do holidays and seasons affect sales?

# \* Can weekly sales be predicted from store, department, calendar, and economic features?

# 

# \## 📊 Dataset

# 

# Source: \[Walmart Sales Forecasting (Kaggle)](https://www.kaggle.com/datasets/anggundwilestari/walmart-sales-forecasting)

# 

# \* \*\*`train`\*\*: 421,570 weekly sales records by store and department

# \* \*\*`features`\*\*: 8,190 rows of temperature, fuel price, CPI, unemployment, and 5 markdown columns

# \* \*\*`stores`\*\*: 45 stores with type (`A`, `B`, `C`) and size

# \* \*\*`test`\*\*: Provided in the Kaggle dataset, but not used in this analysis

# 

# > All four CSV files are included in the `dataset/` folder.

# 

# \## 🛠️ Tech Stack

# 

# \* \*\*Big Data \& ML:\*\* Apache Spark (PySpark, Spark ML)

# \* \*\*Language \& Data Processing:\*\* Python, Pandas

# \* \*\*Visualization:\*\* Matplotlib, Seaborn

# \* \*\*Environment:\*\* Google Colab

# 

# \## ⚙️ Pipeline

# 

# 1\. \*\*Environment Setup:\*\* PySpark on Google Colab

# 2\. \*\*Data Loading:\*\* `train`, `features`, and `stores` loaded as Spark DataFrames

# 3\. \*\*Data Cleaning:\*\* Removed 1,285 rows with negative weekly sales (returns or errors) and parsed dates, leaving 420,285 rows after joining

# 4\. \*\*Feature Engineering:\*\* Extracted year, month, week, and quarter; added holiday flag, total markdown, store type encoding, and previous-week sales per store and department (using window functions)

# 5\. \*\*Descriptive Analytics:\*\* Sales breakdown by store, type, department, month, and holiday

# 6\. \*\*Modeling:\*\* Linear Regression and Random Forest Regressor with an 80/20 split

# 7\. \*\*Export:\*\* Visualization and export of the processed dataset to Parquet

# 

# \## 🔍 Key Findings

# 

# \* Weekly sales are heavily right-skewed: mean \*\*16,030\*\* versus median \*\*7,659\*\*, with a maximum of \*\*693,099\*\*.

# \* Store type matters: \*\*Type A\*\* stores average \*\*20,145\*\* per record, \*\*Type B\*\* \*\*12,288\*\*, and \*\*Type C\*\* \*\*9,547\*\*.

# \* Top stores by total sales: \*\*20, 4, 14, 13, and 2\*\*.

# \* Holiday weeks average \*\*17,093\*\* versus \*\*15,950\*\* for regular weeks.

# \* Departments \*\*92, 95, and 38\*\* have the highest average weekly sales.

# 

# !\[Top stores](images/1\_top\_stores.png)

# !\[Monthly trend](images/2\_monthly\_trend.png)

# !\[Sales by store type](images/3\_store\_type.png)

# 

# \## 📈 Model Results

# 

# Random Forest (50 trees, max depth 10) outperformed Linear Regression on every metric:

# 

# | Model | RMSE | MAE | R² |

# | :--- | :---: | :---: | :---: |

# | \*\*Linear Regression\*\* | 7,294.32 | 2,501.03 | 0.8970 |

# | \*\*Random Forest\*\* | \*\*5,623.24\*\* | \*\*2,488.35\*\* | \*\*0.9388\*\* |

# 

# !\[Actual vs predicted](images/4\_actual\_vs\_predicted.png)

# !\[Feature importance](images/5\_feature\_importance.png)

# 

# Previous-week sales dominates the Random Forest feature importance at about \*\*81%\*\*, followed by department (\*\*11%\*\*) and store size (\*\*3%\*\*). This makes sense for weekly retail data, where last week is a strong predictor of this week.

# 

# \## ⚠️ Limitations

# 

# \* \*\*Data Leakage Risk:\*\* The train/test split is random (`randomSplit`), not chronological. Combined with the lag feature, the model can see neighboring weeks of the same store and department during training, so the reported R² is likely optimistic compared with real forecasting. A time-based split (e.g., train on earlier dates and test on the latest weeks) would give a more honest estimate.

# \* \*\*Model Exploration:\*\* Only two models were compared and no hyperparameter tuning was done.

# \* \*\*Evaluation:\*\* The Kaggle `test` file was not used for evaluation.

# 

# \## 🚀 Next Steps

# 

# \* Switch to a time-based train/test split and re-evaluate.

# \* Try gradient boosting (`XGBoost` or `GBTRegressor`) and cross-validation.

# \* Build a Tableau or Power BI dashboard on the exported Parquet data.

# 

# \## ▶️ How to Run

# 

# 1\. Open `walmart\_sales\_forecasting\_pyspark.ipynb` in Google Colab.

# 2\. Upload the four CSV files from the `dataset/` folder when the upload cell runs.

# &#x20;  \* \*Note:\* The notebook expects the original Kaggle file names (e.g., `stores - Walmart Sales Forecast.csv`). `dataset/stores.csv` is renamed, so rename it back or edit the path in the notebook.

# 3\. Run the cells in order. Outputs are already saved in the notebook if you only want to read the results.

# 

# \## 👥 My Contribution

# 

# This was a group project for a Big Data course. My responsibilities included:

# \* Architecture Design

# \* Infrastructure Decision Analysis

# \* Documentation \& Report Writing

# \* Project Coordination

