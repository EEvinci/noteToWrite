# 数据库设计tips & idea自动生成Entity 

[TOC]

## 数据库设计tips

### sql语句的生成

**概念模型生成物理模型** -> **物理模型生成sql语句** -> **sql语句进行建表** -> 

概念模型对词汇的要求很高，其支持的类型和数据库类型有**对应关系**，但**不是一一对应**的，有其自己的转换规则

是比较麻烦的

#### solution

- **直接设计物理模型** 
- 将数据库设计好之后**反向生成E-R图**

### 数据库中的约束越少越好, 更多的约束在代码中实现

因为数据库更趋向于分布式

### 数据库中主键的设置使用`int型的id`

虽然对于用户没有实际意义, 但是对于在实际开发中会很方便

### 数据库表名使用`下划线全小写`命名法

遵循`全小写, 下划线分段`的命名方法

### 在数据库的设计中不定义外键关联

因为数据关系之间肯定会有外键关系, 但是不在数据库中定义

- 数据库很大, 当涉及到分表 / 分库操作时, 有外键关联很麻烦
- 数据库本身规模较大, 外键关联对数据库集群产生影响
- 案例本身可能不是关系型数据库, 可能会放在非关系型数据库中以便搜索, 会将elasticSearch中的id放到数据库中的id中; 
- 可能案例放在关系型数据库不是一个很好的选择, 如果要转移到非关系型数据库要大量的修改代码, 所以外键的关联用代码来进行实现, 不在数据库中进行约束. 
- 如果是小系统, 会使用外键关联来保持数据的一致性; 如果是大系统不要用数据库来保证, 要用代码来保证.
- 如果在数据库中设计了外键关联, 那么Springdata也提供了多种关于外键关联的实现方式.

## 自动生成Entity的意义

实际的开发中是**基于数据库**搭建整个项目,需要维护的是数据库,而不是Java文件

所以需要根据数据库表自动生成`Entity`文件, 快速的生成数据库,并且使用`JpaRepository`的基本增删查改

## 基于idea的操作步骤

### 在idea右边栏中连接数据库

![029d0beeadfdb00f3e20609465dcbbfa.png](https://img-blog.csdnimg.cn/img_convert/029d0beeadfdb00f3e20609465dcbbfa.png)

### 在File中添加`框架支持`

![34a12dddb6694fed5c8c210cca852b56.png](https://img-blog.csdnimg.cn/img_convert/34a12dddb6694fed5c8c210cca852b56.png)

#### 选择hibernate

![7cba4feac30c6e5ea32c23066f0c9cc5.png](https://img-blog.csdnimg.cn/img_convert/7cba4feac30c6e5ea32c23066f0c9cc5.png)

#### 之后项目中生成以下两个文件

![73a0ca799efded107b71d049133f3e36.png](https://img-blog.csdnimg.cn/img_convert/73a0ca799efded107b71d049133f3e36.png)

### 在view -> Tool Windows -> persistence中选中`生成持久性映射`

![790fa343451c509dde7161d8cdb396c2.png](https://img-blog.csdnimg.cn/img_convert/790fa343451c509dde7161d8cdb396c2.png)

#### 然后选择通过`数据库模式`进行生成

![b502228938cf785f40afca035a69bb05.png](https://img-blog.csdnimg.cn/img_convert/b502228938cf785f40afca035a69bb05.png)

#### 选择包,和文件名前缀,后缀

![199de0dba99f83ab28a4ffaccecd67ad.png](https://img-blog.csdnimg.cn/img_convert/199de0dba99f83ab28a4ffaccecd67ad.png)

### 成功生成

### PS

#### char -> String

在navicat的数据库属性类型是有char类型的, 但是在自动生成Entity的过程中, char会被自动转化为String, 这是需要手动进行修改

## 参考文档

[idea自动生成mysql_Idea自动生成Entity实现过程详解](https://blog.csdn.net/weixin_39643336/article/details/114506642)