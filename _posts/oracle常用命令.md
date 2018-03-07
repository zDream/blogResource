title: oracle常用命令
date: 1/13/2016 11:04:13 PM 
tags: 数据库
---
## 解锁 scott用户 ##

	Cmd 命令下输入 sqlplus / as sysdba  进入 特权模式，然后更改 用户权限
	alter user scott account unlock;

	解锁scott用户时设置密码
	alter user SCOTT account unlock identified by 123456;

## 创建用户，授权 ##

创建用户tom 密码为13456

	create user tom identified by 123456

授权tom登录权限 

	grant connect to tom

授权resource建表权限  

	 grant  resource to tom;

授权用户DBA权限

	grant dba to testuser ;

授权创建视图权限

	grant create any view to test;

给一个用户赋权限使用命令grant,回收权限使用命令 revoke


删除用户tom

	drop user tom

删除带数据的tom 


	drop user tom cascade

查询scott 下的表

	select  *  from  tab;

删除表

	select 'drop table '||table_name||';' 
	from cat 
	where table_type='TABLE'

EXP数据库导出命令

	exp 用户名/密码@localhost:1521/orcl file=E:/test.dmp full=y

DMP文件导入命令

	imp 用户名/密码@localhost:1521/orcl file=E:/test.dmp full=y

删除数据为空的表数据
 
	delete from cs_jsjd where cjdw is null; 

## 聚合函数的使用 ##

聚合函数包含如下函数

	avg count dense_rank rank first last max min sum grouping


group by 用于对查询结果分组统计  
having子句用于限制分组显示结果

如何显示每个部门的平均工资和最高工资

	SQL> select deptno,avg(sal),max(sal) from emp group by deptno;

显示每个部门的每种岗位的平均工资和最低工资

	SQL> select deptno,job,avg(sal),max(sal) from emp group by deptno,job;

显示平均工资in低于2000的部门号和它的平均工资 

	SQL> select deptno,avg(sal),max(sal) from emp group by deptno having avg(sal)>2000;

> 对数据分组的总结  
>  - 分组函数只能出现在选择列表、having、order by 子句中  
>  - 如果在select语句中同时包含group by,having,order by 那么它们的顺序是group by , having ,order by

在选择列中如果有列,表达式和分组函数,那么这些列和表达式必须有一个出现在group by 子句中,否则就会出错

	select deptno,avg(sal),max(sal) from emp group by deptno having avg(sal)<2000; 这里deptno就一定要出现在group by中


用decode 加统计行
	
	表结构如下
	CREATE TABLE "USER_CSCS"."CS_JKRB" 
    (	"JKRB_ID" VARCHAR2(32 BYTE) DEFAULT sys_guid(), 
	"KMDM" VARCHAR2(32 BYTE), 
	"KMMC" VARCHAR2(32 BYTE), 
	"BRFSE" VARCHAR2(32 BYTE), 
	"BSZQ" VARCHAR2(32 BYTE), 
	"ZJR" VARCHAR2(32 BYTE), 
	"ZJSJ" VARCHAR2(32 BYTE) DEFAULT to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'), 
	"XGR" VARCHAR2(32 BYTE), 
	"XGSJ" VARCHAR2(32 BYTE)
    )

	with aa as
	(
	select kmdm,kmmc from cs_jkrb
	)
	select decode (grouping(t.kmdm),1,'合计',t.kmdm) kmdm,
	decode (grouping(t.kmdm),1,' ',max(r.kmmc)) kmmc,
	sum(t.brfse) 
	from cs_jkrb t left join aa r on t.kmdm=r.kmdm
	group by rollup(t.kmdm) 
	order by t.kmdm;


	select  decode (grouping(kmdm),1,'合计',kmdm) kmdm,kmmc,sum(brfse) from cs_jkrb group by rollup(kmdm,kmmc);

	select  decode (grouping(kmdm),1,'合计',kmdm) kmdm,sum(brfse) from cs_jkrb group by rollup(kmdm);

	
	with ds as
	(select nsrsbh,rkrq,shuie from cs_drkxx),--地税
	gs as (select nsrsbh,rkrq,shuie from cs_grkxx)--国税
	select ds.nsrsbh,ds.rkrq,ds.shuie,gs.rkrq,gs.shuie from ds,gs
	where ds.nsrsbh=gs.nsrsbh;

## oracle合并查询 ##

有时在实际应用中,为了合并多个select语句的结果可以使用集合操作符号 union,union all,intersect,minus

 - union 该操作符用于取得两个结果集的并集,使用该操作符时,会自动去掉结果集中重复行

	select ename,sal,job from emp where sal>2500 union select ename,sal,job from emp where job='manager'

 - union all 该操作符与union相似，但不会取消重复行，而且不会排序

	select ename ,sal,job from emp where sal>2500 union all select ename,sal,job from where job='manager'

 - intersect 使用该运算符取得两个结果集的交集

	select ename,sal,job from emp where sal>2500 intersect select ename,sal,job form emp where job='manager'

 - minus 使用该操作符用于取得两个结果集的差集,它只会显示存在第一个集合中,而不存在第二个集合中的数据

	select ename,sal,job from emp where sal>2500 minus select ename,sal,job form emp where job='manager'

## 复杂查询，子查询 ##

如何显示高于自己部门的平均工资的员工的信息

	第一步
	1.	先求各个部门的平均工资和部门号
	select deptno ,avg(sal) mysql from emp group by deptno;
	2.	把上面的子查询看作一张子表
	select a2.ename,a2.deptno,a2.sal,a1.mysql from emp a2 ,(select deptno,avg(sal) mysql from emp by deptno) a1 where a1.deptno=a2.deptno and a2.sal>a1.mysal;

