title: oracle表空间
date: 3/16/2016 4:26:16 PM 
tags: 数据库
---

## 表空间的分类 ##

**永久表空间**  存放永久性数据，如表，索引等。

**临时表空间**  不能存放永久性对象，用于保存数据库排序，分组时产生的临时数据。

**UNDO表空间**  保存数据修改前的镜象。

## 表空间信息 ##

如何查看数据库有哪些表空间？如何查看表空间对应的数据文件？

查看表空间：

查看表空间可以通过下面几个系统视图查看基本信息

--包含数据库中所有表空间的描述信息

	SELECT * FROM DBA_TABLESPACES

--包含当前用户的表空间的描叙信息

	SELECT * FROM USER_TABLESPACES

--包含从控制文件中获取的表空间名称和编号信息

	SELECT * FROM V$TABLESPACE;

---
查看数据文件

--包含数据文件以及所属的表空间的描述信息

	SELECT * FROM DBA_DATA_FILES

--包含临时数据文件以及所属的表空间的描述信息

	SELECT * FROM DBA_TEMP_FILES

--包含从控制文件中获取的数据文件的基本信息，包括它所属的表空间名称、编号等

	SELECT * FROM V$DATAFILE

--包含所有临时数据文件的基本信息

	SELECT * FROM V$TEMPFILE

--查看用户所属的表空间

	select username,default_tablespace from dba_users  where username='用户名';
	用户名需大写


## 创建空间并创建用户指定该空间 ##

### 第1步：创建临时表空间 ###

	create temporary tablespace tv_temp  
	tempfile 'D:\orcldata\tv\temp.dbf' 
	size 20480M  
	autoextend on  
	next 500M maxsize 30720M
	extent management local; 

### 第2步：创建数据表空间 ###

	create tablespace tv_data  
	nologging
	datafile 'D:\orcldata\tv\data.dbf' 
	size 4096M  
	autoextend on  
	next 200M maxsize 20480m
	extent management local;

### 第3步：创建用户并指定表空间 ###
	
	create user tv identified by tv
	default tablespace tv_data
	temporary tablespace tv_temp;

### 第4步：给用户授予权限 ###

	grant connect,resource,dba to username;

	grant create session,create table,unlimited tablespace to tv;

### 1、查看表空间 ###

	select tv.tablespace_name "TABLESPACE_NAME",totalspace "TOTALSPACE/M",freespace "FREESPACE/M",round((1-freespace/totalspace)*100,2) "USED%"
	from 
	(select tablespace_name,round(sum(bytes)/1024/1024) totalspace from    dba_data_files group by tablespace_name) tv,
	         (select tablespace_name,round(sum(bytes)/1024/1024) freespace from dba_free_space group by tablespace_name) fs
	where tv.tablespace_name=fs.tablespace_name;
	
	TABLESPACE_NAME                TOTALSPACE/M FREESPACE/M      USED%
	------------------------------ ------------ ----------- ----------
	KB                                          1024        1023                          .1
	SYSAUX                                    320             3                      99.06
	SYSTEM                                    450             9                           98
	TV                                           4096         128                     96.88
	UNDOTBS1                               5760        5670                      1.56
	USERS                                         5              5                            0
	
	6 rows selected.


### 2、查看临时表空间 ###

	SQL> select tablespace_name from dba_tablespaces;
	
	TABLESPACE_NAME
	------------------------------
	SYSTEM
	UNDOTBS1
	SYSAUX
	TEMP
	USERS
	TV
	KB
	
	7 rows selected.

	
	SQL> select tablespace_name,file_name,bytes/1024/1024 file_size,autoextensible from dba_temp_files;
	
	TABLESPACE_NAME      FILE_NAME                             FILE_SIZE      AUT
	---------------------------------------------------------------------------
	TEMP                           E:\ORCLDATA\TV\TEMP01.DBF    1834          YES

### 3、增加表空间大小 ###

	alter tablespace tv add datafile 'E:\orcldata\tv\data1.dbf' size 4096M;

### 4、增加临时表空间大小 ###

	SQL> alter database tempfile 'E:\orcldata\tv\temp01.dbf' resize 8192M;
	
	Database altered.