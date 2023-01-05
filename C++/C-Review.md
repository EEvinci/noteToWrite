### 字符错题(简直离谱)

![image-20220929093813070](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20220929093813070.png)

![image-20220929093831682](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20220929093831682.png)

![image-20220929093848341](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20220929093848341.png)



### switch语句

基本流程：

```c
switch(常量表达式){
        case 常量1 : statement; break;
        case 常量2 : statement; break;
        default : statement; break;
}
```

- 常量**表达式**: **一般填入单个变量**, 但**也可以填入多个变量**, 前提是多个变量组成的表达式同样可被计算出具体值

![image-20220929144903302](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20220929144903302.png)

- case中的常量: 一般填入单个常量, 且**case中的常量值不能相等**, 但也可以是多个常量组合而成的计算式

![image-20220929145134542](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20220929145134542.png)

​	但是一定不能含有变量的计算，即使该变量在前面已经赋值了

![image-20221013135251406](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221013135251406.png)

![image-20221013135305935](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221013135305935.png)

```txt
the value of 'n' is not usable in a constant expression
即变量n不能再常量表达式中出现
```

- switch语句中可以没有break语句, 也可以没有default语句



### if-else嵌套中的两两对应关系

![image-20221013135810796](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221013135810796.png)

在这里会不会认为else对应的是第一个if, 因为c语言中规定一条当if下只有一条语句的时候可以不加大括号, 而此时第一个if下正好是一条复合if语句, 在逻辑上也认为是一条语句

所以认为想写一个else直接与第一个if对应

但实际上else是与第二个if对应

现在格式化语句:

![image-20221013140101065](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221013140101065.png)

正确的格式应该是这样的

可以看到else是与第二个if对应的

因为C语言中规定:在嵌套使用if语句时，C语言规定**: else总是和之前与其最近的且不带else的if配对**

此时第二个if是与else相距最近的if, 且第二个if也没有与之想配对的else, 所以如果在不加大括号明确语句范围的情况下, 那么题中的else一定是与第二个if相匹配的

所以如果想让else与第一个if对应应该写为如下形式:

![image-20221013140251726](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221013140251726.png)

此时大括号为第一个if明确了作用范围, 所以else只能与第一个if对应

### 为什么要用malloc函数

```c
函数原型: void *malloc(unsigned int num_bytes);　　//分配长度为num_bytes字节的内存块malloc()
```

返回值为void指针, void指针表示没有类型的指针, void *可以指向任何类型的数据, 更明确的说是在用户申请这段空间还不知道用来存储什么类型的数据时, 可以用强制类型转化将void *转化为其他任意类型的指针.

如果分配成功, 则返回已经分配了内存空间的指针(但这时存储空间中的初始值不确定)

如果分配不成功, 则返回空指针NULL

malloc函数是动态内存分配函数, 用来向系统请求分配内存空间. 

当不知道内存的具体位置时, 想要绑定内存空间, 就要使用malloc函数

因为malloc函数只管分配内存空间, 并不能对分配的内存空间进行初始化, 所以一般使用memset函数对分配过的空间进行置0的初始化操作

[为什么要使用malloc函数](https://www.cnblogs.com/ysys/p/6994091.html)



