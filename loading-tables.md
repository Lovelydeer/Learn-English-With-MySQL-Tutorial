# 5.3.3 Loading Data into a table

[Official Tutorial](https://dev.mysql.com/doc/refman/8.4/en/loading-tables.html)

After creating your table, you need to populate it. Tha load Data and insert statements are useful for this.
Suppose that your pet records can be descriped as shown here.
(Observe that Mysql expects dates in 'YYYY-MM-DD' format; this may differ from what you are used to.)

在创建表之后，需要向其中填充数据。可以使用 `LOAD DATA` 和 `INSERT` 语句来实现这一点。
假设你的宠物记录可以按如下所示的方式描述。（请注意，Mysql 期望的日期格式为`YYYY-MM-DD`,这可能与您的习惯格式不同。）


Because you are beginning with an empty table, an easy way to populate it is to create a text file 
containing a row for each of your animals,then load the contents of file into the table with single statement.

由于你是从一个空表开始，一个简便的填充方法是创建一个文本文件，其中每一行对应一只动物的记录，
然后通过一条语句将该文件的内容加载到表中。

You could create a text file pet.txt containing one record per line,with values separated by tabs,and given in the order in which the columns 
were listed in the create table statement. 
For missing values(such as unknown sexs or death dates for animals that are still living),you can use `NULL` values.To represent these in your text file,
use `\N`(backslash, capital-N).For example,the record for Whistlerthe bird would look like this(where the whitespace between values is a single tab character):

```
Whistler    Gwen    bird    \N  1997-12-09  \N
```

你可以创建一个名为pet.txt 的文本文件，每行包含一条记录，值之间用制表符分隔，并且顺序与`CREATE TABLE`语句中列的顺序一致。
对于缺失的值(例如 未知的性别，或仍然存活的动物的死亡日期)，可以使用`NULL`值。
在文本文件中，使用`\N`（反斜杠加大写字母N）来表示。例如，鸟`whistler` 的记录应如下所示（其中各个值之间的空白为一个制表符）。

To load the text file pet.txt into the table pet table,use this statement:
要将文本文件pet.txt 加载到pet表中，请使用如下语句：

```
LOAD DATA LOCAL FILE '/path/pet.txt' INTO TABLE pet.
```

If you created the file on windows with an editor that uses \r\n as a line terminator,you should use this statement instead:
如果你在Windows上用一个以\r\n 作为行终止符的编辑器创建了该文件，那么应该用以下语句：

```
LOAD DATA LOCAL FILE '/path/pet.txt' INTO TABLE pet
LINES TERMINATED BY '\r\n';
```

(On an Apple machine running macOS,you would likely want to use LINES TERMINATED BY '\r'.)
在运行macOS 的apple 电脑上，你可能需要使用···

You can specify the column value separator and end of line marker explicitly in the `LOAD DATA` statement if you wish,but the defaults are tab and linefeeed.
These are sufficient for the statement to read the file `pet.txt` properly.

如果需要，你可以在`LOAD DATA`语句中显示指定列值分隔符和行结束标记，但默认值分别是制表符和换行符。
这些默认设置足以让语句正确读取`pet.txt`文件。

If the statement fails,it is likely that your MySQL installation does not have local file capability enabled by default.See Section 8.1.6,
"Security Considerations for LOAD DATA LOCAL",for information on how to change this.

如果语句执行失败，很可能是因为你的MySQL 安装默认没有启用本地文件功能。有关如何更改此设置的信息，请参见第 8.1.6 节《LOAD DATA LOCAL 的安全注意事项》。

When you want to add new records on at a time,the `INSERT` statement is useful.In its simplest form,you supply values for each column,
in the order in which the columns were listed int the `CREATE TABLE` statement.Suppose that Diane gets a new hamster named "Puffball".
You could add a new record using an `INSERT` statemnet like this:
```
INSERT INTO pet
    VALUES('Puffball','Diane','hamster','f','1999-03-30',NULL);
```

当你想要一次添加一条新记录时，`INSERT` 语句就很有用。在其最简单的形式中，你需要为每一列提供一个值，
顺序与 `CREATE TABLE` 语句中列的顺序一致。假设Diane得到了一只名叫 'Puffball' 的新仓鼠。
你可以使用如下`INSERT`语句添加一条新记录:

String and date values are specified as quoted strings here.Also,with `INSERT`,you can insert `NULL` directly to represent a missing value.
You do not use \N like you do with `LOAD DATA`.

这里字符串和日期值都要写成带引号的字符串。另外，在使用 `INSERT` 时，可以直接插入 `NULL` 来表示缺失值，不像`LOAD DATA`那样需要使用`\N`。

From this example,you should be able to see that there would be a lot more typing involved to load your records initially 
using serveral `INSERT`statements rather than a single `LOAD DATA`statement.

从这个例子中，你应该能看出，如果一开始用多条`INSERT`语句来加载记录，会比用一条`LOAD DATA`语句需要输入更多内容。
