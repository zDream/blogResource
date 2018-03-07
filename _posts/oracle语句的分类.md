title: oracle语句的分类
date: 1/6/2016 9:09:07 AM 
tags: 数据库
---
## oracle语句主要分以下四类 ##

### DML(Data Mannipulation Language)数据操纵语言：查询、操纵数据表资料行 ###

 - SELECT : 检索数据库表或视图数据 
 - INSERT :  将数据行新增至数据库表或视图中
 - UPDATE : 修改表或视图中现有的数据行
 - DELETE : 删除表或视图中现有的数据行

 注意：DML语句不会自动提交事务！

### DDL(Data Definition Language)数据定义语言：建立、修改、删除数据库中数据表对象 ###

 - CREATE TABLE : 创建表 
 - ALTER TABLE : 修改表
 - DROP TABLE : 删除表

 注意：DLL语句会自动提交事务！所以：DML语句事务提交之前可以回滚，DDL语句不能回滚事务

### DCL(Data Control Language)数据控制语言：用于执行权限授予与收回操作 ###

 - GRANT : 给用户或角色授予权限
 - REVOKE : 收回用户或角色的所有权限

 注：DCL[Data Control language,数据控制语言] 一般包括事务控制语句(TCS)、会话控制语句(SCT)、系统控制语句(SCT)。

### TCL(Transactional Control Language)事物控制语言：维护数据的一致性 ###

 - COMMIT :提交已经进行的数据库改变
 - ROLLBACK : 回滚已经进行的数据改变
 - SAVEPOINT : 设置保存点，用于部分数据改变的取消
          
          
          