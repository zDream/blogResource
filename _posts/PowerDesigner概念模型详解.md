title: PowerDesigner概念模型详解
date: 1/13/2016 1:31:06 PM 
tags: PowerDesigner
---
 ## 概念模型的重要性 ##

 数据库设计的基本步骤：

 ### 需求分析 ###

 从系统需求中寻找一些概念性名词，并甄选，并对这些名词相关属性做了解，这部分是人工的，PD做不了什么

 ### 概念结构设计 ###

 针对甄选的名词进行分心，找出其中的关系（独立的、一对一、一对多、多对多、继承五种关系），并用E-R图描述出来，这是大学课本的做法。在PD中，这个过程可以用CDM（概念模型）来描述，PDM中实体概念模型表示方式比E-R更清晰，更好

 ### 逻辑结构设计 ###

 实际上就是设计表的结构和表之间的主外关系等。这部分在PD中对应的是PDM（物理模型），而PD中的物理模型一般都是直接从概念模型生成的。也就是说，只要你做好概念模型，物理模型就可以自动生成。当然，这种生成结果一般都需要做一些调整和优化。

 ### 物理结构设计 ###

 有了PDM，数据库的物理设计将不费吹灰之力，直接可以从PDM导出各种数据库系统的建库脚本。

 ### 数据库的建立和测试 ###
 
 这个过程也很简单，看看建库脚本的执行就知道了。不合理了重新修改PDM，然后生成sql再来

 ### 数据库运行和维护 ###

 这个一般是DBA的事情了，比如时间长了，数据量大了，在某些列上加上索引，调优等等。

 从中可以看到，一上来就建PDM，是不合理的。实际上要求对概念模型有个透彻理解了才去做PDM，这种理解可以不画图，但至少是心中有图。

 ## 使用PD建立数据库概念模型 ##

 ### 一对一CDM ###

 ### 一对多CDM ###

 ### 多对多CDM ###

 ### 继承关系CDM ###

 ## 总结 ##

 - 数据库建模是系统设计中最重要一步，概念模型能很好的描述数据间的关系，还可以从概念模型精确生成符合一定标准范式的物理模型。
 - CDM能描述出更细微的数据关系，比如是0-n还1-n，这直接影响到数据业务上的约束，但是用PDM无法描述。CDM为业务交流节约了沟通成本。
 - CDM也为后来了解底层业务数据关系提供了依据，尤其是表很多很多时候，如果没有CDM，那只有设计数据库的人知道底层的关系了。
 -  如果表很多，分模块的情况，还可以讲CDM分包来管理，这样可以避免将所有的实体关系画到一张图中所带来阅读上烦恼。
 -  PD还有其他很多很强悍的功能，比如数据库反响到PDM，PDM导出脚本，PDM导出Java模型对象、XML模型。还可以生成DAO层的持久化代码，甚至hbm文件，还可以做业务流程建模、生成数据字典报表等等。但PD最擅长的就是CDM-->PDM-->SQL，数据库反向工程，报表功能，用好这些就不错了。