查询与smith的部门和岗位完全相同的所有雇员

	select  * from emp where (deptno,job)=(select deptno,job from emp where ename='SMITH');

**在多行子查询中使用all操作符**
如何显示工资比部门30的所有员工的工资高的员工的姓名、工资和部门号

	select ename,job,sal from emp where sal>all(select sal from emp where deptno=30);
	或是
	select ename,job,sal from emp where sal>(select max(sal) from emp where deptno =30)
	后者效率更高

**在多行子查询中使用any操作符**

如何显示工资比部门30的任意一个员工的工资高的员工的姓名、工资和部门号

	select ename,sal,job from emp where sal>any(select sal from emp where deptno=30);
	或者
	select ename,job,sal from emp where sal> (select min(sal) from emp where deptno=30);


## with as用法  ##

Oracle数据库中，使用with语句可以实现子查询，提高语句执行的效率，当查询中多次用到某一部分时，可以用Oracle with语句创建一个公共临时表。因为子查询在内存临时表中，避免了重复解析，所以执行效率会提高不少。临时表在一次查询结束自动清除。

	例子1
	with 
	q1 as (select 3+5 S from dual),
	q2 as (select 3*5 M from dual),
	q3 as (select S,M,S+M,S*M from q2,q1)
	select * from q3;

	例子2
	with tt as(
	select 'aaa' id, '高' value from dual union all
  	select 'bbb' id, '低' value from dual union all
  	select 'aaa' id, '低' value from dual union all
  	select 'aaa' id, '高' value from dual union all
  	select 'bbb' id, '低' value from dual union all
  	select 'bbb' id, '高' value from dual)
	select id, 
	count(decode(value,'高'，1))高，
	count(decode(value,'高'，1))低
	from tt
	group by id

	例子3
	with aa as
		 (select zbbh, zbmc, dept_code,dept_name, bssj, #{bszq} bszqq from cs_zbinfo)
		select a.dept_name,
		       a.zbmc,
		       decode(b.log_date, '', '否', '是') sfbs,
		       a.bszqq || '-' || a.bssj ybssj,
		       b.log_date,
		       case
		         when b.log_date is not null and to_char(b.log_date,'yyyy-mm-dd')>a.bszqq || '-' || a.bssj then
		            '超期完成'
		         when b.log_date is not null and to_char(b.log_date,'yyyy-mm-dd')<a.bszqq || '-' || a.bssj then 
		            '准时完成'
		         when b.log_date is null and to_char(sysdate,'yyyy-mm-dd')<=a.bszqq || '-' || a.bssj then
		             '未到期'
		         when b.log_date is null and to_char(sysdate,'yyyy-mm-dd')>a.bszqq || '-' || a.bssj then
		              '超期未完成'
		       end bszt
		  from aa a, cs_zblog b
		 where a.zbbh = b.zbbh(+)
		   and a.bszqq = b.bszq(+)


 ORACLE 11G listener.ora配置文件

	# listener.ora Network Configuration File: E:\app\Dreamer\product\11.2.0\dbhome_1\network\admin\listener.ora
	# Generated by Oracle configuration tools.
	
	SID_LIST_LISTENER =
	  (SID_LIST =
	    (SID_DESC =
	      (SID_NAME = CLRExtProc)
	      (ORACLE_HOME = E:\app\Dreamer\product\11.2.0\dbhome_1)
	      (PROGRAM = extproc)
	      (ENVS = "EXTPROC_DLLS=ONLY:E:\app\Dreamer\product\11.2.0\dbhome_1\bin\oraclr11.dll")
	    )
	    (SID_DESC =
	      (SID_NAME = ORCL)
	      (ORACLE_HOME = E:\app\Dreamer\product\11.2.0\dbhome_1)
	      (GLOBAL_DBNAME = ORCL)
	      (ENVS = "EXTPROC_DLLS=ONLY:E:\app\Dreamer\product\11.2.0\dbhome_1\bin\oraclr11.dll")
	    )
	  )
	
	LISTENER =
	  (DESCRIPTION_LIST =
	    (DESCRIPTION =
	      (ADDRESS = (PROTOCOL = TCP)(HOST = Dream)(PORT = 1521))
	    )
	  )
	
	ADR_BASE_LISTENER = E:\app\Dreamer
	

## 建表，默认值，注释 ##

	create  table CS_QYYD(
	obj_id VARCHAR2(32) default sys_guid(),
	yhmc   VARCHAR2(300),
	yhdz   VARCHAR2(300),
	tjsjq  VARCHAR2(32),
	tjsjz  VARCHAR2(32),
	bqs    NUMBER(18,2),
	bnlj   NUMBER(18,2),
	bszq   VARCHAR2(32),
	zjr    VARCHAR2(32),
	zjsj   VARCHAR2(32) default to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
	xgr    VARCHAR2(32),
	xgsj   VARCHAR2(32)
	)
	--给表加注释信息
	comment on table CS_QYYD is '缴纳社保信息';

	--给字段加注释信息
	comment on column CS_QYYD.obj_id is 'ID';
	comment on column CS_QYYD.yhmc  is '用户名称';
	comment on column CS_QYYD.yhdz is '	用户地址	';
	comment on column CS_QYYD.tjsjq is '统计时间起';
	comment on column CS_QYYD.tjsjz is '	统计时间止	';
	comment on column CS_QYYD.bqs is '本期数	';
	comment on column CS_QYYD.bnlj is '本年累计数';
	comment on column CS_QYYD.bszqm is '报送所属期';
	comment on column CS_QYYD.zjr  is '增加人';
	comment on column CS_QYYD.zjsj is '增加时间';
	comment on column CS_QYYD.xgr is '修改人';
	comment on column CS_QYYD.xgsj  is '修改时间';


