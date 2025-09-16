# Retrieving Information from a Table

[Official Tutorial](https://dev.mysql.com/doc/refman/8.4/en/retrieving-data.html)

5.3.4.1 Selecting All Data
5.3.4.2 Selecting Particular Rows 
5.3.4.3 Selecting Particular Columns
5.3.4.4 Sorting Rows
5.3.4.5 Date Calculations
5.3.4.6 Working with NULL Values 
5.3.4.7 Pattern Matching 
5.3.4.8 Counting Rows 
5.3.4.9 Using More Than one Table 

The select statement is used to pull information from a table.The general form of statement is:

"select" 语句用于从表中提取信息。该语句的一般形式如下：

```
SELECT want_to_select
FROM which_table
WHERE conditions_to_satisfy;
```

`want_to_select` indicates what you want to see.This can be a list of columns,or `*` to indicate "all the columns".
`which_table` indicates the table from which you want to retrive data.
The `WHERE` clause is optional.If it is present,`conditions_to_satisfy` specifies one or more conditions that rows must satisfy to qualify for retrieval.

`want_to_select` 表示你想要查看的内容。它可以是一个列的列表，也可以是 *，表示所有列。 
`which_table`  表示你要从中检索数据的表。
`WHERE`子句是可选的。如果存在,`conditions_to_satisfy` 用来指定一个或多个条件，只有满足条件的行才会被检索。 
