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
This is special treatment of NULL is why,in the previous section,it was necessary to determine which animals are no longer alive using death IS NOT NULL instead of death <> NULL.
