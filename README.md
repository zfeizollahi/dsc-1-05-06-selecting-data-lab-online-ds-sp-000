
# Selecting Data - Lab


For the old version of this lab go [here](https://github.com/learn-co-curriculum/dsc-1-05-06-selecting-data-lab-old).

## Introduction 

NASA wants to go to Mars! Before they build their rocket, NASA needs to track information about all of the planets in the Solar System. Use SQL to help NASA create, alter, and insert data into a database that stores all of this important information. Then practice querying the database with various `SELECT` statements. We will select different columns, and employ other SQL clauses like `WHERE` to return the data we would like.

![solar_system](https://bilingualcarloscano.files.wordpress.com/2010/05/venus.jpg)

## Objectives
You will be able to:
* Retrieve all the information from a table
* Retrieve a subset of records from a table using a `WHERE` clause
* Retrieve a subset of columns from a table

## Part 1: Create the Database

To start, let's create a database using sqlite3. To do this, import the package and create a connection as we did in the previous lecture. This method connects to a database if it exists, or creates a new one if none exists. In this case, we'll create a new database called **planets.db**.


```python
# Import sqlite3, then create the SQL database/connect to it (creating the database connection will create a DB)
import sqlite3
connection = sqlite3.connect('planets.db')
cursor = connection.cursor()
```

Now create a cursor object so that you can execute statements through your connection.


```python
#Your code here; create a cursor object so you can execute statement against the database.
```

## Part 2: Table setup

#### Create a table

Write the necessary SQL to create a table using the `CREATE TABLE` command. Call the table `planets`.

**Remember:** your create table statement should be formatted like the following:

```sql
CREATE TABLE table_name (
   # column_names and data types here
);
```

NASA is interested in comparing each planet across several characteristics.  They want to know each planet's name,  color, number of moons, and mass (relative to earth).  Your columns should be the following types:

|column | type  |
|-------|-------|
|id     |integer|
|name   |text   |
|color  |text   |
|num_of_moons  |integer|
|mass   | real  |

> **Notes:** Make sure to set the `id` column as the table's primary key.


```python
#Your code here; create the table as described above.
cursor.execute(''' 
    CREATE TABLE planets (
        id INTEGER PRIMARY KEY,
        name TEXT,
        color TEXT,
        num_of_moons INTEGER,
        mass REAL);''')
```




    <sqlite3.Cursor at 0x7fa8801777a0>



#### Alter the table

NASA notices that several of the planets have rings around them.  However, we do not have a column to keep track of this information.  Use the `ALTER TABLE` statement to add a column called `rings` with a data type of `boolean` to the `planets` table. Write the code below to use sqlite3 to execute that SQL query against your database.


```python
# Your code for reading and executing alter.sql
cursor.execute(''' ALTER TABLE planets ADD COLUMN rings BOOLEAN;''')
```




    <sqlite3.Cursor at 0x7fa8801777a0>



## Part 3: Add and remove data

#### Add data to the table

Populate the table with data for the nine planets that constitute the Solar System using the `INSERT INTO` command.  The relevant information is provided in the table below:

|name   |color |num_of_moons|mass|rings|
|-------|-------|-------|-------|-------|
|Mercury|gray   |0      |0.55   |no     |
|Venus  |yellow |0      |0.82   |no     |
|Earth  |blue   |1      |1.00   |no     |
|Mars   |red    |2      |0.11   |no     |
|Jupiter|orange |53     |317.90 |no     |
|Saturn |hazel  |62     |95.19  |yes    |
|Uranus |light blue|27  |14.54  |yes    |
|Neptune|dark blue|14   |17.15  |yes    |
|Pluto  |brown  |5      |0.003  |no     |

Refer to the [SQLite3 documentation](https://www.sqlite.org/datatype3.html) to remember how to express boolean values in SQLite3.

**Hint:** to save some tedious typing, feel free to open up this cell and copy and paste some of the text to modify in your insert command below.


```python
#Your code here; add data to the table
planets_add = [('Mercury','gray', 0, 0.55, False), 
('Venus', 'yellow', 0, 0.82, False),
('Earth', 'blue', 1, 1.00, False),
('Mars', 'red', 2, 0.11, False),
('Jupiter', 'orange', 53, 317.90, False),
('Saturn', 'hazel', 62, 95.19, True),
('Uranus', 'light blue', 27, 14.54, True),
('Neptune', 'dark blue', 14, 17.15, True),
('Pluto', 'brown', 5, 0.003, False)]

cursor.executemany('''INSERT INTO planets (name, color, num_of_moons, mass, rings) VALUES (?,?,?,?,?);''', 
                   planets_add)
```




    <sqlite3.Cursor at 0x7fa8801777a0>




```python
cursor.execute(''' SELECT * FROM planets''').fetchall()
```




    [(1, 'Mercury', 'gray', 0, 0.55, 0),
     (2, 'Venus', 'yellow', 0, 0.82, 0),
     (3, 'Earth', 'blue', 1, 1.0, 0),
     (4, 'Mars', 'red', 2, 0.11, 0),
     (5, 'Jupiter', 'orange', 53, 317.9, 0),
     (6, 'Saturn', 'hazel', 62, 95.19, 1),
     (7, 'Uranus', 'light blue', 27, 14.54, 1),
     (8, 'Neptune', 'dark blue', 14, 17.15, 1),
     (9, 'Pluto', 'brown', 5, 0.003, 0)]



#### Update table data

NASA has confirmed that Jupiter has another 15 moons! Write an `UPDATE` command so that Jupiter has 68 moons instead of 53.

> **Hint**: you probably need to use a `WHERE` statement to accomplish this task.


```python
# Your code to update the table
cursor.execute(''' UPDATE planets SET num_of_moons = 68 WHERE name = 'Jupiter'; ''')
```




    <sqlite3.Cursor at 0x7fa8801777a0>




```python
cursor.execute(''' SELECT * FROM planets WHERE name = 'Jupiter';  ''').fetchall()
```




    [(5, 'Jupiter', 'orange', 68, 317.9, 0)]



#### Remove data from the table

Wait just a moment!  NASA decided that Pluto is no longer a planet. Remove Pluto from the table using the `DELETE FROM` command.


```python
# Your code to delete pluto here
cursor.execute(''' DELETE FROM planets WHERE name = 'Pluto' ;''')
cursor.execute( ''' SELECT * FROM planets; ''').fetchall()
```




    [(1, 'Mercury', 'gray', 0, 0.55, 0),
     (2, 'Venus', 'yellow', 0, 0.82, 0),
     (3, 'Earth', 'blue', 1, 1.0, 0),
     (4, 'Mars', 'red', 2, 0.11, 0),
     (5, 'Jupiter', 'orange', 68, 317.9, 0),
     (6, 'Saturn', 'hazel', 62, 95.19, 1),
     (7, 'Uranus', 'light blue', 27, 14.54, 1),
     (8, 'Neptune', 'dark blue', 14, 17.15, 1)]



## Onto Selecting Data

We will be querying data from the `planets` table we just created. We can see it again below:

|name   |color |num_of_moons|mass|rings|
|-------|-------|-------|-------|-------|
|Mercury|gray   |0      |0.55   |no     |
|Venus  |yellow |0      |0.82   |no     |
|Earth  |blue   |1      |1.00   |no     |
|Mars   |red    |2      |0.11   |no     |
|Jupiter|orange |67     |317.90 |no     |
|Saturn |hazel  |62     |95.19  |yes    |
|Uranus |light blue|27  |14.54  |yes    |
|Neptune|dark blue|14   |17.15  |yes    |

Write SQL queries for each of the statements below. You can also preview the results as a nice pandas DataFrame if you want to see the output like this:

## Some Notes on Displaying Query Outputs

Let's take brief look at nicely displaying the results of the sql queries you are about to write. Specifically, we'll look at how to pipe your sql queries into Pandas so that you can use your standard data processing techniques and build your workflow into pipelines.


```python
import pandas as pd
```


```python
#Demonstrating running a query and previewing results as pandas DataFrame
results = cursor.execute("""select * from planets;""").fetchall()

#Alternatively we could do this in two steps:
#cur.execute("""select * from planets;""")
#results = cur.fetchall()
df = pd.DataFrame(results)
df.columns = [i[0] for i in cursor.description]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>name</th>
      <th>color</th>
      <th>num_of_moons</th>
      <th>mass</th>
      <th>rings</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Mercury</td>
      <td>gray</td>
      <td>0</td>
      <td>0.55</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Venus</td>
      <td>yellow</td>
      <td>0</td>
      <td>0.82</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Earth</td>
      <td>blue</td>
      <td>1</td>
      <td>1.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Mars</td>
      <td>red</td>
      <td>2</td>
      <td>0.11</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Jupiter</td>
      <td>orange</td>
      <td>68</td>
      <td>317.90</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



For simplicity, you may wish to make this process a function:


```python
def sql_select_to_df(SQL_COMMAND, cur=cursor):
    results = cur.execute(SQL_COMMAND).fetchall()
    df = pd.DataFrame(results)
    df.columns = [i[0] for i in cur.description]
    return df
```


```python
df = sql_select_to_df("""select * from planets;""")
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>name</th>
      <th>color</th>
      <th>num_of_moons</th>
      <th>mass</th>
      <th>rings</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Mercury</td>
      <td>gray</td>
      <td>0</td>
      <td>0.55</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Venus</td>
      <td>yellow</td>
      <td>0</td>
      <td>0.82</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Earth</td>
      <td>blue</td>
      <td>1</td>
      <td>1.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Mars</td>
      <td>red</td>
      <td>2</td>
      <td>0.11</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Jupiter</td>
      <td>orange</td>
      <td>68</td>
      <td>317.90</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



With that, it's time to practice your sql skills!

## Select just the name and color of each planet


```python
#Your code here
df = sql_select_to_df(''' SELECT name, color FROM planets;''')
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>color</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mercury</td>
      <td>gray</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Venus</td>
      <td>yellow</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Earth</td>
      <td>blue</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mars</td>
      <td>red</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Jupiter</td>
      <td>orange</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Saturn</td>
      <td>hazel</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Uranus</td>
      <td>light blue</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Neptune</td>
      <td>dark blue</td>
    </tr>
  </tbody>
</table>
</div>



## Select all columns for each planet whose mass is greater than 1.00



```python
#Your code here
results = sql_select_to_df(''' SELECT * FROM planets WHERE mass > 1.00 ;''')
results
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>name</th>
      <th>color</th>
      <th>num_of_moons</th>
      <th>mass</th>
      <th>rings</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>Jupiter</td>
      <td>orange</td>
      <td>68</td>
      <td>317.90</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6</td>
      <td>Saturn</td>
      <td>hazel</td>
      <td>62</td>
      <td>95.19</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7</td>
      <td>Uranus</td>
      <td>light blue</td>
      <td>27</td>
      <td>14.54</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8</td>
      <td>Neptune</td>
      <td>dark blue</td>
      <td>14</td>
      <td>17.15</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



## Select the name and mass of each planet whose mass is less than or equal to 1.00


```python
#Your code here
df = sql_select_to_df(''' SELECT name, mass FROM planets WHERE mass <= 1.00 ;''')
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>mass</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mercury</td>
      <td>0.55</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Venus</td>
      <td>0.82</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Earth</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mars</td>
      <td>0.11</td>
    </tr>
  </tbody>
</table>
</div>



## Select the name and color of each planet that has more than 10 moons


```python
#Your code here
df = sql_select_to_df(''' SELECT name, color FROM planets WHERE num_of_moons < 10 ;''')
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>color</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mercury</td>
      <td>gray</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Venus</td>
      <td>yellow</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Earth</td>
      <td>blue</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mars</td>
      <td>red</td>
    </tr>
  </tbody>
</table>
</div>



## Select the planet that has at least one moon and a mass less than 1.00


```python
#Your code here
df = sql_select_to_df(''' SELECT * FROM planets WHERE num_of_moons >= 1 AND mass < 1.00 ;''')
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>name</th>
      <th>color</th>
      <th>num_of_moons</th>
      <th>mass</th>
      <th>rings</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>Mars</td>
      <td>red</td>
      <td>2</td>
      <td>0.11</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## Select the name and color of planets that have a color of blue, light blue, or dark blue


```python
#Your code here
df = sql_select_to_df(''' SELECT name, color FROM planets WHERE color in ('blue', 'light blue', 'dark blue');''')
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>color</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Earth</td>
      <td>blue</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Uranus</td>
      <td>light blue</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Neptune</td>
      <td>dark blue</td>
    </tr>
  </tbody>
</table>
</div>



## Summary

Congratulations! NASA is one step closer to embarking upon its mission to Mars. In this lab, we created a table to track all the planets in the solar system, altered the table to include another column, inserted values to populate the table, and we deleted data from the table. Then we practiced writing select statements that query a single table to get specific information. We also used other clauses and specified column names to cherry pick the data we wanted to retrieve. 
