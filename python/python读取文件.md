## python读取csv文件

[TOC]



### open()

```python
# 1
# 直接用open打开
with open("randn_data_regression_X.csv") as f:
    for line in f:
        # 每输出一行后还会输出一个空行
        print(line)
```

```python
# 若不想输出空行
with open("randn_data_regression_X.csv") as f:
    data = f.read()
    print(data)
```

```python
file = open("randn_data_regression_X.csv")
# file只是一个testIO类型的变量，直接打印file并不能显示文件的内容
print(file)
# 要对file进行read()操作才能够读取文件的内容
data = file.read()
print(data)
# 在对文件操作完之后要关闭文件
file.close()
```



### 自带csv标准库-csv.reader()

```python
# 2
# 用python自带的标准库读取
import csv
csv_reader = csv.reader(open("randn_data_regression_X.csv"))
for line in csv_reader:
    print(line)
```



### pandas库-pd.read_csv()

```python
# 3
# 用pandas读取
import pandas as pd

# pandas读取的文件是会将第一行作为标题的
# 并会自动为数据添加行号，且数据之间是以空格分隔
data = pd.read_csv("randn_data_regression_X.csv")

print(data)
```

```python
# 在reas_csv()中指定分隔符，输出结果与上条一致
data1 = pd.read_csv("randn_data_regression_X.csv",sep=",")
print(data1)
```



### 注意变量的类型

在查看文件并对文件进行读写操作时, 要时刻注意变量的类型

要不然极其容易出现bug

python查看变量类型

```python
# 使用type函数
# 假设现在想查看file变量的类型
file = open("a.txt")
print(type(file))

--> <class '_io.TextIOWrapper'>
```



### numpy库-loadtxt()

想不被修改的**加载原始数据**，还是要用`np.loadtxt()`

**不要用pandas的read_csv()**

![image-20221026083419984](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221026083419984.png)
