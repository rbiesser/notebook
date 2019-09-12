Oracle
---

https://oracle.com

[SQL](../../languages/SQL.md)

Data in Oracle is case sensitive, however column names, table names and reserved words are not.

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