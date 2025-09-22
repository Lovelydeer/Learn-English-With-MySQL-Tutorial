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

What if you want to know which animals have birthdays next month?For this type of calculation,year and day are irrelevant;you simply want to extract the month part of
the `birth` column.Mysql provides several functions for extracting parts of dates,such as YEAR(),MONTH(),and DAYOFMONTH().MONTH() is the appropriate funcion here.
To see how it works,run a simple query that displays the value of both `birth` and `MONTH(birth)`:

```
SELECT name, birth MONTH(birth) from pet;
```

Finding animals with birthdays in the upcoming month is also simple.Suppose that the current month is April.Then the month value is 4 and you can look for animals born
in May(month 5)like this:

```
SELECT name, birth, from pet where MONTH(birth) = 5;
```


