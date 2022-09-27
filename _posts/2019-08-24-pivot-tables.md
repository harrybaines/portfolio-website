---
title: "Creating Pivot Tables with Python"
layout: post-page
---

Pivot tables are a great and powerful way to summarize large datasets. In this post, we will use Python and the Pandas library to construct a pivot table from a list of employee time records in CSV format. If you want to follow along with this tutorial I highly recommend using Jupyter notebooks. Alternatively, you can read the post and download the code available in full at the end.

## What is a Pivot Table?

A pivot table is a tool that helps us summarise data, by reorganising and applying functions to it. They are a great way to extract useful information by taking a different look at the data. Columns can be transformed into rows and vice versa. Microsoft Excel can be used to create pivot tables, although in this post we use Python to create them programmatically, which would enable easy integration with software applications.

## Python Code

For this post I generated a synthetic dataset of employee time records in CSV format and can be downloaded <a href="timerecords.csv" download>here</a>. The aim is to create a pivot table for this data to visualise the total hours spent per week for each employee over a set of 5 weeks. We expect the totals for each employee to be roughly equal to a standard working week of 37.5 hours ± a few hours here or there. If you'd like to download the script that generated this data, you can take a look <a href="https://gist.github.com/harrybaines/1cd443fc596c3e9a833f6522b75e25a0">here</a>.

### 1. Import our dependencies
Let's load the pandas library to store the time records data as a DataFrame:

```python
import pandas as pd
```

### 2. Import our time records data
Using the pandas method *read_csv* we can easily load our data into a pandas DataFrame and take a look at the first 20 rows:

```python
df = pd.read_csv('timerecords.csv')
print(df.head(20))
```

<div class="custom-image" style="max-width: 550px;">
  <img src="data.png" alt="First 20 rows of CSV" />
  <figcaption>First 20 rows of our data</figcaption>
</div>

### 3. Cleaning our data
I have deliberately constructed a 'clean' dataset, meaning we don't need to spend much time formatting the data. Notice we have a Week Ending column in our dataset. Let's convert it to a pandas datetime format so we can order our dates in the DataFrame:

```python
df['Week Ending'] = pd.to_datetime(df['Week Ending']).dt.strftime('%m/%d/%Y')
```

Next, let's create a new column, Employee, which contains the Firstname and Surname columns concatenated together into a single column:

```python
df['Employee'] = df['Firstname'].str.cat(df['Surname'], sep=" ")
```

### 4. Creating the pivot table
Pandas comes with a handy built in *pivot_table* method we can use:

```python
# Create pivot table - total hours per week per employee
table = df.pivot_table(
    index='Employee', 
    columns='Week Ending', 
    values='Hours', 
    aggfunc='sum'
)
print(table.head(10))
```

Which gives us the following:

<div class="custom-image" style="max-width: 550px;">
  <img src="pivot_table.png" alt="irst 10 rows of pivot table" />
  <figcaption>First 10 rows of pivot table</figcaption>
</div>

Here we can see each row corresponds to an employee (the index), and all the week endings are taken as column names. The values of each cell in the table contain the total hours spent for that particular employee's week.

### 5. Plotting our pivot table data
We can go one step further and visualise the results on a bar chart using the seaborn library (here I trim the y axis to 30-50 hours for visualisation purposes):

```python
import matplotlib.pyplot as plt
import seaborn as sns; sns.set()

table.plot(kind='bar')
plt.ylabel("Hours")
plt.gcf().set_size_inches(16, 6)
plt.ylim(30, 50)
plt.show()
```

![Pivot table visualisation](vis.png)

This helps us to very quickly identify those employees who are working fewer/more hours compared to other employees. Again this is just a synthetic dataset, in reality the totals would be roughly 37.5 hours per employee with some fluctuations here or there so performances for particular weeks would stand out more in reality, but this was just to show pivot tables in action.

And that's it! In this post we covered what pivot tables are, why they're useful and used an example to demonstrate what pivot tables can give us, with a visualisation to top it all off. Despite using a synthetic dataset, I hope this tutorial was interesting and gives insight into how pivot tables can be used in python.

For reference here is the full code:

```python{numberLines: true}
# Import dependencies
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns; sns.set()

# Load data
df = pd.read_csv('timerecords.csv')
print(df.head(20))

# Data cleaning
df['Week Ending'] = (
  pd.to_datetime(df['Week Ending']).dt.strftime('%m/%d/%Y')
)
df['Employee'] = df['Firstname'].str.cat(df['Surname'], sep=" ")

# Create pivot table
table = df.pivot_table(
    index='Employee', 
    columns='Week Ending', 
    values='Hours', 
    aggfunc='sum'
)
print(table.head(10))

# Visualisation
table.plot(kind= 'bar')
plt.ylabel("Hours")
plt.gcf().set_size_inches(16, 6)
plt.ylim(30, 50)
plt.show()
```







