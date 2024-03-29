# ORMapping思想

##  第一个层次：最原始的jdbc

- 连接数据库
- 编写sql语句
- 执行sql语句
- 遍历sql语句返回的结果
- （核心功能）
- 关闭连接

## 第二个层次：拒绝在代码中写入SQL语句

- 引入`Dao对象` -> 执行各种各样的sql语句 -> 包装为各种各样的对象进行返回. 最常返回的就是Entity对象

- 所有想使用sql语句的地方都封装到Dao的一个对象中

- 使用Entity对象来携带数据库对象中的值

## 第三个层次: 将Dao对象接口化

- 耦合度降低, 不和代码关联, **和规范关联**

- 不想使用具体的Dao对象, 和业务代码有耦合, 想实现解耦
  - 因为oracle和mysql的**语句写法不一致**, 想要更换的时候发现sql语句已经和业务代码进行了耦合
- 通过**工厂方法**创建一个具体的实现, 但是使用时用的是接口
- **可以换不同的数据库**

## 第四个层次: 模式化生成sql语句

- 有**很多sql语句是相同的**,只是操作的实例不同
- 使用接口泛型 -> 几乎所有泛型都能用于所有表的单体实例

- student和teacher的查询一致, 对Dao泛型化, 利用泛型可以通过改变类型然后从Dao对象中进行泛型函数表示
- 写sql语句 -> **反射+配置 -> 通过反射动态装配sql语句**
- 模式化单表的增删改查语句不用再写了, 彻底解决还要去写简单sql语句的苦恼
- JPA中约定大于配置

