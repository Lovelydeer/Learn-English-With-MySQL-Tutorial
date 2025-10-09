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
The name of the column in the output produced by this statement is always 
