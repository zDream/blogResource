title: oracle decode grouping rollup cube统计分组查询
date: 4/12/2016 1:43:49 PM 
tags: 数据库
---

学习所用表为

	create table student(
	nianji varchar(20),
	banji varchar(20),
	math varchar(20),
	english varchar(20)
	);

	插入测试数据
	insert into  student values ('1','1','100','87'); 
	insert into  student values ('1','2','100','76'); 
	insert into  student values ('1','3','48','26'); 
	insert into  student values ('2','1','75','86'); 
	insert into  student values ('2','2','67','87'); 
	insert into  student values ('2','3','96','65'); 
	
	insert into  student values ('1','1','100','100'); 
	insert into  student values ('1','2','100','100'); 
	insert into  student values ('1','3','100','100'); 
	insert into  student values ('2','1','100','100'); 
	insert into  student values ('2','2','100','100'); 
	insert into  student values ('2','3','100','100'); 


### oracle rollup和cube ，grouping sets 函数的使用 ###


**rollup**

#### 向ROLLUP传递一列 ####

	select banji,sum(english) from student group by rollup(banji);
	
	1	373
	2	363
	3	291
		1027

通过用rollup可以自动加一列，最后一列为自动加上的

#### 向ROLLUP传递多列 ####

	select nianji,banji,sum(english) from student group by rollup(nianji,banji);

	1	1	187
	1	2	176
	1	3	126
	1		489
	2	1	186
	2	2	187
	2	3	165
	2		538
			1027

	可以看到，除了在最后有一个求和记录外，每个 jianji,banji 分组也会有一个求和记录。


那么我们现在交换一下ROLLUP中数据列的顺序，看看结果怎样

	select nianji,banji,sum(english) from student group by rollup(banji,nianji);

	1	1	187
	2	1	186
		1	373
	1	2	176
	2	2	187
		2	363
	1	3	126
	2	3	165
		3	291
			1027
	两者效果很显示，不用我多说了吧


rollup 原理如下，也只不过是用了 union all 运算符而已
> 假设有一个表test，有A、B、C、D、E5列。
如果使用group by rollup(A,B,C)，首先会对(A、B、C)进行GROUP BY，然后对(A、B)进行GROUP BY，然后是(A)进行GROUP BY，最后对全表进行GROUP BY操作。roll up的意思是“卷起”，这也可以帮助我们理解group by rollup就是对选择的列从右到左以一次少一列的方式进行grouping直到所有列都去掉后的grouping(也就是全表grouping)，对于n个参数的rollup，有n+1次的grouping。以下2个sql的结果集是一样的：

	Select A,B,C,sum(E) from test group by rollup(A,B,C)
	与
	Select A,B,C,sum(E) from test group by A,B,C
	union all
	Select A,B,null,sum(E) from test group by A,B
	union all
	Select A,null,null,sum(E) from test group by A
	union all
	Select null,null,null,sum(E) from test

和下列代码一样的效果

	select nianji,banji,sum(english) from student group by nianji,banji union all
	select nianji,null,sum(english) from student group by nianji union all
	select null,null,sum(english) from student order by nianji

	1	3	126
	1		489
	1	1	187
	1	2	176
	2	1	186
	2	2	187
	2	3	165
	2		538
			1027

#### 向CUBE传递一列 ####

	select banji,sum(math) from student group by cube(banji) order by banji;

	1	373
	2	323
	3	344
		1040
好像没什么不一样，别急，往下看

#### 向CUBE传递多列 ####

	select nianji,banji,sum(english) from student group by cube(nianji,banji) order by nianji,banji;

	1	1	187
	1	2	176
	1	3	126
	1		489
	2	1	186
	2	2	187
	2	3	165
	2		538
		1	373
		2	363
		3	291
			1027
	大家看出有什么不一样了吗，看最后一列.根据nianji统计分数，根据banji统计分数，


cube原理如下

> cube的意思是立方，对cube的每个参数，都可以理解为取值为参与grouping和不参与grouping两个值的一个维度，然后所有维度取值组合的集合就是grouping的集合，对于n个参数的cube，有2^n次的grouping。如果使用group by cube(A,B,C),，则首先会对(A、B、C)进行GROUP BY，然后依次是(A、B)，(A、C)，(A)，(B、C)，(B)，(C)，最后对全表进行GROUP BY操作，一共是2^3=8次grouping。同rollup一样，也可以用基本的group by加上结果集的union all写出一个与group by cube结果集相同的sql：
	
	Select A,B,C,sum(E) from test group by cube(A,B,C);
	与
	Select A,B,C,sum(E) from test group by A,B,C
	union all
	Select A,B,null,sum(E) from test group by A,B
	union all
	Select A,null,C,sum(E) from test group by A,C
	union all
	Select A,null,null,sum(E) from test group by A
	union all
	Select null,B,C,sum(E) from test group by B,C
	union all
	Select null,B,null,sum(E) from test group by B
	union all
	Select null,null,C,sum(E) from test group by C
	union all
	Select null,null,null,sum(E) from test;

和下列代码产生一样的效果 

	select nianji,banji,sum(english) from student group by nianji,banji union all
	select nianji,null,sum(english) from student group by nianji union all
	select null,banji,sum(english) from student group by banji union all
	select null,null,sum(english) from student ;

	1	1	187
	2	2	187
	2	1	186
	1	2	176
	2	3	165
	1	3	126
	1		489
	2		538
		1	373
		3	291
		2	363
			1027

#### grouping sets ####

