# numpy.core._exceptions._UFuncNoLoopError: ufunc 'multiply' did not contain a loop with signature matching types (dtype('<U1'), dtype('float64')) -> None

## 报错背景

在使用gradio的时候, 有一个接口有三个输入参数, 一个是图像, 另外两个是数字, 在我指定interface中的input=['image','text', gr.Slider(0.1,10)]之后就一直报这个错

![image-20230321040951299](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230321040951299.png)

## 问题分析

一开始我以为是图像没有转化为浮点数, 所以一直在做`img = img.astypr(float32)`和`img = img.astypr(float64)`甚至转化为了`int32`等将数据类型转化为浮点型/整型的操作.

![image-20230321040935693](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230321040935693.png)

## 解决方法

但是后来我发现gradio中输入数据的input格式还有`number`, 我才恍然大悟, 怪不得一直报错说矩阵相乘的时候有**字符串**, 所以**计算出错**, 因为指定的输入格式写成了`text`, 所以在gradio读取`input格式`为`text`类型的数据的时候就转化为了字符串

![image-20230321040914249](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230321040914249.png)



所以在传入参数为数字时, input格式一定要是`number`, 不然计算会出问题