title: mongodb
date: 3/7/2016 3:24:44 PM 
tags: 数据库
---

## NoSql简介 ##

> 是非关系型的数据库  
> 关系型数据库中的表都是存储一些结构化的数据，每条记录的字段的组成都一样，即使不是每条记录都需要所有的字段，但数据库会为每条数据分配所有的字段。而非关系型数据库以键值对(key-value)存储，它的结构不固定，每一条记录可以有不一样的键，每条记录可以根据需要增加一些自己的键值对，这样就不会局限于固定的结构，可以减少一些时间和空间的开销。 


## 常见的非关系型数据库 ##

 - Redis 
 - MongoDB
 - CouchDB 
 - -

## NoSql优缺点 ##

优点

 - 简单的扩展
 - 快速的读写
 - 低廉的成本
 - 灵活的数据模型

缺点

 - 不提供对SQL的支持
 - 支持的特性不够丰富
 - 现有的产品不够成熟

## MongoDB简介 ##

> MongoDB是用C++语言编写的非关系型数据库。特点是高性能、易部署、易使用，存储数据十分方便，
> 主要特性有：
> 面向集合存储，易于存储对象类型的数据
> 模式自由
> 支持动态查询
> 支持完全索引，包含内部对象
> 支持复制和故障恢复
> 使用高效的二进制数据存储，包括大型对象
> 文件存储格式为BSON(一种JSON的扩展)

