title: pd生成sql语句带引号
date: 1/14/2016 12:14:05 AM 
tags: PowerDesigner
---
   
 最近在用Powerdesigner生成oracle数据库sql语句时，发现表和字段名中都带有引号。例如：

	create table "authorISBN"  (
    "authorID"           INTEGER        not null,
    "tit_isbn"           VARCHAR2(20),
    "aut_authorID"       INTEGER,
    "isbn"               VARCHAR2(20),
    constraint PK_AUTHORISBN primary key ("authorID")
	);

  如果这样生成表的话，那么你查询或者插入数据都会显示table or view does not exist(表或视图不存在)，然后让你郁闷的事情来了，这些表中oracle数据库中是存在的(我是建立在scott用户中的)，但是你去删除这些表(drop table 表名，或者drop table "表名")都是无法删除的，经过自己查找资料和研究发现，说明scott用户的权限不够。解决的方法是：你先连接到system用户下，使用命令 grant select any table to scott;(这句命令的意思是，授权给scott用户选择任何的表),这样你在连接到scott用户下，发现可以查询出这张表(select * from "表名") 但是表名上要加引号。删除这张表(drop table "表名") 表名上同样要加引号。
    
  那么为什么用PowerDesigner生成的oracle数据库sql语句的表名和字段名上会出现引号呢？
  **因为,Oracle创建表的一条规则为：**在命名表的时候可以使用大写或小写字母。只要表名或字段名没有用双引号括住，Oracle 对大小写就不敏感。Oracle 支持使用双引号的语法。但是，最好不要直接使用双引号。

   那么怎么让这些引号不出现呢？
  在PowerDesiger中，在**physical data model** 中找到菜单中的**Database**下的**Edit current DBMS**中,选择**Script->Sql->Format**，有一项CaseSensitivityUsingQuote，它的comment为“Determines if the case sensitivity for identifiers is managed using double quotes”，表示是否适用双引号来规定标识符的大小写，可以看到右边的values默认值为“YES”,改为“No”，点击【应用】按钮。
   这样再生成sql语句时，表和字段名上是没有引号了。