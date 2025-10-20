# Getting Information About Databases and Tables

What if you forget the name of a database or table,or what the structure of a given table is(for example,what its columns are called)?MySQL addresses this problem through 
serveral statements that provide information about the databases and table it supports.

You have previously seen SHOW DATABASES,which lists the database managed by the server.To find out which databse is selected,use the `DATABASE()` function:

```mysql
select database();
```
If you have not yet selected any database,the result is null.
To find out what tables the default database contains(for example,when you are not sure about the name of a table),use this statement:

```mysql
show tables;
```
The name of the column in the output produced by this statement is always Tables_in_db_name,where db_name is the name of database.See section 15.7.7.39"SHOW TABLES Statement",
for more information.

If you want to find out about the structure of a table,the DESCRIBE statement is useful;it displays information about each of a table's columns:

```mysql
DESCRIBE pet;
```

Field indicates the column name,Type is the data type of the column,NULL indicates whether the column can contain NULL values,Key indicates whether the column is indexed,and 
Default specifies the column's dafault value.Extra dispalys special information about columns: If a column was created with AUTO_INCREMENT option,the value is auto_increment 
rather than empty.

DESC is a short form of DESCRIBE.See Section 15.8.1,"DESCRIBE Statement",for more information.

You can obtain the create table statement necessary to create an existing table using the show create table statement.See section 15.7.7.11,"SHOW CREATE TABLE STATEMENT".

If you have indexs on a table,SHOW INDEX FROM tbl_name produces information about them.See section 15.7.7.23,"SHOW INDEX Statement",for more about this statement.
