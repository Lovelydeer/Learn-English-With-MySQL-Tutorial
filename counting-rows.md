# Counting Rows

Database are ofen used to answer the question,"How ofen dose a certain type of data occur in a table?" For example,you may want to know how many pets you have,
or how many pets each owner has,or you might want to perform various kinds of census operations on your animals.

Counting the total number of animals you have is the same question as "How many rows are in the pet table?" because there is one record per pet.COUNT(*) counts the number of
rows,so the query to count your animals looks like this:

```mysql
select count(*) from pet;
```

Earlier,you retrieved the names of the people who owned pets.You can use count() if you want to find out how many pets each owner has:

```mysql
select owner, COUNT(*) from pet group by owner;
```
The preceding query uses `group by` to group all records for each owner.The use of `COUNT()` in conjunction with group by is useful for characterizing your data under various
groupings.The following examples show different ways to perform animal census operations.

Number of animals per species:

```mysql
select species, count(*) from pet group by species;
```

Number of animals per sex:

```mysql
select sex, count(*) from pet group by sex;
```
(In this output,NULL indicates that sex is unknown.)

Number of animals per combination of species and sex:

```mysql
select species, sex, count(*) from pet group by species, sex;
```
You need not retrieve table when you use `count()`.For example,the previous query,when performed just on dogs and cats,looks like this:

```mysql
select species, sex, count(*) from pet
    where species = 'dog' or species = 'cat'
    group by species, sex;
```

Or, if you wanted the number of animals per sex only for animals whose sex is known:

```mysql
select species, sex, count(*) from pet
    where sex is not null
    group by species, sex;
```
If you name columns to select in addition to the count() value,a group by clause should be present that names those same columns.Otherwise, the following occurs:

- if the `ONLY_FULL_GROUP_BY` SQL mode is enabled,an error occurs:
  ```mysql
  set sql_mode = 'ONLY_FULL_GROUP_BY';

  select owner, count(*) from pet;
  ```
- if `ONLY_FULL_GROUP_BY` is not enabled,the query is processed by treating all rowsas a single group,but the value selected for each named columns is nondeterministic.
  The server is free to select the value from any row:
  ```mysql
  set sql_mode = '';

  select owner, count(*) from pet;
  ```
See also [Section 14.19.3,"MySQL Handling of GROUP BY"](https://dev.mysql.com/doc/refman/8.4/en/group-by-handling.html). 
See [Section 14.19.1,"Aggregate Function Descriptions"](https://dev.mysql.com/doc/refman/8.4/en/aggregate-functions.html) for information about count(expr) behaviorand related optimizations. 
