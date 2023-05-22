## SQL的相关用途

SQL是一种数据查询语句现在已经成为国际标准

### OLAP-联机分析的数据库

A -> analysis

抖音在用的一个数据库

数据查询速度是MySQL的8倍

### DORIS-大规模并行处理数据库

### Spark-并行计算框架-在内存中操作数据-同样支持SQL操作

### Flink-流式计算处理框架-操纵流数据

### ElasticSearch-分布式的数据搜索引擎

## OR-Mapping

### 目的:

在Java中消灭SQL语句, 实现SQL语句的外置化

### 设计原理

**接口**: **设计规约** -> **规范和实现进行分离**

![image-20230320083846022](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230320083846022.png)

## springDataJPA-History

SpringDataJPA是SpringData(顶级项目)下面的一个项目

其下包括了很多常用的工具, 包括Neo4j(图数据库) MongDB(非关系型数据库) ElasticSearch(分布式数据搜索引擎)等, 所以SpringDataJPA的操作实现是较为完善的

### JDBC

使用**`JDBC Template`**通过在`mvn.xml`中引入两个依赖, 同时在全局配置文件`application.properties`来配置数据库连接信息

通过在Java中写SQL语句, 使得**语句结果向对象绑定**, 与此同时再添加一些参数

### Hibernate

基于**`OR-Mapping`**(*Object-Relational Mapping*对象关系)思想开发的数据库操作框架

- Hibernate
- **MyBatis** - 使用较为广泛, 但相比于Hibernate和SpringDAtaJPA是最不面向对象的一个

都是**基于`JDBC`**的数据库操作框架

### JPA

Java官方为**`OR-Mapping`**操作提供的对象存储**规范**, 大部分都是基于Hibernate

### SpringDataJPA

是`SpringData`项目的组成部分, 为**基于JPA**快速方便的实现**数据库访问**提供支持(包括非关系型数据库)

## SpringDataJPA-Operate

### JPA Entity类

**Entity类**是可以**自动生成**的

JPA的Entity类是普通的Java Class。但是这个Java Class有一些特定的标注对应数据库中的实体属性类

主要用的标注(注解)如下:

- **Entity(一个类最上面有一个即可)**
- **Table**
- **Id(标明是主键)**
- **Column**
- **CreateTimestamp(记录创建时间)**
- **UpdateTimestamp()**

### Repository

对某个表操作的DAO对象

继承泛型JpaRepository接口, 之后不用写任何CRUD即可work

![image-20230320090424206](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230320090424206.png)

## code-related-instruction

任意一个OR-Mapping都有缓存策略，JpaRepository中有一个`flush`操作, 就是从缓存中直接清洗入库

### 命名约定模式

SpringData提供了一种名称规范, 名称就会被解析为Sql语句

[Query Methods](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods)



依赖注入\反转控制 -> spring特性



## Work

从设计数据库 -> 实现接口 -> 数据访问

设计数据库 -> 使用JPA对接数据库然后接入前面的内容

- E-R图设计
- 利用开发环境自动生成数据实体对象 -> recording
- 使用JPA进行数据库操作

代码打包: src / pom.xml / 配置文件

### 实体关系

课程与知识点之间的对应关系 (一对多 / 多对多)

**案例与知识点之间的关系**

案例的形态和案例本身(markdown)有关系

知识点和知识点之间的关系(前置和后置 / 依赖关系)

**标签与标签组**

> 标签为什么有标签组? 
>
> 要对分类本身进行分类, 标签是用于对实体进行分类, 标签组用于对标签进行归类

标签与课程 / 知识点之间的关系