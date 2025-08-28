# Project 2
The dataset contains the wallmart retail sales, the store type, dept etc.

## Objective:
- To analyze the monthly sales of the wallmart
- Plot the graph for moving averages and seasonal patterns
- Break down revenue by product and region over time

### Explanation:
I uploaded the data nad merge the data with the store csv

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('D:\.MY PERSONAL DATA/programming Practice/Data Analytics/Elevvo Pathways Projects/Time Series Breakdown of Retail Sales/csv files/train.csv')

stores = pd.read_csv('D:\.MY PERSONAL DATA/programming Practice/Data Analytics/Elevvo Pathways Projects/Time Series Breakdown of Retail Sales/csv files/stores.csv')


df = df.merge(stores, on='Store', how='left')
```

Then i converted the date to datetime and then Extracted month from it.I groupby the month to analyze the monthly trends


```python
df['Date'] = pd.to_datetime(df['Date'])

# for Monthly_Sales 
df['Month'] = df['Date'].dt.to_period('M')
monthly_sales = df.groupby('Month').Weekly_Sales.sum().reset_index()
monthly_sales['Month'] = monthly_sales['Month'].dt.to_timestamp()
```


I also calculated the moving avg so that i can see the overtime sales in  one frame
```python
# For Moving Average
monthly_sales['Moving_Avg'] = monthly_sales['Weekly_Sales'].rolling(window=3, center=True).mean()

# Plotting the monthly Sales
plt.figure(figsize=(20, 7))
sns.lineplot(data=monthly_sales, x='Month', y='Weekly_Sales',markers='o')
sns.lineplot(data=monthly_sales, x='Month', y='Moving_Avg',markers='o')
plt.title('Monthly Sales with Moving Average')
plt.xlabel('Month')
plt.ylabel('Total Sales')
plt.tight_layout()
plt.show()
```