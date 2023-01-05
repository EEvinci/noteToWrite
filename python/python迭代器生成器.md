# 迭代器

迭代器是一种实现了迭代器协议的对象

在C++中，迭代器是一种行为和指针类似的对象

## 有三个基本概念：

可迭代对象：一般为数据容器，且保留迭代接口`__iter__`, 该方法返回一个迭代器

### 迭代器: 

是一个类, 实现了`__iter__`和`__next__`接口的类, 其中`__iter__`返回对象自身(self), `__next__`方法返回迭代的数据(逐个遍历进行输出), 如果没有数据则需要抛出StopIteration异常, 以终止迭代 , 

迭代器对象是一个可以记住遍历位置的对象, 在使用`__next__`函数进行元素访问时, 从集合的第一个对象进行访问, 知道所有的元素都被访问完, 迭代器只能向前访问不能后退

```python
class IT(Object):
    def __int__(self):
        self.counter = 0
        
    def __iter__(self):
        return self
    
    def __next__(self):
        self.counter += 1
        if self.counter == 3:
            raise StopAsyncIteration()
        return self.counter

obj1 = IT()

# 直接调用IT类中的函数
v1 = obj1.__next__() #此时counter==1
v2 = obj1.__next__() #此时counter==2
v3 = ojb1.__next__() # 此时counter==3, 根据__next__中的语句, 已经没有输出, 所以此时要进行StopIteration的异常提示

# 还可以直接使用python的内置函数, 将obj1传入next()函数
v1 = next(obj1) # next()函数自动执行迭代器对象obj1中的__next__()函数进行输出

# 迭代器可以执行for循环
for item in obj1:
    print(item)
# 执行过程就是: 首先会执行迭代器对象的__iter__方法并获取返回值, 然后反复执行__next__方法,每执行一次就把返回值赋给item


# 在 Python 中，使用了 yield 的函数被称为生成器（generator）。
# 跟普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。
# yield关键字很像return，所不同的是，它返回的是一个生成器。
# 在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值,
# 并在下一次执行 next() 方法时从当前位置继续运行。
# 调用一个生成器函数，返回的是一个迭代器对象。

#创建一个生成器
def func():
    yield 1
    yield 2

obj1=func()
v1=next(obj1)
v2=next(obj1)
v3=next(obj1)#抛异常

#可迭代对象
# 如果一个类中有__iter__方法且返回一个迭代器对象，则我们称以这个类创建的对象为可迭代对象。
class Foo(object):
    def __iter__(self):
        return IT() #IT()为迭代器对象/生成器对象

obj = Foo() #obj是可迭代对象
    
# 可迭代对象是可以使用for来进行循环，在循环内部是先执行__iter__方法，获取其迭代对象，
# 然后再在其内部执行这个迭代器对象的next功能，逐步取值
# dir(obj) 获取obj有哪些成员

for item in obj:
    pass
# 基于可迭代对象&迭代器实现：自定义range
class IterRange(object):
    def __init__(self,num):
        self.num = num
        self.counter = -1

    def __iter__(self):
        return self

    def __next__(self):
        self.counter += 1
        if self.counter == self.num:
            raise StopIteration()
        return self.counter
    
# class Xrange(object):
#     def __init__(self,max_num):
#         self.max_num = max_num
#
#     def __iter__(self):
#         return IterRange(self.max_num)


#基于可迭代对象&生成器 实现自定义range
class Xrange(object):
    def __init__(self,max_num):
        self.max_num = max_num
    
    def __iter__(self):
        counter = 0
        while counter < self.max_num:
            yield counter
            counter += 1

#测试代码
obj = Xrange(100)
for item in obj:
    print(item)
    
#常见的数据类型列表，字典，元祖创建的对象都是可迭代对象
#v1是一个可迭代对象，因为在列表中声明了一个__iter__方法并且返回一个迭代器对象
from  collections.abc import Iterator,Iterable
v1 = list(【11,22,33,44】)
print(isinstance(v1,Iterable))#true 判断是否可迭代：是否存在__iter__且反馈迭代器对象
print(isinstance(v1,Iterator))#false 判断是否是迭代器；判断依据是__iter__和__next__

v2 = v1.__iter__()
print(isinstance(v2,Iterable))#T
print(isinstance(v2,Iterator))#T
```



### 生成器：

是一种特殊的迭代器，本质上还是迭代器, 以函数的方式出现, 通过`yield`关键字遍历元素, 每次调用函数类似于对迭代器执行`__next__`方法