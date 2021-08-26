# Python for Data Viz

# Hands on link
https://colab.research.google.com/drive/14F12sPFU6PN7_YFwX77UCfHowWMUdEIr?usp=sharing

# Mini Capstone link
1: https://colab.research.google.com/drive/1sAO8x9ooYND4G1KRo0Tnsp9GGpdfRVYP?usp=sharing

# Dataset
covid cluster: https://raw.githubusercontent.com/MoH-Malaysia/covid19-public/main/epidemic/clusters.csv
covid cases: https://raw.githubusercontent.com/MoH-Malaysia/covid19-public/main/epidemic/cases_state.csv


# Refresher in Python
Importing packages
Assigning variables
Using functions
Importing data into dataframes

# Live Coding
```
df_cluster.shape
df_cluster.columns
df_cluster.dtypes
df_cluster.isna().sum()
```

# Cluster by state
```
df_cluster['state_list'] = df_cluster['state'].str.split(',')
df_cluster_exploded = df_cluster.explode(column='state_list')
df_cluster_exploded

df_cluster['state_list'] = df_cluster['state']\
                                              .str.replace(' & ',',')\
                                              .str.strip()\
                                              .str.upper()\
                                              .str.split(',')

df_cluster_exploded = df_cluster.explode(column='state_list')

df_cluster_exploded['state_list'] = df_cluster_exploded['state_list'].str.strip()

df_cluster_exploded.groupby('state_list').count()['cluster'].sort_values(ascending=False).plot(kind='bar')
```

```
plt.figure(figsize=(12,8))
plt.title("Customer")
plt.ylabel("Total Spending in RM")
plt.xlabel("Customer Name")
plt.xticks(rotation=45)
plt.bar(df_grouped.index, df_grouped.values)
```

# Pie Chart
```
# Check the columns
df_cluster['status'].value_counts().plot(kind='pie', autopct='%.2f')
```

# Line Chart
```
df_case_by_state.query("state == 'Selangor'").set_index('date')['cases_new'].plot()
```

# Extra
```
df_cluster.set_index('cluster').filter(['tests', 'cases_total']).head(10).plot.bar(stacked=True)

df_cluster.sort_values(by='deaths', ascending=False).filter(['cluster', 'deaths']).head(10)
```

# Mini Capstone
3. Create a chart to show which customer has been spending the most at the cafe

```
df.groupby('customer_name').sum()['order_cost'].sort_values(ascending=False).plot(kind='bar')

df_grouped = df.groupby('customer_name').sum()['order_cost'].sort_values(ascending=False)

plt.figure(figsize=(12,8))
plt.bar(df_grouped.index, df_grouped.values)

plt.figure(figsize=(12,8))
plt.title("Customer")
plt.ylabel("Total Spending in RM")
plt.xlabel("Customer Name")
plt.xticks(rotation=45)
plt.bar(df_grouped.index, df_grouped.values)
```

4. Sales

```
y = df.groupby('month').sum()['order_cost'].values
x = ['Jan', 'Feb', 'Mar', 'April', 'May', 'Jun', 'July', 'August', 'September', 'October', 'November', 'December']

plt.plot(x,y, color='red')
plt.bar(x,y, color='blue')

```