# RBAC Champion - Final Round
---
## I. Introduction
\- In this competition, our team of four members was tasked with analyzing a real-world case study focusing on a critical issue at SHB Finance: customer delinquency.  
\- Upon receiving the assignment, we immediately began researching various aspects related to the topic of non-performing loans. Below is the content of our presentation.  

\- [Here is slides](https://drive.google.com/file/d/14za7it2xeYQ7anyipXZ6_VaUAfW5qsVT/view?usp=sharing)

## II. Data Preparation
\- The three datasets are summarized with information about missing value rates (ranging from 3% to 17.2%) and important characteristics. For example, the "Democracy" dataset has missing data in its primary key column, while the "Loan Origin" dataset contains format issues.  

\- Merging Tables: I merged the three datasets using the shared CONTRACT_NO column as the primary key to create a unified dataset.  

\- Data Transformation: Extracted the month and year from the DISBURSEMENT_DATE column and encoded the binary column HAS_INSURANCE to make the data suitable for further analysis.

\- Handling Missing Values: Missing values were filled using domain knowledge and business understanding. For columns where no specific context was available, a forward-fill method was used to maintain data integrity.

\- Dropping Unnecessary Columns: Removed redundant columns created for preprocessing purposes, such as DISBURSEMENT_DATE, which was replaced by the month and year columns.

## III. Developing the Matrix
Here are some matrices that we built:  

\- Transaction Metrix: This matrix highlights customer movement across DPD buckets within a 1-month period, reflecting the rate of transition between buckets in the dataset. The structure of matric is: Rows represent the initial month. Columns represent the next month from the dataset and the values in the cells represent the proportion of outstanding transitioning between stages.

\- Vintage Metrix: The vintage analysis matrix tracks the cumulative delinquency rates of accounts opened in different quarters (e.g., 2022Q2, 2022Q3, etc.) over a 12-month period. Each row corresponds to a cohort (accounts opened in a specific quarter), and each column represents the cumulative delinquency rate (30 - 90 DPD) at the end of a given month.

\- Flowrate Matrix: In credit risk analysis for banks, the flow rate is a crucial indicator used to evaluate and forecast the movement of loans or debts across different levels of delinquency over time. It is a component of the Delinquency Matrix, which helps banks better understand the likelihood and probability of loans transitioning into overdue status within specific timeframes.

\- Monthly Rollrate Matrix:
> - Roll rate analysis is used for solving various type of problems
> - Roll rate are used to estimates financial losses due to future defaults and it is also used to determine the definition of 'bad' customers(defaulters)
> - Most common definition of 'bad customer' is customer delinquent for 90 days or more
> - Roll rate analysis help to answer the question with quantitative reasoning - "Should we use 60 days or 90 days or 120 days or higher delinquency to identify 'bad' customer?".

## IV. Modeling 
\- **Objectives**
We predict the probability of a contract rolling to higher bucket in the next month. Then, we will use the results to divide the contracts into 9 score band
\- **About the target variable: IS_ROLL_UP**
It represents whether a contract’s delinquency status worsens or not in the following month.
\- **Modeling - Prediction**
We will use data with mob = to 12 for prediction. Previous data will be used for modeling.
\- **Feature Selection**
We select variables in the repayment table have high correlation with the target. We use VIF to detect similar variables and take actions.
About other selected variables: 
Although they show a low correlation, they still provide value in certain contexts. So we put them in the model
> - Improving Model Interpretability
> - Capture Non-linear Relationships
> - Provide Contextual Information
> - Domain Knowledge:SHBFinance, features like LOAN_AMOUNT, WORKING_IN_YEAR, and LOAN_TERM are critical for profiling customers and designing appropriate products.  

\- **Data preprocessing**
After feature selection, we do some data preprocessing like address imbalance data by oversampling, then we scale the data using standard scaler. 
\- **Modeling process**
First, we tune the hyperparameter using randomsearchcv 
Next, We use cross validation to provide reliable estimate of model performance
Then, we calculate GINI, KS and PSI for further evaluation. 
\- **Results**
> - There are two models that stand out which are logistic regression and XGBoost.
> - About PSI, Logistic Regression has the lowest PSI, indicating it is the most stable model
> - However, XGBoost has highest results in other metrics.
> - Our target is to maximize accuracy so we decided to choose XGBOOST as the best model for prediction.  
> 
\- **Prediction Probabilities**
\- **Score band division**
> - We divide the result into 9 scoreband using K-means.  
> - The division of scoreband is reliable through some investigation in the slides here.  

\- **Customer Segmentation** 
> - For better understanding of customer, we use Kmeans Clustering to segmentize them. 
> - And here is our feature selection 
> - We use the elbow method to find the optimal number of clusters, and in this case it is 4. 
> - As you can see, cluster 1 and 2 have higher quantities and also lower risk. All relevant information has been summarized in this table.
