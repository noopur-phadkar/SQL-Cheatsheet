# SQL-Cheatsheet

Resources-

1. https://sqlbolt.com/lesson/introduction
2. https://www.techagilist.com/mainframe/db2/db2-join-inner-joins-and-outer-joins/
3. https://www.w3schools.com/sql/default.asp
4. https://learn.microsoft.com/en-us/power-query/merge-queries-left-outer

# What is SQL?

SQL, or Structured Query Language, is a language designed to allow both technical and non-technical users query, manipulate, and transform data from a relational database. And due to its simplicity, SQL databases provide safe and scalable storage for millions of websites and mobile applications.

# What is a Relational Database?

A relational database is a type of database that stores and provides access to data points that are related to one another. Relational [databases](https://www.oracle.com/database/what-is-database/) are based on the relational model, an intuitive, straightforward way of representing data in tables. In a relational database, each row in the table is a record with a unique ID called the key. The columns of the table hold attributes of the data, and each record usually has a value for each attribute, making it easy to establish the relationships among data points.

<aside>
💡 [https://www.oracle.com/database/what-is-a-relational-database/](https://www.oracle.com/database/what-is-a-relational-database/)

</aside>

The relational model means that the logical data structures—the data tables, views, and indexes—are separate from the physical storage structures. This separation means that database administrators can manage physical data storage without affecting access to that data as a logical structure. For example, renaming a database file does not rename the tables stored within it.

# Types of SQL Queries

In the realm of relational databases, SQL (Structured Query Language) serves as the fundamental tool for interacting with and managing data. SQL commands are broadly categorized into 

## Data Definition Language (DDL) - shape the database structure

- **CREATE**: Used to create database objects like tables, views, or indexes.
- **ALTER**: Modifies the structure of existing database objects.
- **DROP**: Deletes database objects, such as tables or views.
- **TRUNCATE**: Removes all records from a table but retains the table structure.

## Data Manipulation Language (DML) - handle data modification

- **SELECT**: Retrieves data from one or more tables.
- **INSERT**: Adds new records into a table.
- **UPDATE**: Modifies existing records in a table.
- **DELETE**: Removes records from a table.

## Data Control Language (DCL) - manage access and permissions

- **GRANT**: Provides specific privileges to database users.
- **REVOKE**: Withdraws previously granted privileges.

## Transaction Control Language (TCL) - ensure transaction integrity

- **COMMIT**: Finalizes a transaction, making all changes permanent.
- **ROLLBACK**: Reverts the database to its state before the beginning of a transaction.
- **SAVEPOINT**: Sets points within transactions to which you can later roll back.

## Data Query Language (DQL) - facilitate data retrieval

- **SELECT**: Primarily used for querying the database to retrieve specific information.

# Basic Queries

## SELECT

```sql
SELECT column, another_column, … 
	FROM mytable;
```

## WHERE - Queries with constraints

```sql
SELECT column, another_column, …
	FROM mytable
		WHERE condition
	    AND/OR another_condition
	    AND/OR …;
```

Integer Operators -

| Operator | Condition | SQL Example |
| --- | --- | --- |
| =, !=, < <=, >, >= | Standard numerical operators | col_name != 4 |
| BETWEEN … AND … | Number is within range of two values (inclusive) | col_name BETWEEN 1.5 AND 10.5 |
| NOT BETWEEN … AND … | Number is not within range of two values (inclusive) | col_name NOT BETWEEN 1 AND10 |
| IN (…) | Number exists in a list | col_name IN (2, 4, 6) |
| NOT IN (…) | Number does not exist in a list | col_name NOT IN (1, 3, 5) |

String Operators -

| Operator | Condition | Example |
| --- | --- | --- |
| = | Case sensitive exact string comparison (notice the single equals) | col_name = "abc" |
| != or <> | Case sensitive exact string inequality comparison | col_name != "abcd" |
| LIKE | Case insensitive exact string comparison | col_name LIKE "ABC" |
| NOT LIKE | Case insensitive exact string inequality comparison | col_name NOT LIKE "ABCD" |
| % | Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE) | col_name LIKE "%AT%"(matches "AT", "ATTIC", "CAT" or even "BATS") |
| _ | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE) | col_name LIKE "AN_"(matches "AND", but not "AN") |
| IN (…) | String exists in a list | col_name IN ("A", "B", "C") |
| NOT IN (…) | String does not exist in a list | col_name NOT IN ("D", "E", "F") |

