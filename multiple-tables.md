# Using More Than one Table

The pet table keeps track of which pets you have.If you want to record other information about them,such as events in their lives like visits to the vet or when litters are born,
you need another table.What should this table look like?It needs to contain the following information:

- The pet name so that you know which animal each event pertains to.
- A date so that you know when the event occurred.
- A field to describe the event.
- An event type field,if you want to be able to categorize events.

Given these considerations,the `create table` statement for the `event` table might look like this:

```mysql
create table event (name varchar(20), date DATE, type varchar(15), remark varchar(255));
```

As with the `pet` table,it is easiest to load the initial records by creating a tab-delimited text file containing the folling information.

Load the records like this:

```mysql
load data local infile 'event.txt' into table event;
```
Based on what you have learned from queries that you have run on the `pet` table,you should be able to preform retrievals on the records in the `event` table;the principles
are the same.But when is the `event` table by itself insufficient to answer questions you might ask?

Suppose that you want to find out the ages at which each pet had its litters.We saw earlier how to calculate ages from two dates.The litter date of the monther is in the `event`
table,but to calculate her age on that date you need her birth date,which is stored in the `pet` table.This means the query requires both tables:

```mysql
select pet.name, TIMESTAMPDIFF(YEAR,birth,date) as age, remark
    from pet inner join event 
      on pet.name = event.name
    where event.type = 'litter';
```

There are several things to note about this query:

- The `FROM` clause joins two tables because the query needs to pull information from both of them.
- When combining(joining) information from multiple tables,you need to specify how records in one table can be matched to records in the other.This is easy because they both have
  a `name` column.The query uses an `ON` clause to match up records in the two tables based on the `name` values.

  The query uses an `INNER JOIN` to combine the tables.An `INNER JOIN` permits rows from either table to appear in the result if and only if both tables meet the conditions
  specified in the `ON` clause.In this example,the `ON` clause 
