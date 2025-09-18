# Selecting Particular Rows

As shown in the preceding section,it is easy to retrieve an entire table.Just omit the `WHERE` clause from the `SELECT` statement.
But typically you don't wnat to see the entire table,particularly when it becomes large.Instead,you're usually more interested in answering a particular question,
in which case you specify some constraints on the information you want.Let's look at some selection quries in terms of question about your pets that they answer.

如前一节所示，获取整张表很简单--只需在`SELECT`语句中省略`WHERE`子句即可。
然而，在实际应用中，你通常并不需要查看整张表，尤其是在表变得很大的时候。
相反，你更关心的是回答某个特定的问题，这时就需要对所需的信息加上一些约束条件。
接下来，让我们通过一些关于宠物的问题来看看相应的选择查询是如何回答它们的。

You can select only particular rows from your table.For example,if you want to verify the change that you made to Bowser's birth date,select Bowser's record like this:
你可以只从表中选取特定的行。例如，如果你想要验证自己对`Bowser`出生日期所做的修改，可以像下面这样查询`Bowser`的记录：

```mysql
SELECT * FROM pet WHERE name = 'Bowser';
```

The output confirms that the year is correctly recorded as 1989,not 1979.

输出结果确认年份已被正确记录为`1989`,而不是`1979`.

String comparisons normally are case-insensitive,so you can specify the name as 'bowser','Bowser',and so forth.The query result is the same.

字符串比较通常是不分大小写的，所以你可以把名字写成`bowser`、`BOWSER`等形式，查询结果都是相同的。

You can specify conditions on any column,not just `name`.For example,if you want to know which animals were born during or after 1998,test the `birth` column:

你不仅可以对`name`列设置条件，还可以对任意列进行限制。例如，如果你想知道哪些动物是在`1998年或之后`出生的，可以对`birth`列进行测试：

```mysql
SELECT * FROM pet WHERE birth >= '1998-1-1';
```

