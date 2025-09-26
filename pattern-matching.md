# Pattern Matching

MySQL provides standard SQL pattern matching as well as a form of pattern matching based on extended regular expressions similar to those used by Unix utilities such
vi,grep, and sed.

SQL pattern matching enables you to use _ to match any single character and % to match an arbitrary number of characters(including zero characters).In MySQL,SQL patterns
are case-insensitive by default.Some examples are shown here.Do not use `=` or `<>` when you use SQL patterns.Use the `LIKE` or `NOT LIKE` comparison operators instead.

To find names begining with b:

```mysql
SELECT * FROM pet WHERE name like 'b%';
```

To find names ending with `fy`:

```mysql
SELECT * FROM pet where name like '%fy';
```

To find names containing a `w`:

```mysql
SELECT * FROM pet where name like '%w%';
```

To find names containing exactly five characters,use five instances of the _ pattern character:

```mysql
SELECT * FROM pet where name like '_____';
```

The other type of pattern matching provided by MySQL uses extended regular expressions.When you test for a month for this type of pattern,use the REGEXP_LIKE()
function(or the REGEXP or RLIKE operators,which are synonyms for REGEXP_LIKE()).

The following list describes some characteristics of extended regular expressions:

- . matches any single character.
- A character class [...] matches any character within the brackets.For example,[abc] matches a,b or c.To name a range of characters,use a dash.[a-z] matches any
  letter,whereas [0-9] matches any digit.
- * matches zero or more instances of the thing preceding it.For example,x* matches any number of x characters,[0-9]* matches any number of digits,and .* matches any number
  of anything.
- A regular expression pattern match succeeds if the pattern matches anywhere in the value being tested.(This differs from a LIKE pattern match,which succeed only if
 the pattern matches the entire value.)
- To anchor a pattern so that it must match the beginning or end of the value being tested.use ^ at the beginning or $ at the end of the pattern.

To demonstrate how extended regular expressions work,the LIKE queries shown previously are rewritten here to use REGEXP_LIKE().

To find names beginning with b,use ^ to match the beginning of the name:

```mysql
select * from pet where regexp_like(name,^b);
```

To force a regular expression comparison to be case-sensitive,use a case-sensitive collation,or use the `binary` keyword to make one of the strings a binary string,
or specify the c match-control character.Each of these queries matches only lowercase b at beginning of a name:

```mysql
select * from pet where regexp_like(name, '^b' COLLATE utf8mb4_0900_as_cs);
select * from pet where regexp_like(name, BINARY '^b');
select * from pet where regexp_like(name, '^b', 'c');
```
To find names ending with fy,use $ to match the end of the name:

```mysql
select * from pet where regexp_like(name, 'fy$');
```

To find names containing a w,use this query:

```mysql
select * from pet where regexp_like(name, 'w');
```
Because a regular expression pattern matches if it occurs anywhere in the value,it is not necessary in ther previous query to put a wildcard on either side of the pattern to
get it to match the entire value as would be true with an SQL pattern.

To find names containing exactly five characters,use ^ and $ to match the beginning and the end of the name,and five instances of . in between:

```mysql
select * from pet where regexp_like(name, '^.....$');
```

You could also write the preivous query using the {n}("repeat-n-times") operator:

```mysql
select * from pet where regexp_like(name, '^.{5}$');
```

For more information about the syntax for regular expressions,see [Section 14.8.2, “Regular Expressions”](https://dev.mysql.com/doc/refman/8.4/en/regexp.html).
