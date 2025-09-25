# Working with NULL Values

[Official Tutorial](https://dev.mysql.com/doc/refman/8.4/en/working-with-null.html)

The NULL value can be surprising until you get used to it.Conceptually,NULL means "a missing unknown value" and it is treated somewhat differently 
from other values.To test for NULL,use the IS NULL and IS NOT NULL operators,as shown here:

```mysql
SELECT 1 IS NULL, 1 IS NOT NULL;
```
You cannot use arithmetic compaison operators such as =,<,or<> to test for NULL.To demonstrate this for yourself,try the following query:

```mysql
SELECT 1 = NULL, 1 <> NULL, 1 < NULL, 1 > NULL;
```

Because the result of any arithmetic comparison with NULL is also NULL,you cannot obtain any meaningful results from such comparisons.
In MySQL,0 or NULL means false and anything else means true.The defalut truth value from a boolean operation is 1.
This is special treatment of NULL is why,in the previous section,it was necessary to determine which animals are no longer alive using death IS NOT NULL instead of death `<>` NULL.

Two NULL values are regarded as equal in a group BY.

When doing an order by,NULL values are presented first if you do order by ... ASC and last if you do order by ... DESC.

A common error when working with NULL is to assume that it is not possible insert a zero or empty string into a column defined as not NULL,but this is not the case.
These are in fact values,whereas NULL means "not having a value".You can test this easily enough by using IS [NOT] NULL as shown:

```
SELECT 0 is NULL, 0 is NOT NULL, '' is NULL, '' is NOT NULL;
```
Thus it is entirely possible to insert a zero or empty string into a not NULL column,as these are in fact NOT NULL.See section B.3.4.3,"Problems with NULL values".