## DISTINCT - discard rows that have a duplicate column value

```sql
SELECT DISTINCT column, another_column, …
	FROM mytable
		WHERE condition(s);
```

## ORDER - sort alpha-numerically based on the specified column's value

```sql
SELECT column, another_column, …
	FROM mytable
		WHERE condition(s)
			ORDER BY column ASC/DESC;
```

## LIMIT and OFFSET

**`LIMIT`** will reduce the number of rows to return, and the optional **`OFFSET`** will specify where to begin counting the number rows from.

```sql
SELECT column, another_column, …
	FROM mytable
		WHERE condition(s)
			ORDER BY column ASC/DESC
				LIMIT num_limit OFFSET num_offset;
```

Example - List the first five movies sorted alphabetically

```sql
SELECT * 
	FROM movies 
		ORDER BY title asc 
			LIMIT 5;
```

Example - List the next five movies sorted alphabetically

```sql
SELECT * 
	FROM movies 
		ORDER BY title asc 
			LIMIT 5 OFFSET 5;
```

## Exercise

The table instead contains information about a few of the most populous cities of North America[[1]](http://en.wikipedia.org/wiki/List_of_North_American_cities_by_population) including their population and geo-spatial location in the world.

Positive latitudes correspond to the northern hemisphere, and positive longitudes correspond to the eastern hemisphere. Since North America is north of the equator and west of the prime meridian, all of the cities in the list have positive latitudes and negative longitudes.

Table Subset: north_american_cities

| City | Country | Population | Latitude | Longitude |
| --- | --- | --- | --- | --- |
| Guadalajara | Mexico | 1500800 | 20.659699 | -103.349609 |
| Toronto | Canada | 2795060 | 43.653226 | -79.383184 |
| Houston | United States | 2195914 | 29.760427 | -95.369803 |
| New York | United States | 8405837 | 40.712784 | -74.005941 |

### List all the Canadian cities and their populations

```sql
SELECT * 
	FROM north_american_cities 
		WHERE Country = "Canada";
```

### Order all the cities in the United States by their latitude from north to south

```sql
SELECT * 
	FROM north_american_cities 
		WHERE Country = "United States" 
			ORDER BY Latitude DESC;
```

### List all the cities west of Chicago, ordered from west to east

```sql
SELECT * 
	FROM north_american_cities 
		WHERE Longitude < -87.629798 
			ORDER BY Longitude ASC;
```

### List the two largest cities in Mexico (by population)

```sql
SELECT * 
	FROM north_american_cities 
		WHERE Country = "Mexico" 
			ORDER BY Population DESC 
				LIMIT 2;
```

### List the third and fourth largest cities (by population) in the United States and their population

```sql
SELECT * 
	FROM north_american_cities 
		WHERE Country = "United States" 
			ORDER BY Population DESC 
				LIMIT 2 OFFSET 2;
```

# Normalization

<aside>
💡 Process of breaking down entity data into pieces and stored across multiple tables.

</aside>

Database normalization is useful because it minimizes duplicate data in any single table, and allows for data in the database to grow independently of each other (ie. Types of car engines can grow independent of each type of car). As a trade-off, queries get slightly more complex since they have to be able to find data from different parts of the database, and performance issues can arise when working with many large tables.

In order to answer questions about an entity that has data spanning multiple tables in a normalized database, we need to learn how to write a query that can combine all that data and pull out exactly the information we need.

# **Types of Keys in Relational Model**

## Candidate Key

The minimal set of attributes that can uniquely identify a tuple is known as a candidate key.

- It is a minimal super key.
- It is a super key with no repeated data is called a candidate key.
- The minimal set of attributes that can uniquely identify a record.
- It must contain unique values.
- It can contain NULL values.
- Every table must have at least a single candidate key.
- A table can have multiple candidate keys but only one primary key.
- The value of the Candidate Key is unique and may be null for a tuple.
- There can be more than one candidate key in a relationship.

## Primary Key

- It is a unique key.
- It can identify only one tuple (a record) at a time.
- It has no duplicate values, it has unique values.
- It cannot be NULL.
- Primary keys are not necessarily to be a single column; more than one column can also be a primary key for a table.
- One common primary key type is an auto-incrementing integer (because they are space efficient), but it can also be a string, hashed value, so long as it is unique.

## Super Key

A super key is a group of single or multiple keys that identifies rows in a table. It supports NULL values.

- Adding zero or more attributes to the candidate key generates the super key.
- A candidate key is a super key but vice versa is not true.
- Super Key values may also be NULL.

## Alternate Key

The candidate key other than the primary key is called an alternate key.

- All the keys which are not primary keys are called alternate keys.
- It is a secondary key.
- It contains two or more fields to identify two or more records.
- These values are repeated.

![Primary-key-alternative-key-in-dbms](https://github.com/user-attachments/assets/38718d28-e42a-4230-8e02-31d04add31e9)

## Foreign Key

If an attribute can only take the values which are present as values of some other attribute, it will be a [**foreign key**](https://www.geeksforgeeks.org/foreign-key-constraint-in-sql/) to the attribute to which it refers. The relation which is being referenced is called referenced relation and the corresponding attribute is called referenced attribute. The referenced attribute of the referenced relation should be the primary key to it.

- It is a key it acts as a primary key in one table and it acts as secondary key in another table.
- It combines two or more relations (tables) at a time.
- They act as a cross-reference between the tables.
- For example, DNO is a primary key in the DEPT table and a non-key in EMP
- They can be NULL
- They may contain duplicate tuples i.e. it need not follow uniqueness constraint.

![Foreign-keys](https://github.com/user-attachments/assets/9a5f70fd-6e8a-4bec-b5ce-a344d974f4c3)

## Composite Key

Sometimes, a table might not have a single column/attribute that uniquely identifies all the records of a table. To uniquely identify rows of a table, a combination of two or more columns/attributes can be used.  It still can give duplicate values in rare cases. So, we need to find the optimal set of attributes that can uniquely identify rows in a table.

- It acts as a primary key if there is no primary key in a table
- Two or more attributes are used together to make a [**composite key**](https://www.geeksforgeeks.org/composite-key-in-sql/).
- Different combinations of attributes may give different accuracy in terms of identifying the rows uniquely.

![Different-types-of-keys](https://github.com/user-attachments/assets/08044c24-2f75-4020-8103-09092345ac85)

<aside>
💡 Candidate keys allow for distinct identification, the Primary key serves as the chosen identifier, Alternate keys offer other choices, and Foreign keys create vital linkages that guarantee data integrity between tables. The creation of strong and effective relational databases requires the thoughtful application of these keys.

</aside>

# Joins

<aside>
💡 Tables that share information about a single entity need to have a *primary key* that identifies that entity *uniquely* across the database.

</aside>

## Inner Join

```sql
SELECT column, another_table_column, …
	FROM mytable
		INNER JOIN another_table 
	    ON mytable.id = another_table.id
				WHERE condition(s)
					ORDER BY column, … ASC/DESC
						LIMIT num_limit OFFSET num_offset;
```

The **`INNER JOIN`** is a process that matches rows from the first table and the second table which have the same key (as defined by the **`ON`** constraint) to create a result row with the combined columns from both tables.

<img width="500" alt="inner-join-operation" src="https://github.com/user-attachments/assets/d6ca0061-b6d4-4747-8dab-8053e785d737">

```sql
SELECT * 
	FROM left_table 
		INNER JOIN right_table 
			WHERE left_table.countryID=right_table.ID;
```

## Full Join

```sql
SELECT column, another_column, …
	FROM mytable
		FULL JOIN another_table 
	    ON mytable.id = another_table.matching_id
				WHERE condition(s)
					ORDER BY column, … ASC/DESC
						LIMIT num_limit OFFSET num_offset;
```

This is an outer join, as known as **`FULL OUTER JOIN` .**

A **`FULL JOIN`** simply means that rows from both tables are kept, regardless of whether a matching row exists in the other table.

<img width="500" alt="full-outer-join-operation" src="https://github.com/user-attachments/assets/3d2d77cd-1753-4f05-aae1-4016111b2b90">


```sql
SELECT * 
	FROM left_table 
		FULL JOIN right_table 
			ON left_table.countryID = right_table.ID;
```

## Left Join

```sql
SELECT column, another_column, …
	FROM mytable
		LEFT JOIN another_table 
	    ON mytable.id = another_table.matching_id
				WHERE condition(s)
					ORDER BY column, … ASC/DESC
						LIMIT num_limit OFFSET num_offset;
```

This is an outer join, as known as **`LEFT OUTER JOIN` .**

A **`LEFT JOIN`** simply includes rows from A regardless of whether a matching row is found in B

<img width="500" alt="left-outer-join-operation" src="https://github.com/user-attachments/assets/063178f8-5606-47d9-afa1-702d82518a4c">

```sql
SELECT * 
	FROM left_table 
		LEFT JOIN right_table 
			WHERE left_table.countryID = right_table.ID;
```

## Right Join

```sql
SELECT column, another_column, …
	FROM mytable
		RIGHT JOIN another_table 
	    ON mytable.id = another_table.matching_id
				WHERE condition(s)
					ORDER BY column, … ASC/DESC
						LIMIT num_limit OFFSET num_offset;
```

This is an outer join, as known as **`RIGHT OUTER JOIN` .**

The `RIGHT JOIN`  returns all records from the table B, and the matching records from the table A. The result is 0 records from the left side, if there is no match.

![right-outer-join-operation](https://github.com/user-attachments/assets/03296995-41ca-429b-888b-9194a6e1373c)

```sql
SELECT * 
	FROM left_table 
		RIGHT JOIN right_table 
			WHERE left_table.countryID=right_table.ID;
```

## Exercise 1

Table: Movies

| Id | Title | Director | Year | Length_minutes |
| --- | --- | --- | --- | --- |
| 1 | Toy Story | John Lasseter | 1995 | 81 |
| 2 | A Bug's Life | John Lasseter | 1998 | 95 |
| 3 | Toy Story 2 | John Lasseter | 1999 | 93 |
| 4 | Monsters, Inc. | Pete Docter | 2001 | 92 |
| 5 | Finding Nemo | Andrew Stanton | 2003 | 107 |
| 6 | The Incredibles | Brad Bird | 2004 | 116 |

Table: Boxoffice

| Movie_id | Rating | Domestic_sales | International_sales |
| --- | --- | --- | --- |
| 5 | null | 380843261 | 555900000 |
| 14 | 7.4 | 268492764 | 475066843 |
| 8 | 8 | 206445654 | 417277164 |
| 12 | 6.4 | 191452396 | 368400000 |
| 3 | null | 245852179 | 239163000 |
| 6 | 8 | 261441092 | 370001000 |

### Find the domestic and international sales for each movie

```sql
SELECT * 
	FROM movies 
		INNER JOIN boxoffice 
			ON movies.id = boxoffice.movie_id;
```

### Show the sales numbers for each movie that did better internationally rather than domestically

```sql
SELECT title, boxoffice.domestic_sales, boxoffice.international_sales 
	FROM movies 
		INNER JOIN boxoffice 
			ON movies.id = boxoffice.movie_id 
				WHERE 
		boxoffice.international_sales > boxoffice.domestic_sales;
```

### List all the movies by their ratings in descending order

```sql
SELECT * 
	FROM movies 
		INNER JOIN boxoffice 
			ON movies.id = boxoffice.movie_id 
				ORDER BY boxoffice.rating DESC;
```

### Find all information for each movie and sort by title

```sql
SELECT * 
	FROM movies
		FULL OUTER JOIN boxoffice 
			ON movies.id = boxoffice.movie_id
				ORDER BY movies.title ASC;
```

## Exercise 2

Table: Customers

| CustomerID | CustomerName | ContactName | Address |
| --- | --- | --- | --- |
| 1 | Alfreds Futterkiste | Maria Anders | Obere Str. 57 |
| 2 | Ana Trujillo Emparedados y helados | Ana Trujillo | Avda. de 222 |
| 3 | Antonio Moreno Taquería | Antonio Moreno | Mataderos 2312 |

Table: Orders

| OrderID | CustomerID | EmployeeID | OrderDate | ShipperID |
| --- | --- | --- | --- | --- |
| 10308 | 2 | 7 | 1996-09-18 | 3 |
| 10309 | 37 | 3 | 1996-09-19 | 1 |
| 10310 | 77 | 8 | 1996-09-20 | 2 |

### Display customer names and order ID, and any orders they might have:

```sql
SELECT customers.CustomerName, orders.OrderID 
	FROM customers
		LEFT JOIN orders 
			ON customers.CustomerId = orders.CustomerID;
```

### Display customer names and order ID, and any orders they might have:

```sql
SELECT customers.CustomerName, orders.OrderID 
	FROM customers
		RIGHT JOIN orders 
			ON customers.CustomerId = orders.CustomerID;
```

## Exercise 3

Table: Buildings (Read-Only)

| Building_name | Capacity |
| --- | --- |
| 1e | 24 |
| 1w | 32 |
| 2e | 16 |
| 2w | 20 |

Table: Employees (Read-Only)

| Role | Name | Building | Years_employed |
| --- | --- | --- | --- |
| Engineer | Becky A. | 1e | 4 |
| Engineer | Dan B. | 1e | 2 |
| Engineer | Sharon F. | 1e | 6 |
| Engineer | Dan M. | 1e | 4 |

### Find the list of all buildings that have employees

```sql
SELECT DISTINCT building_name from buildings 
	LEFT JOIN employees 
		ON buildings.building_name=employees.building 
			WHERE employees.building;
```

### Find the list of all buildings and their capacity

```sql
SELECT DISTINCT building_name, capacity 
	FROM buildings;
```

### List all buildings and the distinct employee roles in each building (including empty buildings)

```sql
SELECT DISTINCT employees.role, buildings.building_name 
	FROM buildings 
		LEFT JOIN employees 
			ON buildings.building_name=employees.building;
```

# Nulls

It's always good to reduce the possibility of **`NULL`** values in databases because they require special attention when constructing queries, constraints (certain functions behave differently with null values) and when processing the results.

An alternative to **`NULL`** values in your database is to have ***data-type appropriate default values***, like 0 for numerical data, empty strings for text data, etc.

But if your database needs to store incomplete data, then **`NULL`** values can be appropriate if the default values will skew later analysis (for example, when taking averages of numerical data).

```sql
SELECT column, another_column, …
	FROM mytable
		WHERE column IS/IS NOT NULL
			AND/OR another_condition
			AND/OR …;
```

# **Queries with expressions**

Here, we use **`expressions`**  to write more complex logic on column values in a query. These expressions can use mathematical and string functions along with basic arithmetic to transform values when the query is executed.

In addition to expressions, regular columns and even tables can also have aliases to make them easier to reference in the output and as a part of simplifying more complex queries.

```sql
SELECT col_expression AS expr_description, …
	FROM mytable;
```

The use of expressions can save time and extra post-processing of the result data, but can also make the query harder to read, so when expressions are used in the **SELECT** part of the query, that they are also given a descriptive *alias* using the **AS** keyword.

```sql
SELECT column AS better_column_name, …
	FROM a_long_widgets_table_name AS mywidgets
		INNER JOIN widget_sales
		  ON mywidgets.id = widget_sales.widget_id;
```

## Example

```sql
SELECT particle_speed / 2.0 AS half_particle_speed
	FROM physics_data
		WHERE ABS(particle_position) * 10.0 > 500;
```

## Exercise

Table: Movies (Read-Only)

| Id | Title | Director | Year | Length_minutes |
| --- | --- | --- | --- | --- |
| 1 | Toy Story | John Lasseter | 1995 | 81 |
| 2 | A Bug's Life | John Lasseter | 1998 | 95 |
| 3 | Toy Story 2 | John Lasseter | 1999 | 93 |
| 4 | Monsters, Inc. | Pete Docter | 2001 | 92 |
| 5 | Finding Nemo | Andrew Stanton | 2003 | 107 |
| 6 | The Incredibles | Brad Bird | 2004 | 116 |

Table: Boxoffice (Read-Only)

| Movie_id | Rating | Domestic_sales | International_sales |
| --- | --- | --- | --- |
| 5 | 8.2 | 380843261 | 555900000 |
| 14 | 7.4 | 268492764 | 475066843 |
| 8 | 8 | 206445654 | 417277164 |
| 12 | 6.4 | 191452396 | 368400000 |
| 3 | 7.9 | 245852179 | 239163000 |
| 6 | 8 | 261441092 | 370001000 |

### List all movies and their combined sal\es in **millions** of dollars

```sql
SELECT title, (boxoffice.domestic_sales + boxoffice.international_sales)/1000000 AS total_sales 
	FROM movies 
		LEFT JOIN boxoffice 
			ON movies.id=boxoffice.movie_id;
```

### List all movies and their ratings **in percent**

```sql
SELECT title, boxoffice.rating * 10 AS total_sales 
	FROM movies 
		LEFT JOIN boxoffice 
			ON movies.id=boxoffice.movie_id;
```

### List all movies that were released on even number years

```sql
SELECT title 
	FROM movies 
		LEFT JOIN boxoffice 
			ON movies.id=boxoffice.movie_id 
				WHERE movies.year % 2 == 0;
```

# Queries with Aggregates

SQL supports the use of aggregate expressions (or functions) that allow you to summarize information about a group of rows of data.

```sql
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
	FROM mytable
		WHERE constraint_expression;
```

Without a specified grouping, each aggregate function is going to run on the whole set of result rows and return a single value.

| Function | Description |
| --- | --- |
| COUNT(*), COUNT(column) | A common function used to counts the number of rows in the group if no column name is specified. Otherwise, count the number of rows in the group with non-NULL values in the specified column. |
| MIN(column) | Finds the smallest numerical value in the specified column for all rows in the group. |
| MAX(column) | Finds the largest numerical value in the specified column for all rows in the group. |
| AVG(column) | Finds the average numerical value in the specified column for all rows in the group. |
| SUM(column) | Finds the sum of all numerical values in the specified column for the rows in the group. |

## Grouped Aggregate Functions

In addition to aggregating across all the rows, you can instead apply the aggregate functions to individual groups of data within that group (ie. box office sales for Comedies vs Action movies).

This would then create as many results as there are unique groups defined as by the **`GROUP BY`** clause.

The **`GROUP BY`** clause works by grouping rows that have the same value in the column specified.

```sql
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
	FROM mytable
		WHERE constraint_expression
			GROUP BY column;
```

## Exercise

Table: Employees

| Role | Name | Building | Years_employed |
| --- | --- | --- | --- |
| Engineer | Becky A. | 1e | 4 |
| Engineer | Dan B. | 1e | 2 |
| Engineer | Sharon F. | 1e | 6 |
| Engineer | Dan M. | 1e | 4 |
| Engineer | Malcom S. | 1e | 1 |

### Find the longest time that an employee has been at the studio

```sql
SELECT MAX(years_employed) 
	FROM employees;
```

### For each role, find the average number of years employed by employees in that role

```sql
SELECT role, AVG(years_employed) 
	FROM employees 
		GROUP BY role;
```

### Find the total number of employee years worked in each building

```sql
SELECT building, SUM(years_employed) 
	FROM employees 
		GROUP BY building;
```

## HAVING Clause

If the **`GROUP BY`**clause is executed after the **`WHERE`** clause (which filters the rows which are to be grouped), then how exactly do we filter the grouped rows?

SQL allows us to do this by adding an additional **`HAVING`** clause which is used specifically with the **`GROUP BY`** clause to allow us to filter grouped rows from the result set.

```sql
SELECT group_by_column, AGG_FUNC(column_expression) AS aggregate_result_alias, …
	FROM mytable
		WHERE condition
			GROUP BY column
				HAVING group_condition;
```

If you aren't using the `GROUP BY` clause, a simple `WHERE` clause will suffice for this purpose.

## Exercise

Table: Employees

| Role | Name | Building | Years_employed |
| --- | --- | --- | --- |
| Engineer | Becky A. | 1e | 4 |
| Engineer | Dan B. | 1e | 2 |
| Engineer | Sharon F. | 1e | 6 |
| Engineer | Dan M. | 1e | 4 |
| Engineer | Malcom S. | 1e | 1 |

### Find the number of Artists in the studio (without a **HAVING** clause)

```sql
SELECT COUNT() FROM employees 
    WHERE role="Artist"
        GROUP BY building;
```

### Find the number of Employees of each role in the studio

```sql
SELECT role, COUNT(*)
	FROM employees
		GROUP BY role;
```

### Find the total number of years employed by all Engineers

```sql
SELECT SUM(years_employed)
	FROM employees
		GROUP BY role
			HAVING role="Engineer";
```

# Order Of Execution

Complete Query:

```sql
SELECT DISTINCT column, AGG_FUNC(column_or_expression), …
FROM mytable
    JOIN another_table
      ON mytable.column = another_table.column
    WHERE constraint_expression
    GROUP BY column
    HAVING constraint_expression
    ORDER BY column ASC/DESC
    LIMIT count OFFSET COUNT;
```

Each query begins with finding the data that we need in a database, and then filtering that data down into something that can be processed and understood as quickly as possible.

## 1. **`FROM`** and **`JOIN`**s

The **`FROM`** clause, and subsequent **`JOIN`**s are first executed to determine the total working set of data that is being queried. This includes subqueries in this clause, and can cause temporary tables to be created under the hood containing all the columns and rows of the tables being joined.

## 2. **`WHERE`**

Once we have the total working set of data, the first-pass **`WHERE`** constraints are applied to the individual rows, and rows that do not satisfy the constraint are discarded. Each of the constraints can only access columns directly from the tables requested in the **`FROM`** clause. Aliases in the **`SELECT`** part of the query are not accessible in most databases since they may include expressions dependent on parts of the query that have not yet executed.

## 3. **`GROUP BY`**

The remaining rows after the **`WHERE`** constraints are applied are then grouped based on common values in the column specified in the **`GROUP BY`** clause. As a result of the grouping, there will only be as many rows as there are unique values in that column. Implicitly, this means that you should only need to use this when you have aggregate functions in your query.

## 4. **`HAVING`**

If the query has a **`GROUP BY`** clause, then the constraints in the **`HAVING`** clause are then applied to the grouped rows, discard the grouped rows that don't satisfy the constraint. Like the **`WHERE`** clause, aliases are also not accessible from this step in most databases.

## 5. **`SELECT`**

Any expressions in the **`SELECT`** part of the query are finally computed.

## 6. **`DISTINCT`**

Of the remaining rows, rows with duplicate values in the column marked as **`DISTINCT`**will be discarded.

## 7. **`ORDER BY`**

If an order is specified by the **`ORDER BY`** clause, the rows are then sorted by the specified data in either ascending or descending order. Since all the expressions in the **`SELECT`** part of the query have been computed, you can reference aliases in this clause.

## 8. **`LIMIT`** / **`OFFSET`**

Finally, the rows that fall outside the range specified by the **`LIMIT`** and **`OFFSET`** are discarded, leaving the final set of rows to be returned from the query.

## Exercise

Table: Movies (Read-Only)

| Id | Title | Director | Year | Length_minutes |
| --- | --- | --- | --- | --- |
| 1 | Toy Story | John Lasseter | 1995 | 81 |
| 2 | A Bug's Life | John Lasseter | 1998 | 95 |
| 3 | Toy Story 2 | John Lasseter | 1999 | 93 |
| 4 | Monsters, Inc. | Pete Docter | 2001 | 92 |
| 5 | Finding Nemo | Andrew Stanton | 2003 | 107 |

Table: Boxoffice (Read-Only)

| Movie_id | Rating | Domestic_sales | International_sales |
| --- | --- | --- | --- |
| 5 | 8.2 | 380843261 | 555900000 |
| 14 | 7.4 | 268492764 | 475066843 |
| 8 | 8 | 206445654 | 417277164 |
| 12 | 6.4 | 191452396 | 368400000 |
| 3 | 7.9 | 245852179 | 239163000 |
| 6 | 8 | 261441092 | 370001000 |

### Find the total domestic and international sales that can be attributed to each director

```sql
SELECT director, SUM(boxoffice.domestic_sales + boxoffice.international_sales) AS total_sales 
    FROM movies
	    LEFT JOIN boxoffice
		    ON movies.id=boxoffice.movie_id
			    GROUP BY director;
```

# Create Table

```sql
CREATE TABLE IF NOT EXISTS mytable (
    column DataType TableConstraint DEFAULT default_value,
    another_column DataType TableConstraint DEFAULT default_value,
    …
);
```

## DataTypes

| Data type | Description |
| --- | --- |
| INTEGER, BOOLEAN | The integer datatypes can store whole integer values like the count of a number or an age. In some implementations, the boolean value is just represented as an integer value of just 0 or 1. |
| FLOAT, DOUBLE, REAL | The floating point datatypes can store more precise numerical data like measurements or fractional values. Different types can be used depending on the floating point precision required for that value. |
| CHARACTER(num_chars), VARCHAR(num_chars), TEXT | The text based datatypes can store strings and text in all sorts of locales. The distinction between the various types generally amount to underlaying efficiency of the database when working with these columns.
Both the CHARACTER and VARCHAR (variable character) types are specified with the max number of characters that they can store (longer values may be truncated), so can be more efficient to store and query with big tables. |
| DATE, DATETIME | SQL can also store date and time stamps to keep track of time series and event data. They can be tricky to work with especially when manipulating data across timezones. |
| BLOB | Finally, SQL can store binary data in blobs right in the database. These values are often opaque to the database, so you usually have to store them with the right metadata to requery them. |

## Table Constraints

| Constraint | Description |
| --- | --- |
| PRIMARY KEY | This means that the values in this column are unique, and each value can be used to identify a single row in this table. |
| AUTOINCREMENT | For integer values, this means that the value is automatically filled in and incremented with each row insertion. Not supported in all databases. |
| UNIQUE | This means that the values in this column have to be unique, so you can't insert another row with the same value in this column as another row in the table. Differs from the `PRIMARY KEY` in that it doesn't have to be a key for a row in the table. |
| NOT NULL | This means that the inserted value can not be `NULL`. |
| CHECK (expression) | This allows you to run a more complex expression to test whether the values inserted are valid. For example, you can check that values are positive, or greater than a specific size, or start with a certain prefix, etc. |
| FOREIGN KEY | This is a consistency check which ensures that each value in this column corresponds to another value in a column in another table.For example, if there are two tables, one listing all Employees by ID, and another listing their payroll information, the `FOREIGN KEY` can ensure that every row in the payroll table corresponds to a valid employee in the master Employee list. |

# Insert Row

```sql
INSERT INTO mytable
(column, another_column, …)
VALUES (value_or_expr, another_value_or_expr, …),
      (value_or_expr_2, another_value_or_expr_2, …),
      …;
```

# Update Row

```sql
UPDATE mytable
SET column = value_or_expr, 
    other_column = another_value_or_expr, 
    …
WHERE condition;
```

# Delete Row

```sql
DELETE FROM mytable
WHERE condition;
```

# Alter Table

## Adding Columns

```sql
ALTER TABLE mytable
ADD column_name DataType OptionalTableConstraint 
    DEFAULT default_value;
```

## Removing Columns

```sql
ALTER TABLE mytable
DROP column_to_be_deleted;
```

## Renaming Table

```sql
ALTER TABLE mytable
RENAME TO new_table_name;
```

# Drop Table

```sql
DROP TABLE IF EXISTS mytable;
```

<aside>
💡  If we have another table that is dependent on columns in table you are removing (for example, with a **`FOREIGN KEY`** dependency) then you will have to either update all dependent tables first to remove the dependent rows or to remove those tables entirely.

</aside>

# Nested Queries / Subqueries

## Example

Lets say your company has a list of all Sales Associates, with data on the revenue that each Associate brings in, and their individual salary. Times are tight, and you now want to find out which of your Associates are costing the company more than the average revenue brought per Associate.

First, you would need to calculate the average revenue all the Associates are generating:

```sql
SELECT AVG(revenue_generated)
FROM sales_associates;
```

And then using that result, we can then compare the costs of each of the Associates against that value. To use it as a subquery, we can just write it straight into the **`WHERE`** clause of the query:

```sql
SELECT *
FROM sales_associates
WHERE salary > 
   **(SELECT AVG(revenue_generated)
    FROM sales_associates)**;
```

As the constraint is executed, each Associate's salary will be tested against the value queried from the inner subquery.

## Correlated subqueries

A more powerful type of subquery is the *correlated subquery* in which the inner query references, and is dependent on, a column or alias from the outer query. Unlike the subqueries above, each of these inner queries need to be run for each of the rows in the outer query, since the inner query is dependent on the current outer query row.

## Example

Instead of the list of just Sales Associates above, imagine if you have a general list of Employees, their departments (engineering, sales, etc.), revenue, and salary. This time, you are now looking across the company to find the employees who perform worse than average in their department.

For each employee, you would need to calculate their cost relative to the average revenue generated by all people in their department. To take the average for the department, the subquery will need to know what department each employee is in:

```sql
SELECT *
FROM employees
WHERE salary > 
   (SELECT AVG(revenue_generated)
    FROM employees AS dept_employees
    **WHERE dept_employees.department = employees.department**);
```

## Existence tests - Subquery with IN

```sql
SELECT *, …
FROM mytable
WHERE column
    IN/NOT IN (SELECT another_column
               FROM another_table);
```

# TODO

1. Grant
2. Revoke
3. Commit
4. Rollback
5. Savepoint
6. Union
7. Intersection
8. Exceptions ??
9. Anti-joins
