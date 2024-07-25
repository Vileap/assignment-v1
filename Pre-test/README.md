# <p align="center" style="margin-top: 0px;">Pre-interview Test For Data Analys


## 📚 Table of Contents
- [Tool and Technology](#tool-and-technology)
- [Data Overview](#data-overview)
- [Question and Insights](#questions-and-insights)

***

## Tool and Technology
  To convert data from an SQL file into a CSV file using MySQL Workbench, begin by opening MySQL Workbench and connecting to your MySQL database where you intend to create the table. Import the SQL file containing table creation and data insertion queries and this action will create the necessary table and popluate them with data if your script includes INSERT statement. 
  
  Next, to export the table data as CSV file, locate the table within the schema, right-click on its name, and choose Table Data Export Wizard. Configure the export settings to specify CSV as the file format and designate a destination path for saving the CSV file.

  After the obtaining the CSV from MySQL WorkBench, the next step involves analyzing it using the Python and the Pandas library.


## Data Overview
The dataset includes two tables:

Customers: Contains information about customers including their ID, name, gender, university, and state.
Transactions: Records the transactions made by customers, including the transaction ID, customer ID, product, amount, and date.

***

## Questions and Insights

**1. Which Gender spend the most in this data?**

Description:
  To calcuate it, we need to look at the data transactions and customers by combine this two table into one by using the CustomerId from Customers Table align it with CustomerId from Transaction Table.

```python
import pandas as pd
import os

path = os.getcwd()
export_path = os.path.join(path,'./insight/total-count-gender.csv')


customer = pd.read_csv("./data/customer.csv")
transactions = pd.read_csv("./data/transactions.csv")

df = pd.merge(customer, transactions, on="CustomerId")

value = df.rename(columns={0:'Username', 2:'Gender'})[['Username', 'Gender']]

summary = value.drop_duplicates('Username', keep='first')
summary

sum_total_gender = summary.groupby('Gender')['Gender'].count()
sum_total_gender 
# This line of code is for show graph in Bar chart
sum_total_gender.plot(kind='bar')
# This line of code is used for printing the table
print(sum_total_gender)
# this line of code is for printing the data to csv file.
sum_total_gender.to_csv(export_path, index=False, header=True)
```
### Output:
Gender | Count
-- | --
F | 95
M |  264

![total-count-gender-chart](https://github.com/user-attachments/assets/26413523-3247-4e15-a24f-54eca7a17e71)

### Insghts:
  * You might find that males tend to have higher transaction compared to fameles, which could indicate different purchasing priorities or finacial behaviors.

***


**2. What is the average amount of student expense in Monthly?**

Description:

  To analyze the monthly student expenses using Python and Pandas, you'll typically start with a Transaction Date and Amount. Here's a structured approach to perform this analysis:

```python
import pandas as pd

customer = pd.read_csv("./data/customer.csv")
transactions = pd.read_csv("./data/transactions.csv")

df = pd.merge(customer, transactions, on="CustomerId")

df['TransactionDate'] = pd.to_datetime(df['TransactionDate'], format='ISO8601')
monthly = df.groupby(by=[pd.Grouper(key='TransactionDate', freq='M')])['Amount'].mean().reset_index()
print(monthly)
```

### Output:
Month | TransactionDate	| Amount
-- | -- | --
0	| 2023-01-31 |	1048.341707
1	| 2023-02-28 |	1063.370353
2 |	2023-03-31 |	1015.124375
3 |	2023-04-30 |	1122.768372
4	| 2023-05-31 | 956.328875
5	| 2023-06-30 | 959.833780
6	| 2023-07-31 | 1121.541778
7	| 2023-08-31 | 975.807174
8	| 2023-09-30 | 1102.982703
9 | 2023-10-31 | 1035.899213
10 | 2023-11-30 | 1039.426986
11 | 2023-12-31	| 1045.446207

![monthly-expense-student](https://github.com/user-attachments/assets/ff1998bc-86c9-4a48-804d-e615b4cd1081)

  To extract the insight from this analyze, we need to find which month is the most expense by perform the following code:
```python
  monthly.sort_values("Amount", axis=0, ascending=False, inplace=True, na_position='last')
  print(monthly)
```
### Output 2: 
Month | TransactionDate	| Amount
-- | -- | --
3	| 2023-04-30| 1122.768372
6 |	2023-07-31 | 1121.541778
8	| 2023-09-30 | 1102.982703
1	| 2023-02-28 | 1063.370353
0	| 2023-01-31 | 1048.341707
11 | 2023-12-31 | 1045.446207
10 | 2023-11-30 | 1039.426986
9	| 2023-10-31 | 1035.899213
2	| 2023-03-31 | 1015.124375
7	| 2023-08-31 | 975.807174
5	| 2023-06-30 | 959.833780
4	| 2023-05-31 | 956.328875


### Insghts:
  * Based on chart above and "Output 2" table, it has been identified that April had the highest spending compared to other months. This conclusion was drawn after grouping the data by TransactionDate, summing up the amount or expenses for each month, and sorting the results to find the month with the highest total expenditure.

***
  