> grouping sets就是对参数中的每个参数做grouping，也就是有几个参数做几次grouping,例如使用group by grouping sets(A,B,C)，则对(A),(B),(C)进行group by，如果使用group by grouping sets((A,B),C),则对(A,B),(C)进行group by。甚至grouping by grouping set(A,A)都是语法允许的，也就是对(A)进行2次group by,grouping sets的参数允许重复


#### 注意 ####

 - **机制不同** -- 在rollup和cube的说明中分别给出了用基本group by加结果集union all给出了结果集相同的sql，但这只是为了理解的方便而给出的sql，并不说明rollup和cube与基本group by加结果集union all等价。实际上两者的内部机制是安全不一样的，前者除了写法简洁以外，运行时不需多次扫描表，效率远比后者高。
 - **集合可运算** --3种扩展用法的参数可以是源表中的某一个具体的列，也可以是若干列经过计算而形成的一个新列（比如说A+B，A||B），也可以是这两种列的一个集合（例如（A+B，C）），对于grouping set更是特殊，可以是空集合()，表示对全表进行group by。
 - **group by 与 rollup, cube组合使用** --

### oracle grouping 函数的使用 ###

#### grouping() ####

参数只有一个，而且必须为group by中出现的某一列，表示结果集的一行是否对该列做了grouping。对于对该列做了grouping的行而言，grouping()=0，反之为1；

> GROUPING函数可以接受一列，返回0或者1。如果列值为空，那么GROUPING()返回1；如果列值非空，那么返回0。GROUPING只能在使用ROLLUP或CUBE的查询中使用。当需要在返回空值的地方显示某个值时，GROUPING()就非常有用。

#### grouping_id() ####

参数可以是多个，但必须为group by中出现的列。Grouping_id()的返回值其实就是参数中的每列的grouping()值的二进制向量，例如如果grouping(A)=1，grouping(B)=0，则grouping_id(A,B)的返回值就是二进制的10，转成10进制就是2。

#### group_id() ####

无参数。见上面的说明3），group by对某些列的集合会进行重复的grouping，而实际上绝大多数情况下对结果集中的这些重复行是不需要的，那就必须有办法剔出这些重复grouping的行。当结果集中有n条重复grouping而形成的行时，每行的group_id()分别是0,1,…,n，这样我们在条件中加入一个group_id()<1就可以剔出这些重复grouping的行了。

#### 在ROLLUP中对单列使用GROUPING() ####

	select grouping(banji),banji,sum(english) from student group by rollup(banji);

	0	1	373
	0	2	363
	0	3	291
	1		1027

#### 使用CASE转换GROUPING()的返回值 ####

	select case grouping(banji) when 1 then '总计' else banji end as div ,sum(english) from student group by rollup(banji);
	
	1	373
	2	363
	3	291
	总计	1027



### oracle中的decode 函数的使用 ###

上次是用case来转换值 ，这次来使用decode，

	select decode (grouping(banji),1,'总计',banji), sum(english) from student group by rollup(banji);

	1	373
	2	363
	3	291
	总计	1027

	看到了吧，效果是一样的，用这个函数更好些


	select decode (grouping(banji),1,'总计','0',banji), sum(english) from student group by rollup(banji);

	1	373
	2	363
	3	291
	总计	1027

	我如果这样写效果是一样的

我们继续往下走

	select grouping(nianji),grouping(banji), sum(english) from student group by rollup(banji,nianji);

	0	0	187
	0	0	176
	0	0	126
	0	1	489
	0	0	186
	0	0	187
	0	0	165
	0	1	538
	1	1	1027

	你发现了什么，是不是有什么规律呢


	select decode(grouping(nianji),0,nianji,1,'总计'),decode(grouping(banji),0,banji,1,'总计'), sum(english) from student group by rollup(nianji,banji);

	1	1	187
	1	2	176
	1	3	126
	1	总计	489
	2	1	186
	2	2	187
	2	3	165
	2	总计	538
	总计	总计	1027

	到这你是不是已经知道decode怎么用了，不过这还不是我想要的，

	select  decode(grouping(nianji)+grouping(banji),1,'年级总分',2,'总计',nianji) 年级,
	decode(grouping(banji)+grouping(nianji),1,'班级总计',2,'总计',banji) 班级,sum(math) 数学成绩,sum(english ) 英语成绩
	from student
	group by rollup(nianji,banji) ;

	1	1	198	187
	1	2	156	176
	1	3	148	126
	年级总分	班级总计	502	489
	2	1	175	186
	2	2	167	187
	2	3	196	165
	年级总分	班级总计	538	538
	总计	总计	1040	1027

	好了，到这就结束了，这就是我要的效果，是不是感觉sql很强大，用法就是这么写的，下面来看下语法 

结合上面的sql，你再看语法将会变得非常简单，通俗易懂。

**decode语法**

	decode(条件,值1,返回值1,值2,返回值2,...值n,返回值n,缺省值)
	该函数的含义如下：
	IF 条件=值1 THEN
	　　　　RETURN(翻译值1)
	ELSIF 条件=值2 THEN
	　　　　RETURN(翻译值2)
	　　　　......
	ELSIF 条件=值n THEN
	　　　　RETURN(翻译值n)
	ELSE
	　　　　RETURN(缺省值)
	END IF
	decode(字段或字段的运算，值1，值2，值3）

 这个函数运行的结果是，当字段或字段的运算的值等于值1时，该函数返回值2，否则返回值3
当然值1，值2，值3也可以是表达式，这个函数使得某些sql语句简单了许多，结合上面的例子，是不是变得简单明了。


爱学习，爱编程，爱挑战，爱钻研。

如果你觉的好，对你有帮助就，捐助1分钱吧，以资鼓励
![](http://7xpw00.com1.z0.glb.clouddn.com/1460449579392.jpg)

