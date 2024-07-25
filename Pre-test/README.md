# <p align="center" style="margin-top: 0px;">Pre-interview Test For Data Analys


## 📚 Table of Contents
- [Tool and Technology](#tool-and-technology)
- [Data Explanation](#data-explanation)
- [Question and Solution](#question-and-solution)

***

## Tool and Technology
  To convert data from an SQL file into a CSV file using MySQL Workbench, begin by opening MySQL Workbench and connecting to your MySQL database where you intend to create the table. Import the SQL file containing table creation and data insertion queries and this action will create the necessary table and popluate them with data if your script includes INSERT statement. 
  
  Next, to export the table data as CSV file, locate the table within the schema, right-click on its name, and choose Table Data Export Wizard. Configure the export settings to specify CSV as the file format and designate a destination path for saving the CSV file.

  After the obtaining the CSV from MySQL WorkBench, the next step involves analyzing it using the Python and the Pandas library.


## Data Explanation

Here are some further details about the dataset:

1. This data has University and ShopInState operations using university and shopState dataset.
2. `TrsansactionId`: its can be count as total or unique purchase and `Amount` is the acutal amount of purchase
***

## Question and Solution

**1. Which Gender spend the most in this data?**

Description:
  To calcuate it, we need to look at the data transactions and customers by combine this two table into one by using the CustomerId from Customers Table align it with CustomerId from Transaction Table.

```python
import pandas as pd
import os

path = os.getcwd()
export_path = os.path.join(path,'./insight/total-count-gender.csv')


customer = pd.read_csv("./warmup-data/customer.csv")
transactions = pd.read_csv("./warmup-data/transactions.csv")

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
---

### Insghts:
  * You might find that males tend to have higher transaction compared to fameles, which could indicate different purchasing priorities or finacial behaviors.


**1. Monthly Transaction?**
