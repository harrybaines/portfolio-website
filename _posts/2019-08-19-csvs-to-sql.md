---
title: "Converting CSV's to SQL statements"
layout: post-page
---

We commonly encounter situations where we require data in one format to be converted into another. In Data Science, this conversion process is known as Data Wrangling or Data Munging, where we transform data into a more appropriate format for processing. This post describes an application I developed to automatically process CSV files into SQL insert statements using a Flask server. If you want to follow along, you can download the job extracts file <a href="extracts.csv" download>here</a>.

Since the summer of 2018 I've been developing a full-stack time management system for an architecture firm <a href="/projects/ca-tms">(read more)</a>. 
During the migration from the old system (involving several Excel files) to my new system (a React front-end with a PostgreSQL database hosted on Heroku) I was frequently presented with data files, in CSV format, to be inserted into the new database. 

## What is a CSV?

A CSV (Comma Separated Values) file organises data into rows, with each row comprising of multiple values, all separated by commas. I was using the pgweb tool to connect to the remote database hosted on 
Heroku, however it lacked functionality to import CSV's into the database. Therefore, I decided to apply some of my python and data science skills to automate the process of converting CSV files into PostgreSQL statements which could be directly executed in the pgweb execution window.

Below is a synthetic set of job extracts in a CSV file that resemble something very similar to what I was frequently presented with:

<div class="custom-image" style="max-width: 600px;">
  <img src="extracts.png" alt="Job Extracts CSV" />
  <figcaption>Job Extracts CSV</figcaption>
</div>

## Pivot table with Python

Before creating the application, I decided to simplify matters and manually change the CSV headers to match those of the jobs table in my database (i.e. job\_title was initially Job Title, date\_added was initially Date Added etc). Make sure to change Date Added in the CSV date_added so the following code will work.

### 1. Import dependencies 
First step is to import our dependencies. Pandas is a useful library which provides data structures to store and manipulate our data, including a useful 
*read_csv* method to open our CSV in a single line of code. I also decided to run a Flask server on localhost, enabling a more intuitive UI to upload CSV 
files and observe the SQL output.

```python
# Import dependencies
import pandas as pd
from flask import Flask, request
```

### 2. Define a helper function

The following helper function will format dates in the format dd/mm/yyyy to PostgreSQL date formats:

```python
def format_date(date):
  return '-'.join(date.split('/')[::-1]) + 'T00:00:00Z'
```

### 3. Create Flask application
We can very easily create our Flask server object with the following line of code:

```python
app = Flask(__name__)
```

### 4. Define the index route
Next step is to create the separate routes for our application. When the application is opened in a browser on localhost:5000 we visit the '/' route. 
This is the main/index page of our application, and as such we can serve a basic HTML form to allow users to input CSV files:

```python{numberLines: true}
@app.route('/')
def form():
  return """
    <html>
      <body>
        <h1>Transform a file demo</h1>
        <form action="/transform" method="post" enctype="multipart/form-data">
          <input type="file" name="data_file" />
          <input type="submit" />
        </form>
      </body>
    </html>
  """
```

Notice how we set the form action attribute of the form to '/transform' and the method to 'post'. Once a CSV file is selected and we click the submit button, this will submit a POST request containing the CSV and the browser will redirect to the url localhost:5000/transform. This /transform route is where our SQL output will be presented.

### 5. Define the /transform route

Here is the full function to transform the CSV file into an SQL statement:

```python{numberLines: true}
@app.route('/transform', methods=["POST"])
def transform_view():
  request_file = request.files['data_file']
  if not request_file:
    return "No file"

  df = pd.read_csv(request_file)

  # Read job extracts and fill empty cells with nulls
  df = df.fillna('null')

  # Construct insert stmt from columns
  headers = map((lambda x: '\"' + x + '\"'), df.columns)
  insert = 'INSERT INTO jobs (' + ", ".join(headers) + ") VALUES"

  # Deal with single quotes (postgres needs extra ' after every ')
  df = df.applymap(lambda x: str(x).replace("'","''"))

  # Format dates
  df['date_added'] = df['date_added'].apply(format_date)

  # Get data from rows and append to insert statement
  inserts = []
  for index, row in df.iterrows():
    values = map((lambda x: '\'' + str(x) + '\'' if x != 'null' else x), row)
    inserts.append("<br />(" + ", ".join(values) + ")")

  # Append inserts to main insert stmt
  insert += ",".join(inserts)
  return '''
    <div>
      <pre style="font-size: 23px; white-space:normal;">
        {}
      </pre>
    </div>
  '''.format(insert)
```

Let's break this down. First, we define a function, *transform\_view*, which will hold our CSV to PostgreSQL code. This route accepts a POST request containing our CSV file from the form we submitted earlier. We can access the CSV using request.files - we use data\_file to access our CSV because the name of our HTML input had a name attribute of data\_file. 

```python
@app.route('/transform', methods=["POST"])
def transform_view():
  request_file = request.files['data_file']
  if not request_file:
    return "No file"
```

