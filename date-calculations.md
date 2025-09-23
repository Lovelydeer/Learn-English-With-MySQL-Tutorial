# Date Calculations

[Official Tutorial](https://dev.mysql.com/doc/refman/8.4/en/date-calculations.html)

Mysql provides several functions that you can use to perform calculations on dates,for example,to calculate ages or extract parts of dates.

Mysql 提供了若干函数，可以用来对日期进行计算，例如计算年龄或提取日期的部分信息。

To detdermine how many years old each of your pet is,use TIMESTAMPDIFF() function.Its arguments are the unit in which you want the result expressed,
and the two dates for which to take the difference.The following query shows,for each pet,the birth date,the current date,and the age in years.An alias(age)
is used to make the final output column label more meaningful.

要确定每只宠物的年龄，可以使用`TIMESTAMPDIFF()`函数。
它的参数包括：你希望结果所使用的时间单位，以及需要进行差值计算的两个日期。

下面的查询会显示每只宠物的出生日期，当前日期以及年龄（以年为单位）。
其中使用了别名`age`,使最终输出的列标题更直观：

```
SELECT name,birth,CURDATE(),
    TIMESTAMPDIFF(YEAR,birth,CURDATE()) AS age
    FROM pet;
```

The query works,but the result could be scanned more easily if the rows were presented in some order.This can be done by adding an ORDER BY name clause to
sort the output by name:

查询能够正常工作，但结果如果按一定顺序排列，会更容易查看。可以通过在语句中添加`ORDER BY name`子句来按照名字排序输出结果：

```
SELECT name,birth,CURDATE(),
    TIMESTAMPDIFF(YEAR,birth,CURDATE()) AS age
    FROM pet ORDER BY name;
```

To sort the output by age rather than name,just use a different ORDER BY clause:

要按照年龄而不是名字对结果进行排序、只需使用不同的`ORDER BY`子句：

```
SLECT name, birth, CURDATE(),
    TIMESTAMPDIFF(YEAR, birth, CURDATE()) AS age
    FROM pet order by age;
```

A similar query can be used to determine age at death for animals that have died.You determine which animals these are by checking whether the death value is NULL.
Then,for those with non-NULL values,compute the difference between the death and birth values:

类似的查询可以用来计算已死亡动物的死亡年龄。你可以通过检查`death`值是否为 NULL 来判断哪些动物已经死亡。
对于那些`death`值非 NULL 的记录，计算它们的 `death` 与 `birth` 之间的差值即可：

```
select name, birth, death,
    timestampdiff(YEAR,birth,death) as age
    from pet where death IS NOT NULL ORDER BY age;
```

The query uses death IS NOT NULL rather than death `<>` NULL because  NULL is a specail value that cannot be compared using the usual comparison operators.
This is dicussed later.See Section 5.3.4.6,"Working with NULL Values".

查询语句使用的是`death is not NULL` 而不是`death <> NULL`,因为`NULL`是一个特殊值，不能使用常规比较运算符来比较。这部分将在后面进一步讨论，
参见`第 5.3.4.6 节《使用 NULL 值》`。

What if you want to know which animals have birthdays next month?For this type of calculation,year and day are irrelevant;you simply want to extract the month part of
the `birth` column.Mysql provides several functions for extracting parts of dates,such as YEAR(),MONTH(),and DAYOFMONTH().MONTH() is the appropriate funcion here.
To see how it works,run a simple query that displays the value of both `birth` and `MONTH(birth)`:

如果你想知道哪些动物下个月过生日，该怎么做呢？
对于这种计算，年份和日期并不重要，你只需要提取 birth 列中的月部分。Mysql 提供了若干函数来提取日期的不同部分，例如`YEAR()`、`MONTH` 和 `DAYOFMONTH()`。
在这里，`MONTH()`是合适的函数。
为了了解它的工作方式，可以运行一个简单的查询语句，同时显示 birth 和 MONTH(birth) 的值:

```
SELECT name, birth MONTH(birth) from pet;
```

Finding animals with birthdays in the upcoming month is also simple.Suppose that the current month is April.Then the month value is 4 and you can look for animals born
in May(month 5)like this:

查找下个月过生日的动物也很简单。假设当前月份 4月，那么月份值为 4,你就可以通过查找 5 月（月份值为 5）出生的动物来实现：

```
SELECT name, birth, from pet where MONTH(birth) = 5;
```

There is a small complication if the current month is December.You cannot merely add one to the month number(12) and look for animals born in month 13,
because there is no such month.Instead,you look for animals born in January(month 1).

如果当前月份是十二月，会有一个小问题。你不能简单地在月份数（12）上加1，然后查找出生在第13个月的动物，因为并不存在第13个月。
正确的做法是查找一月份(月份值为 1)出生的动物。

You can write the query so that it works no matter what the current month is,so that you do not have to use the number for a particular month.DATE_ADD() enables you to add 
a time interval to a given date.If you add a month to the value of CURDATE(),then extract the month part with MONTH(),the result produces the month in which to look for birthdays:

你可以这样写查询，使其在任何月份都能正常工作，而无需手动指定具体的月份数字。
DATE_ADD() 函数允许你在给定日期上增加一个时间间隔。如果你在CURDATE()的值上加一个月，再用MONTH()提取月份部分，那么结果就是你要查找生日的目标月份：

```
SELECT name, birth FROM pet
    WHERE MONTH(birth) = MONTH(DATE_ADD(CURDATE(),INTERVAL 1 MONTH));
```

A different way to accomplish the same task is to add 1 to get the next month after the current one after using the modulo function (MOD) to wrap the month value to 0
if it is currently 12:

另一种实现相同任务的方法是：在当前月份值的基础上加1,并使用取模函数(MOD)进行处理，这样如果当前月份是12，就会绕到0，从而得到下一个月份：

```
SELECT name, birth FROM pet
    WHERE MONTH(birth) = MOD(MONTH(CURDATE(), 12)) + 1;
```
MONTH returns a number between 1 and 12.And MOD(something,12) returns a number 0 and 11.So the addition has to be after the MOD(),otherwise we would go 
from November(11) to January(1).

MONTH()返回1 到 12 之间的数字。而`MOD(something, 12)` 返回的是 0 到 11 之间的数字。
因此，必须在 `MOD()` 之后再进行加法运算，否则会出现从 11月(11) 直接跳到 1月(1) 的错误情况。

If a calculation uses invalid dates,the calculation fails and produces warnings:

如果某个计算中使用了无效日期，计算会失败并产生警告：

```
SELECT '2018-10-31' + INTERVAL 1 DAY;

SELECT '2018-10-32' + INTERVAL 1 DAY;

SHOW WARNINGS;
```

