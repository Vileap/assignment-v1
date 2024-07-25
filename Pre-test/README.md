# <p align="center" style="margin-top: 0px;">Pre-interview Test For Data Analys


## 📚 Table of Contents
- [Tools and Technology](#tools-and-technology)
- [Data Overview](#data-overview)
- [Observation and Insights](#observation-and-insights)
- [Conclusion](#conclusion)

***

## Tools and Technology
  To convert data from an SQL file into a CSV file using MySQL Workbench, begin by opening MySQL Workbench and connecting to your MySQL database where you intend to create the table. Import the SQL file containing table creation and data insertion queries and this action will create the necessary table and popluate them with data if your script includes INSERT statement. 
  
  Next, to export the table data as CSV file, locate the table within the schema, right-click on its name, and choose Table Data Export Wizard. Configure the export settings to specify CSV as the file format and designate a destination path for saving the CSV file.

  After the obtaining the CSV from MySQL WorkBench, the next step involves analyzing it using the Python and the Pandas library.


## Data Overview
The dataset includes two tables:

* Customers: Contains information about customers including their ID, name, gender, university, and state.

* Transactions: Records the transactions made by customers, including the transaction ID, customer ID, product, amount, and date.

***

## Observation and Insights


**1. Gender Spending Analysis**

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

#### Observation:
The analysis of transaction data revealed that males (Gender: M) have a higher count of transactions compared to females (Gender: F). Specifically, there were 264 transactions for males and 95 for females.

Gender | Count
-- | --
F | 95
M |  264

![total-count-gender-chart](https://github.com/user-attachments/assets/26413523-3247-4e15-a24f-54eca7a17e71)



#### Insights:
You might find that males tend to have higher transaction compared to fameles, which could indicate different purchasing priorities or finacial behaviors.

***


**2. Average Monthly Student Expenses**

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

#### Output:
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
#### Output 2: 
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

#### Observation:
The average monthly spending of students shows variability throughout the year. Notable peaks in expenses were observed in April and July, with average amounts of 1122.77 and 1121.54 respectively. The lowest average spending occurred in May, with an amount of 956.33.

#### Insight:
April and July experienced the highest average expenses, indicating these months may be associated with higher spending activities or special events. The lowest spending in May suggests that students might be more conservative with their expenses during this period. Understanding these patterns could help in targeting financial products or promotional strategies during high-expense months.

***

**3. Purchase Amount by University**

Description:

  To identify and visualize the top 10 universities by expenses based on your student expenses dataset using Python and Pandas: 

```python
import pandas as pd

customer = pd.read_csv("./data/customer.csv")
transactions = pd.read_csv("./data/transactions.csv")

df = pd.merge(customer, transactions, on="CustomerId")
summary_uni = df[['University', 'Amount']]
value = (summary_uni.Amount).groupby(summary_uni.University).sum().reset_index(name="Amount")

value.sort_values("Amount", axis=0, ascending=False, inplace=True, na_position='last')
print(value.head(10))
```



![top-10-university-expense](https://github.com/user-attachments/assets/99bfbddb-e309-4f15-8e70-f88021ca90c6)

#### Observation:
Here are the top universities by total purchase amount:

University	| Amount
| -- | --
University of Illinois at Urbana-Champaign | 39584.67
Illinois Institute of Technology | 30217.00
University of California, Santa Barbara (UCSB) | 27325.62
University of Illinois, Chicago (UIC) |  23871.08
Clark University | 22100.34
Howard University |  21652.28
University of Wisconsin-Madison | 21609.87
California Institute of Technology (Caltech) |  20743.08
Wayne State University | 20249.32
University of California, San Diego (UCSD) |  19984.42

#### Insights:
Universities with higher purchase amounts might have a more affluent student base or higher purchasing frequency. Tailoring offers or discounts to students from these universities could be beneficial for increasing sales and engagement.

***

**4. Top 10 Users by Purchase Transactions**

Description:

  To identify and visualize the top users by expenses based on your student expenses dataset using Python and Pandas: 

```python
import pandas as pd

customer = pd.read_csv("./data/customer.csv")
transactions = pd.read_csv("./data/transactions.csv")

df = pd.merge(customer, transactions, on="CustomerId")
most_spender = df['Username'].value_counts().reset_index(name='Count')
most_spender.head(10)

```

#### Observation:

Username | Count
-- | --
Phillip Maxwell	| 7
Jocelyn Hope	| 7
Tom Bristow	| 7
Bart Bell	| 7
Chad Mullins | 6
Tyson Wade | 6
Joseph Kelly | 6
Isla Holmes | 6
Mark Swift | 6
Kirsten Bennett	| 6

![top-10-most-spender](https://github.com/user-attachments/assets/219ff521-ad87-4b37-ac96-115e3f17b03f)

#### Insight:
Users with the highest number of transactions are highly engaged customers. Targeting these top users with personalized offers or loyalty rewards could enhance customer retention and increase overall engagement.

***

**5. Top 5 States by Transaction Amount**

```python
import pandas as pd

customer = pd.read_csv("./data/customer.csv")
transactions = pd.read_csv("./data/transactions.csv")
df = pd.merge(customer, transactions, on="CustomerId")

value = df[['ShopInState', 'Amount']]

summary = (value.Amount).groupby(value.ShopInState).sum().reset_index(name="Amount")
summary.sort_values("Amount", axis=0, ascending=False, inplace=True, na_position='last')
print(summary.head(5))
```

#### Observation:
The top 5 states by total transaction amount are:

ShopInState | Amount
-- | --
Kansas  | 30244.23
Wisconsin  | 29881.15
Pennsylvania  | 28898.50
Missouri | 26285.01
Minnesota  | 26061.73

![top-5-state-bytransaction-amount](https://github.com/user-attachments/assets/fc2d2b33-1695-429a-af5f-4e6a2f69dd43)


#### Insight:
States with the highest transaction amounts represent key markets for regional promotions. Focused marketing strategies and promotional campaigns in these states could drive further growth and capitalize on existing high spending patterns.

***

**6. Comparison of Top 5 Most and Least Popular Products**

#### Top 5 Most Popular Products
```python
import pandas as pd

customer = pd.read_csv("./data/customer.csv")
transactions = pd.read_csv("./data/transactions.csv")
df = pd.merge(customer, transactions, on="CustomerId")
mostItem = df.groupby(["Description"]).size().reset_index(name="Count")

mostItem.sort_values("Count", axis=0, ascending=False,
                 inplace=True, na_position='last')
print(mostItem.head(5))
```

![top5-most-popular](https://github.com/user-attachments/assets/3514ec57-9125-4c7f-8586-e6ff3d12d446)


Description | Count
-- | --
Salmon | 32
Bottled waters |31
Seafood |27
Flour |27
Dairy |27

#### Top 5 Least Popular Products

```python
import pandas as pd

customer = pd.read_csv("./data/customer.csv")
transactions = pd.read_csv("./data/transactions.csv")
df = pd.merge(customer, transactions, on="CustomerId")
mostItem = df.groupby(["Description"]).size().reset_index(name="Count")

mostItem.sort_values("Count", axis=0, ascending=False,
                 inplace=True, na_position='last')
print(mostItem.tail(5))
```

Description | Count
-- | --
Grooming products | 13
Dishware | 12
Personal Care | 11
Condiments | 11
House-cleaning products | 9

![top-5-least-popular](https://github.com/user-attachments/assets/ac5b7fdb-04e5-44ba-be9d-fcef3087cb15)



#### Observation:
Top-selling products are predominantly food items and essentials, while the least popular items include household and personal care products.

#### Insight:
 Consumers prioritize essential and consumable goods over household and personal care items, which could influence inventory and marketing strategies.

***

## Conclusion
The transaction data analysis reveals key spending patterns and priorities:

* Gender Spending: Males spend more than females, indicating possible differing financial behaviors.
* Monthly Expenses: Significant fluctuations in student expenses suggest seasonal spending trends.
* Top Users: Highly engaged users show loyalty, offering opportunities for targeted marketing.
* University Purchases: Purchase amounts vary widely, reflecting differences in student financial capacity.
* State Spending: High transaction volumes in certain states highlight key markets.
* Product Popularity: Top-selling items are essentials and staples, while least popular products are household and personal care items.

These insights can inform marketing strategies, improve customer engagement, and optimize financial planning to enhance business performance and satisfaction.
