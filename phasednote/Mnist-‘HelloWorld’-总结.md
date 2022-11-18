# Mnist-‘HelloWorld’-总结

## 数据集的导入方式集成功能问题

在导入数据集前, 一定要先查看数据集, 对数据集中的**特征名, 标签名, 训练数据的数量, 测试数据的数量. 数据的维度等相关信息**都有所了解, 然后再进行数据的**读取和预处理**

### mnist数据标准化运算区别

valid_x = valid_x/255.0 和 valid_x /= 255.0是有很大的区别的

valid_x表示验证集中的特征集合

valid_x /= 255.0在执行过程中会报错

https://stackoverflow.com/questions/63645132/normalizing-difference-between-x-train-255-0-and-x-train-x-train-255-0

### 不同的数据导入和标准化过程

#### 第一种

```py
```



## 

