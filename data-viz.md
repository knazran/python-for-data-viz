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


# Day 2

# Chart Styling
```
axis1 = df_case_by_state.groupby('date').sum().plot(
                                            figsize=(12,8),
                                            title="Trend of daily COVID-19 cases in Malaysia",
                                            xlabel="Date",
                                            ylabel="Number of cases",
                                            legend=False,
                                            fontsize=12
                                            )


axis1.set_ylabel("Number of cases", fontdict={'fontsize':14}, labelpad=16)
axis1.set_xlabel("Date", fontdict={'fontsize':14}, labelpad=16)
axis1.set_title("Trend of daily COVID-19 cases in Malaysia",pad=20, fontdict={'fontsize':16})

plt.style.use('bmh')
```

# Bar Chart
```
plt.style.use('ggplot')

axis1 = df_cluster_viz.plot(kind='bar', figsize=(10,5))

axis1.set_ylabel("Number of Clusters", fontsize=10, labelpad=16)
axis1.set_xlabel("")
axis1.set_xticklabels(df_cluster_viz.index, rotation=70, fontsize=9)
axis1.set_title("Number of COVID-19 clusters in Malaysia by States",pad=20, fontsize=16)

axis1.spines['right'].set_visible(False)
axis1.spines['top'].set_visible(False)
```

# Seaborn
```
iris = sns.load_dataset('iris')
 
# plotting strip plot with seaborn
ax = sns.stripplot(x='species', y='sepal_length', data=iris)

plt.figure(figsize=(12,8))
ax = sns.stripplot(x='state', y='cases_new', data=df_case_by_state)


sns.displot(df_cluster_exploded, x="cases_total")
```

```
# Data prep
sns.boxplot(x='category', y='deaths', data=df_cluster_exploded)
sns.violinplot(x='category', y='deaths', data=df_cluster_exploded,
               hue='status', split=True)
```

# Formatted Violin plot
```
from matplotlib.ticker import FuncFormatter

def format_tick_labels(x, pos):
    return '{:,}'.format(int(x))

axis1 = sns.violinplot(x='house_age_group', y='median_house_value', data=df_housing)

axis1.set_ylabel("Number of Clusters", fontsize=10, labelpad=16)
axis1.set_xlabel("")
axis1.set_xticklabels(df_cluster_viz.index, rotation=70, fontsize=9)
axis1.set_title("Number of COVID-19 clusters in Malaysia by States",pad=20, fontsize=16)

axis1.yaxis.set_major_formatter(FuncFormatter(format_tick_labels))

axis1.spines['right'].set_visible(False)
axis1.spines['top'].set_visible(False)
```

# Housing
```
sns.boxplot(x='income_group', y='median_house_value', data=df_housing)
sns.boxplot(x='house_age_group', y='median_house_value', data=df_housing)
sns.violinplot(x='house_age_group', y='median_house_value', data=df_housing)
```

# Heatmap
```
corr_y = pd.DataFrame(df_housing).corr()

plt.figure(figsize=(12,8))
cmap = sns.diverging_palette(220, 20, as_cmap=True)

sns.heatmap(corr_y, 
            xticklabels=corr_y.columns.values,
            yticklabels=corr_y.columns.values, 
            annot=True,
            cmap=cmap
          )
```

# Mapping
```
import folium
from folium.plugins import HeatMap

map_california = folium.Map(location=[36.7783,-119.4179],
                    zoom_start = 6, min_zoom=5) 

df = df_housing[['latitude', 'longitude']]
data = [[row['latitude'],row['longitude']] for index, row in df.iterrows()]
HeatMap(data, radius=10).add_to(map_california)

map_california
```

# Plotly
```
fig = px.line(df_case_by_state, x = "date", y = "cases_new",
              color = "state")
fig.show()
```

Interactive plots
```
df_cases_grouped = df_case_by_state.groupby('date').sum()

# Create figure
fig = go.Figure()

fig.add_trace(
    go.Scatter(x=list(df_cases_grouped.index), y=list(df_cases_grouped['cases_new'])))

# Set title
fig.update_layout(
    title_text="Time series with range slider and selectors"
)

# Add range slider
fig.update_layout(
    xaxis=dict(
        rangeselector=dict(
            buttons=list([
                dict(count=1,
                     label="1m",
                     step="month",
                     stepmode="backward"),
                dict(count=6,
                     label="6m",
                     step="month",
                     stepmode="backward"),
                dict(count=1,
                     label="YTD",
                     step="year",
                     stepmode="todate"),
                dict(count=1,
                     label="1y",
                     step="year",
                     stepmode="backward"),
                dict(step="all")
            ])
        ),
        rangeslider=dict(
            visible=True
        ),
        type="date"
    )
)
```