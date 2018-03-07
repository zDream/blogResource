title: oracle左，右，全连接
date: 1/21/2016 5:58:42 PM 
tags: 数据库
---
## Oracle  外连接 ##

- 左外连接 (左边的表不加限制)
- 右外连接(右边的表不加限制)
- 全外连接(左右两表都不加限制)

外连接(Outer Join)

outer join则会返回每个满足第一个（顶端）输入与第二个（底端）输入的联接的行。它还返回任何在第二个输入中没有匹配行的第一个输入中的行。外连接分为三种： 左外连接，右外连接，全外连接。 对应SQL：LEFT/RIGHT/FULL OUTER JOIN。 通常我们省略outer 这个关键字。 写成：LEFT/RIGHT/FULL JOIN。

在左外连接和右外连接时都会以一张表为基表，该表的内容会全部显示，然后加上两张表匹配的内容。 如果基表的数据在另一张表没有记录。 那么在相关联的结果集行中列显示为空值（NULL）。

对于外连接， 也可以使用“(+) ”来表示。 关于使用（+）的一些注意事项：

---

 - （+）操作符只能出现在where子句中，并且不能与outer join语法同时使用。
 -  当使用（+）操作符执行外连接时，如果在where子句中包含有多个条件，则必须在所有条件中都包含（+）操作符
 -（+）操作符只适用于列，而不能用在表达式上。
 - （+）操作符不能与or和in操作符一起使用。
 - （+）操作符只能用于实现左外连接和右外连接，而不能用于实现完全外连接。


在做实验之前，我们先将dave表和bl里加一些不同的数据。 以方便测试。

	SQL> select * from bl;

        ID NAME

         1 dave

         2 bl

         3 big bird

         4 exc

         9 怀宁

	SQL> select * from dave;

        ID NAME

         8 安庆

         1 dave

         2 bl

         1 bl

         2 dave

         3 dba

         4 sf-express

         5 dmm

### 左外连接（Left outer join/ left join） ###

 left join是以左表的记录为基础的,示例中Dave可以看成左表,BL可以看成右表,它的结果集是Dave表中的数据，在加上Dave表和BL表匹配的数据。换句话说,左表(Dave)的记录将会全部表示出来,而右表(BL)只会显示符合搜索条件的记录。BL表记录不足的地方均为NULL.

	示例：

	SQL> select * from dave a left join bl b on a.id = b.id;

       ID NAME               ID NAME

        1 bl                  1 dave

        1 dave                1 dave

        2 dave                2 bl

        2 bl                  2 bl

        3 dba                 3 big bird

        4 sf-express          4 exc

        5 dmm                             -- 此处B表为null，因为没有匹配到

        8 安庆                             -- 此处B表为null，因为没有匹配到

	SQL> select * from dave a left outer join bl b on a.id = b.id;

        ID NAME               ID NAME

         1 bl                  1 dave

         1 dave                1 dave

         2 dave                2 bl

         2 bl                  2 bl

         3 dba                 3 big bird

         4 sf-express          4 exc

         5 dmm

         8 安庆

用（+）来实现， 这个+号可以这样来理解： + 表示补充，即哪个表有加号，这个表就是匹配表。所以加号写在右表，左表就是全部显示，故是左连接。

	SQL> Select * from dave a,bl b where a.id=b.id(+);    -- 注意： 用（+） 就要用关键字where

        ID NAME               ID NAME

         1 bl                  1 dave

         1 dave                1 dave

         2 dave                2 bl

         2 bl                  2 bl

         3 dba                 3 big bird

         4 sf-express          4 exc

         5 dmm

         8 安庆

### 右外连接（right outer join/ right join） ###

和left join的结果刚好相反,是以右表(BL)为基础的, 显示BL表的所以记录，在加上Dave和BL 匹配的结果。 Dave表不足的地方用NULL填充.

	示例：

	SQL> select * from dave a right join bl b on a.id = b.id;
 
        ID NAME               ID NAME

         1 dave                1 dave

         2 bl                  2 bl

         1 bl                  1 dave

         2 dave                2 bl

         3 dba                 3 big bird

         4 sf-express          4 exc

                               9 怀宁    --此处左表不足用Null 填充

	SQL> select * from dave a right outer join bl b on a.id = b.id;

        ID NAME               ID NAME

         1 dave                1 dave

         2 bl                  2 bl

         1 bl                  1 dave

         2 dave                2 bl

         3 dba                 3 big bird

         4 sf-express          4 exc

                               9 怀宁  --此处左表不足用Null 填充

用（+）来实现， 这个+号可以这样来理解： + 表示补充，即哪个表有加号，这个表就是匹配表。所以加号写在左表，右表就是全部显示，故是右连接。

	SQL> Select * from dave a,bl b where a.id(+)=b.id;

        ID NAME               ID NAME

         1 dave                1 dave

         2 bl                  2 bl

         1 bl                  1 dave

         2 dave                2 bl

         3 dba                 3 big bird

         4 sf-express          4 exc

                               9 怀宁

### 全外连接（full outer join/ full join） ###

左表和右表都不做限制，所有的记录都显示，两表不足的地方用null 填充。 全外连接不支持（+）这种写法。

	示例：

	SQL> select * from dave a full join bl b on a.id = b.id;

        ID NAME               ID NAME

         8 安庆

         1 dave                1 dave

         2 bl                  2 bl

         1 bl                  1 dave

         2 dave                2 bl

         3 dba                 3 big bird

         4 sf-express          4 exc

         5 dmm

                               9 怀宁

    SQL> select * from dave a full outer join bl b on a.id = b.id;

        ID NAME               ID NAME

         8 安庆

         1 dave                1 dave

         2 bl                  2 bl

         1 bl                  1 dave

         2 dave                2 bl

         3 dba                 3 big bird

         4 sf-express          4 exc

         5 dmm      