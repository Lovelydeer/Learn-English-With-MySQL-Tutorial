# Selecting All Data

The simplest form of `select` retrieves everything from a table:
`SELECT`最简单的形式就是检索表中的所有内容：

`select * from pet;`

This form of `SELECT` uses `*`,which is shorthand for "select all columns".This is useful if you want to review your entire table,
for example,after you've just loaded it with your initial data set.For example,you may happen to think that the birth date for Bowser doesn't seem quite right.
Consulting your original pedigree papers,you find that the correct birth year should be 1989,not 1979.

这种形式的`SELECT`使用了`*`,它是'选择所有列'的缩写。如果你想查看整个表的数据时，这种方式很有用，例如在刚用初始数据集填充完表之后。
举个例子，你可能会觉得Bowser的出生日期好像不太对。查阅最初的血统证明文件后，你发现正确的出生年份应该是1989，而不是1979。

There are at least two ways to fix this:
至少有两种方法可以解决这个问题：

- Edit the file `pet.txt` to correct the error,then empty the table and reload it using `DELETE` and `LOAD DATA`:

    ```
    DELETE FROM PET;
    LOAD DATA LOCAL INFILE 'pet.txt' INTO TABLE pet;
    ```
However,if you do this,you must also re-enter the record for Puffball.

编辑文件 pet.txt 来纠正错误，然后使用 `DELETE` 和 `LOAD DATA` 清空表并重新加载：
但是，如果你这样做，就必须重新输入 `Puffball` 的记录。

- Fix only the erroneous record with an `UPDATAE` statement:

    ```
    UPDATE pet set birth = '1989-08-31' WHERE name = 'Bowser';
    ```
The `UPDATE` changes only the record in question and does not require you to reload the table.

只用一条`UPDATE`语句来纠正那条出错的记录：
`UPDATE`语句只会修改相关的那条记录，而不需要你重新加载整个表。

There is an exception to the principle that `SELECT *` selects all columns.If a table contains invisible columns,`*` does not include them.
For more information,see `Section 15.1.20.10,"Invisible Columns"`.

在`SLECT *`会选择所有列这一原则中有一个例外：如果表中包含不可见列，`*` 并不会包含这些列。更多信息请参见第`15.1.20.10节《不可见列》`。