Next we can load our uploaded CSV in request\_file into a Pandas DataFrame, a 2D data structure capable of storing data in columns of different types. We 
can fill empty cells of the DataFrame using fillna('null') on the DataFrame object.

```python
  df = pd.read_csv(request_file)
  df = df.fillna('null')
```

This next step is important. Here we dynamically obtain the column names from our CSV file. This assumes the first row of our CSV contained column names 
which correspond to our database column names. To obtain a list of these column names we can map over our columns in the DataFrame, appending double quotes around each column name. Using python's .join function generates a comma separated list of each of our column names. This constructs the beginning of our SQL insert statement:

```python
  headers = map((lambda x: '\"' + x + '\"'), df.columns)
  insert = 'INSERT INTO jobs (' + ", ".join(headers) + ") VALUES"
```

PostgreSQL can be fussy when it comes to single quotes. If we have a data item in our CSV containing a single quote (e.g. O'Riley) we must escape the quote 
  by adding an extra quote next to it. The following code replaces every single quote with two single quotes in our DataFrame:

```python
  df = df.applymap(lambda x: str(x).replace("'","''"))
```

For my specific use case I was required to store the date an individual row was added. PostgreSQL has a format of YYYY-MM-DDT00:00:00Z and the dates I had 
the format dd/mm/yyyy. We can apply a custom function to a specific column in our DataFrame by using the .apply function on a DataFrame column:

```python
  df['date_added'] = df['date_added'].apply(format_date)
```

Where the format\_date function was previously defined as a helper function.

Returing to our SQL logic now, we can iterate over the rows of our DataFrame using .iterrows() on our df object. We then extract the row values into a values 
variable, and append a string of these values separated by commas, inside parentheses, to an inserts list. After completion of the loop, our inserts list contains 
all our SQL insert statements as strings. We must then join these statements by commas into a new variable, which we insert into a HTML pre tag to format our 
SQL code.

```python
  # Get data from rows and append to insert stmt
  inserts = []
  for index, row in df.iterrows():
    values = map((lambda x: '\'' + str(x) + '\'' if x != 'null' else x), row)
    inserts.append("<br />(" + ", ".join(values) + ")")

  # Append inserts to main insert stmt
  insert += ",".join(inserts)
  return '''
  <div>
    <pre style="font-size: 23px; white-space:normal;">
      {}
    </pre>
  </div>
  '''.format(insert)
```

### 6. Run the application

Finally, we can run our Flask application:

```python
if __name__ == "__main__":
  app.run()
```

If we open localhost:5000, we see the following:

<div class="custom-image" style="max-width: 300px;">
  <img src="form.png" alt="User Input Form" />
  <figcaption>User Input Form</figcaption>
</div>

If we upload our CSV (ensure the first row has column names the same as our database column names) and submit, we are presented with the following:

<div class="custom-image" style="max-width: 750px;">
  <img src="result.png" alt="PostgreSQL Insert Statement" />
  <figcaption>PostgreSQL Insert Statement</figcaption>
</div>

The result is a single PostgreSQL insert statement we can use in an SQL engine. And that's it! We have successfully converted a CSV file to SQL statements using Python, Pandas and Flask. This tool has been extremely helpful for processing large CSV files.

For reference here is the full code:

```python{numberLines: true}
from flask import Flask, make_response, request
import pandas as pd

# Formats a date in format dd/mm/yyyy to PostgreSQL date format
def format_date(date):
  return '-'.join(date.split('/')[::-1]) + 'T00:00:00Z'

app = Flask(__name__)

@app.route('/')
def form():
  return """
    <html>
      <body>
        <h1>Transform a file demo</h1>
        <form action="/transform" method="post" enctype="multipart/form-data">
          <input type="file" name="data_file" />
          <input type="submit" />
        </form>
      </body>
    </html>
  """

@app.route('/transform', methods=["POST"])
def transform_view():
  request_file = request.files['data_file']
  if not request_file:
    return "No file"

  df = pd.read_csv(request_file)

  # Read job extracts and fill empty cells with nulls
  df = df.fillna('null')

  # Construct insert stmt from columns
  headers = map((lambda x: '\"' + x + '\"'), df.columns)
  insert = 'INSERT INTO jobs (' + ", ".join(headers) + ") VALUES"

  # Deal with single quotes (postgres needs extra ' after every ')
  df = df.applymap(lambda x: str(x).replace("'","''"))

  # Format dates
  df['date_added'] = df['date_added'].apply(format_date)

  # Get data from rows and append to insert statement
  inserts = []
  for index, row in df.iterrows():
    values = map((lambda x: '\'' + str(x) + '\'' if x != 'null' else x), row)
    inserts.append("<br />(" + ", ".join(values) + ")")

  # Append inserts to main insert stmt
  insert += ",".join(inserts)
  return '''
    <div>
      <pre style="font-size: 23px; white-space:normal;">
        {}
      </pre>
    </div>
  '''.format(insert)

if __name__ == "__main__":
  app.run()
```





