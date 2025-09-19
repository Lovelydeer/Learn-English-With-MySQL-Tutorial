# Selecting Particular Columns

[Official Tutorial](https://dev.mysql.com/doc/refman/8.4/en/selecting-columns.html)

If you do not want to see entire rows from your table,just name the columns in which you are insterested,separated by columns.
For example,if you want to know when your animals were born,select the `name` and `birth` columns:

如果你不想查看表中的整行数据，只需列出你感兴趣的列名，并用逗号分隔即可。
例如，如果你想知道你的宠物是什么时候出生的，可以选择`name`和`birth`这两列。

```
SELECT name, birth FROM pet;
```

To find out who owns pets,use this query:

要查询谁拥有宠物，可以使用如下语句：


```
SELECT owner FROM pet;
```

Notice that the query simply retrieves the owner column from each record,and some of them appear more than once.
To minimize the output,retrieve each unique output record just once by the keyword `DISTINCT`:

请注意，该查询只是从每条记录中取出`owner`列，而其中有些名字会出现多次。
为了减少该输出结果的重复，可以通过添加关键词`DISTINCT`,让每个唯一的记录只显示一次：

```
SELECT DISTINCT owner FROM pet;
```

You can use a `WHERE` clause to combine row selection with column selection.For example,to get birth dates for dogs and cats only,use this query:

你可以在`WHERE`子句中同时进行行选择与列选择。例如，如果你只想获取狗的猫的出生日期，可以使用以下查询：

```
SELECT name, species, birth FROM pet
    WHERE species = 'dog' OR species = 'cat';
```
