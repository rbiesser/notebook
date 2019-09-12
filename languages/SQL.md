SQL
===

The name SQL stands for Structured Query Language. It is pronounced “S-Q-L” and can also be pronounced “sequel.”

SQL is a computer language designed to get information from data that is stored in a relational database. Working with a database follows the same basic `CRUD (Create, Read, Update, Delete)` operations. You can then define that operations act on the structure of a table or they act on the data contained within the table.

Standard SQL should be the same for any relational database, however, view the [list of databases](../applications/databases.md) for implementations that deviate from the standard for a specific database.

[Overview of the select statement](#overview-of-the-select-statement)  
[Create Tables and Views](#create-tables-and-views)  
[Deleting a table](#deleting-a-table)  
Updating Tables  
[Adding one new row to a table](#adding-one-new-row-to-a-table)  
[Changing data in the rows already in a table](#changing-data-in-the-rows-already-in-a-table)  
[Deleting rows from a table](#deleting-rows-from-a-table)


Definitions
---
<dl>
    <dt>relational database</dt>
        <dd>A relational database is one way to organize data in a computer.</dd>
    <dt>table</dt>
        <dd>In a relational database, all the data is stored in tables. A table is a two-dimensional structure that has <b>columns</b> and <b>rows</b>. Using more traditional computer terminology, the columns are called <b>fields</b> and the rows are called <b>records</b>. You can use either terminology.</dd>
    <dt>column</dt>
        <dd>A column contains one particular type of information that is kept about all the rows in the table.</dd>
    <dt>row</dt>
        <dd>Each row of a table represents one object, event, or relationship</dd>
    <dt>cell</dt>
        <dd>A cell occurs where one row meets with one column. It is the smallest part of a table and it cannot be broken down into smaller parts. A cell contains one single piece of data.</dd>
    <dt>primary key</dt>
        <dd>Most tables contain a primary key that identifies each row in the table and gives it a name. A primary key must be unique and cannot be null.</dd>
    <dt>surrogate key</dt>
        <dd>A surrogate key is also called a meaningless primary key and is used in place of a complex primary key or as a convenient name for a row and has no intrinsic meaning related to the record.</dd> 
    <dt>data dictionary</dt>
        <dd>The Data Dictionary is a set of tables that contains all the information about the structure of the database. It contains the names of all the tables, their columns, their primary keys, the names of the views, the select statements that define the views, and much more. The Data Dictionary is sometimes called the System Catalog. Most SQL products have a Data Dictionary.</dd> 
    <dt>constraint</dt>
        <dd>A constraint is any rule that restricts the data that can be entered into a column</dd>
</dl>

Data Types
---
All datatypes used by the various RDBMS are derivatives of these types, but may not be directly compatible with each other. Look into each database for specifics on the data types supported.
- Text
- Numbers
- Dates with times
- NULL

Overview of the `select` statement
---
The `select` statement is used to get some of the data from a table. It has six clauses and must be written in this order:  

Name | Usage
--- | ---
`select` | Which columns of data to get
`from` | Which table has the data
`where` |Which rows of data to get
`group by` | Which columns to summarize as a group
`having` | Eliminating some of the summarized data
`order by` | Which columns are used to sort the result

Other Keywords
---
Keyword | Usage | Example
--- | --- | ---
`*` | Retrieve all columns | `select` * <br>or<br>`select` table_name.*
`as` | Rename a selected column | `select` column_name `as` new_name
`distinct` | Eliminate duplicate row from the result | `select distinct` column_name, column_name

### Use a `select` clause to get a list of all of the columns
This is the simplest `select` statement that you can write. The `select` clause and the `from` clause are required in any `select` statement. All other clauses are optional.

```sql
select * from table_name;
```

### Using a constant value in the `select` clause
```sql
select employee_id,
    last_name,
    'EXCELLENT WORKER' as evaluation, 
    10 as rating, 
    '01-JAN-2011' as eval_date, 
    null as next_eval 
from l_employees
order by employee_id;
```
Or it might make more sense to create a table of constants to prevent needing to modify your query in the future.

Overview of the `where` clause
---
Use boolean logic to choose which rows of data you want to retrieve. All of these conditions can be used with any of the main types of data — text, numbers, and dates. Each condition has both a positive form and a negative form and the negative form is always the exact opposite of the positive form. 

The `where` clause is processed first before `select`, which removes some rows from the beginning table.

Keyword | Usage | Example
--- | --- | ---
`=` | Equal | with numbers: credit_limit `=` 25.00<br>with text: first_name `=` 'SUE'<br>with dates: hire_date `=` '01-JUN-2010'
`<` | Less than | credit_limit `<` 25.00
`<=` | Less than or equal | first_name `<=` 'M'
`>` | Greather than | hire_date `>` '01-JUN-2010'
`>=` | Greater than or equal | credit_limit `>=` 30.00
`<>` and others| Not equal | `where` manager_id `<>` 203<br>`where not` manager_id `=` 203<br>`where` manager_id `!=` 203<br>`where` manager_id `^=` 203
`in` | In a set | credit_limit `in` (15.00, 25.00)
`between` | In a range | credit_limit `between` 21.00 `and` 27.00
`like` | Matches a pattern | phone_number `like` '%48%'
`is null` | Is a `null` value | `where` column_name `is null`
`and` | Both conditions are true | (credit_limit = 25.00) `and` (first_name = 'SUE')
`or` | One of the conditions is true | (credit_limit = 25.00) `or` (first_name = 'SUE')
`not` | The condition is false | `where` column_name<br>`not in` (item1, item2)<br>`not between` value1 `and` value2<br>`not like` '%48%'<br>`is not null`

### SQL uses three-Valued logic
Either a statement true or it is false. But there could be a third condition. If there can be a `null` value, the condition is unknown. Therefore, `null` values are not included when the `where` clause uses a specific logic condition that excludes them. This is to remind you that a `null` is missing data and it is not like any other value in the table, because it does not have a particular value.

### Wildcard Characters and their meanings
Character | Meaning
--- | ---
`%` (percent sign) | A string of characters of any length, or possibly no characters at all (a zero-length string).
`_` (underscore) | One character.
`\%` or `\_` (backslash) | Wildcard characters must be escaped to use them literally.

### Definition of standard form in the `where` clause
The three Boolean connectors `and`, `or`, and `not` are strictly controlled:
- `Not` is applied only to simple conditions. It is not applied to compound conditions that include an `and` or an `or`.
- `And` is used to combine simple conditions and conditions involving `not`. None of these conditions are allowed to contain an `or`. Many conditions can be combined together with `and`. If there is more than one `and`, the conditions can be combined in any order and no parentheses are required. Each of these compound conditions is usually enclosed in parentheses.
- `Or` is the top-level connector. It combines all the compound conditions using `and` and `not`. If there is more than one `or`, the compound conditions can be combined in any order and no parentheses are required.

These rules for the standard form help to avoid logic errors and allow the database management system to process queries more efficiently. For example, an `or` condition is inclusive and can take advantage of short-circuiting to match the first true condition.

Overview of the `order by` clause
---
If you leave out the `order by` clause, you are saying that you do not care about the order of the result table. Most likely this will be the order the records were entered into the table. 

Option | Meaning
---|---
`asc`| Means ascending order (default)
`desc` | Means descending order.

You may specify a list of column names or a list of numbers that refer to the position of the column in the `select` clause.

```sql
select * from employees order by employee_id;
select * from employees order by last_name, first_name;
select * from employees order by 1 asc, 2 desc;
```

Punctuation
---
Problems stemming from incorrect punctuation will usually generate an error, but those errors are not always specific to the problem, so it is good practice to follow these conventions.

### Spaces ( )
Column names and table names that have spaces in them can cause trouble if they are not enclosed in double quotes. Instead use an underscore or MixedCase.

SQL does not interpret blank lines or tabs any different than a single space, so pretty printing SQL is purely for human readability.

### Commas (,)
Separate lists with commas, but commas do not end a list, so remove them if they are not followed by a value.

### Quotes (', ")
Surround text strings and dates with single quotes, but do not use quotes with numbers. Some database do not distinguish between single quotes and double quotes, single quotes are pretty much the standard. Also, be careful about text editors that convert quotes to curly quotes. A literal single quote may be stored in a database by escaping with two single quotes.

```sql
-- use two single quotes to use literal single quote in a where clause
select * from suppliers where supplier_name like '%''%';
```

### Semicolon (;)
A semicolon marks the end of a statement.

### Reserved Words
Any words that are recognized by SQL can not be used for column or table names. Adding an underscore is a way to avoid using a reserved word so, it would be acceptable to name a column `from_` or `date_`.

### Comments (-- or /* */)
Double dashes can be used to create a comment. The C-style comment can also be used for larger comment blocks. Comments can be helpful when debugging larger queries.

```sql
/*This is a 
    multiline comment*/
select * from table_name; -- comment inline
-- comment on a new line
```

### Periods (.)
Use a period to indicate that a column is a field of a particular table. This is similar to the dot (.) operator in the C language.

### Case-Sensitivity
The original designers of the SQL language believed that non-case-sensitivity is best. While column names and table names may not be case-sensitive, some databases treat data stored within the tables as case-sensitive. 

```sql
-- these two are the same
SELECT * FROM TABLE_NAME;
select * from table_name;
-- but, these may be different
select * from table_name where column_name = 'filter';
select * from table_name where column_name = 'FiLtEr';
```

Functions
---
Term | Usage
--- | ---
count | Get an integer value for the number of records matching a query.
upper `|` lower | Convert the case of data stored in a table.

Create `Tables` and `Views`
---
Tables and views are very similar. They can both be used as a source of data in the `from` clause of a `select` statement.

A `table` stores data directly on the disk. A `view` stores a `select` statement on the disk, but does not store any data. Whenever you use a `view`, SQL runs the `select` statement that defines the `view`.

`Views` can be used to build other `views`, but the computer must be able to find the `base tables` and will not allow circular definitions.

### The `create table` statement
The `create table` statement creates a new table. When it is first created, this table will not have any rows of data in it. Each SQL product supports datatypes that differ slightly from other SQL products.

```sql
create table table_name
(column_name_1 data_type_1,
column_name_2 data_type_2,
...);
```
This is the simplest form of the command. Many other options can be specified in this command or added later. All the columns of the table must be listed.

The `alter table` statement is used to add a primary key to a table after it has been created.

### A `table` can be created from a `select` statement
```sql
create table sec0401_sales_staff as 
select employee_id,
    first_name,
    last_name,
    dept_code
from 1_employees
where dept_code = 'SAL';
```

### Similarly, a `view` can created from a `select` statement
```sql
create view sec0402_sales_staff_view as 
select employee_id,
    first_name,
    last_name,
    dept_code
from 1_employees
where dept_code = 'SAL'
order by employee_id;
```

Modifying the table structure
---
### !

Deleting a table
---
You can delete a table with the SQL command `drop table`, followed by the name of the table. This gets rid of the table entirely. It deletes the data in the table, the table structure, and the definitions of the columns. The name of the table is no longer reserved. 

There is a technique known as preventative delete where a table or view is deleted just prior to issuing a `create` statement.

```sql
drop table sec0405_sales_staff;

drop view sec0402_sales_staff_view;
create view sec0402_sales_staff_view as 
select employee_id,
    first_name,
    last_name,
    dept_code
from 1_employees
where dept_code = 'SAL'
order by employee_id;
```

Adding one new row to a table
---
There are two methods to do this. Both are versions of the `insert` statement, and begin with `insert into` followed by the name of the table. They both have the word `values` followed by a list of values in parentheses. The value put into any column must always match the datatype of that column: text, number, or date.

```sql
-- insert a value into all columns, including nulls
insert into sec0409_foods 
values ('ARR', 'AP', 11, 'APPLE PIE', 1.50, null); 

-- or specify the columns
insert into sec0409_foods
(product_code, description, supplier_id, price) 
values ('BP', 'BLUEBERRY PIE', 'ARR', 1.60); 
```
Some database will truncate text that is too large for the column while others will generate an error.

Changing data in the rows already in a table
---
You can modify the values in one column or several columns with the `update` statement. Usually, the data in only a few columns is modified at a time. If you want to modify the data in all the columns, it might be easier to add a new row to the table and delete the old row.

The value can be a fixed value, a function, an expression, or even a sub-query.

If you want to change the data in a single row, it is best to specify the values of the primary key columns in the `where` clause. All the rows for which the condition is true are modified.

```sql
update table_name
set column_1 = value_1,
          column_2 = value_2
where condition;
```

Deleting rows from a table
---
You can delete one row or several rows using a `delete` statement. All the rows for which the condition is true are deleted.

```sql
delete from table_name
where condition;
```

The `commit` and `rollback` commands
---
When you make a change to the data in a table, at first the change is made in a temporary way. `Commit` makes the change permanent. It is a save command on the SQL level. `Rollback` throws out the changes. It is an undo command on the SQL level. Rollback goes back to the last commit point.

When you first enter your changes, they are private and only you can see them. If other people are using the database table you are changing, they will not see your changes until you `commit` them.

```sql
-- Issue one or more commands to change the data —insert, update, delete.
commit;

-- setting autocommit on will call commit after every modify command.
set autocommit off;
```

Transactions
---
A transaction is used to ensure that the data in the database stays consistent. Sometimes the data in several tables needs to be changed in a coordinated way. By placing all these changes within a transaction, you can be sure that the tables will not become corrupted if some of the changes succeed and others fail. A transaction can only occur when the `autocommit` option is turned `off`. Commands can then be issued in sequence followed by a single `commit`.

All of the updates are successful and they will all go into the database together at the same time with a single `commit` command or All of the changes will be rolled back and none of the changes will be made to the database.

Oracle Data Dictionary
---
Note that user_tables are limited to information about the database objects that you own; all_tables may also include information about database objects that are owned by other people, but only if they have decided to share them with you.

Information to Get | Data Dictionary Table | Data Dictionary Columns
--- | --- | ---
Table names | user_tables<br>or<br>all_tables | table_name
View names | user_views<br>or<br>all_views | view_name
View definition | user_views<br>or<br>all_views | text
Columns of tables and views | user_tab_columns<br>or<br>all_tab_columns | column_name
Primary keys of tables | user_constraints and<br>user_cons_columns<br>or<br>all_constraints and<br>all_cons_columns | constraint_name<br>and<br>constraint_type, position

### How to find the names of the columns in a table or view
Oracle has two different methods to obtain this information. One method uses the `describe` command followed by the name of the table. This is a command that only works in Oracle. The other queries the `user_tab_columns` table.

### Contstraints
The `constraint_type` column contains the following codes:  
P—Primary key  
R—Referential Integrity, foreign key  
C—Check constraint  
U—Uniqueness constraint



References
---
Patrick, J. J. [SQL Fundamentals, Third Edition](https://www.oreilly.com/library/view/sql-fundamentals-third/9780137156146/) 2008.