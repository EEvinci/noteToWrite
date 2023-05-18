# generic & stream & Lambda

## pure generic

Java是一种**强类型语言**，不像JavaScript是函数式语言，对于类型的要求很低，而泛型是Java为了确保类型安全的过程中出现的一种机制

泛型T本身可以是**任何一种类型**

T是**类型参数**, 表示一种未知的类型

```java
class Box{
    private T data;
    
    public void setData(T data){
        this.data = data;
    }
    
    public T getData(){
        return data;
    }
}
```

要求T继承自Object, 但是这是默认的, 所有类都继承于Object接口

### 泛型最核心的用途就是**代码复用**

将类型作为参数, 实现一种算法

比如对不同的对象的排序算法, 就可以使用一个**泛型方法**来统一的进行方法声明

用**接口**来规范该类**类型的行为特征**

**算法 对象 算法在对象中的行为特性**

```java
// 对于包含T类型的数组进行排序, 并且该类型继承于Comparable 
public static <T extends Comparable<T>> void genericSort(T[] array){
    ...;
      // 只要是两个对象有可比较的属性, 那么就可以进行排序
      // 这就说明了排序算法本身和对象之间的隔离
      // 用接口来规范对 对象 的要求
      // 要求对象是可比较的
      // 即对象实现了Comparable接口
      if(array[i].compareTo(array[i+1] > 0)){
          ...
      }
    ...
 }
```

T本身可以是任何类型, 但是需要**实现Comparable接口**

但是**Comparable本身就是一个泛型**, 所以Comparable也需要一个T来进行表示

### 不同对象的复用

对于排序算法, 只要一个对象有可排序的属性, 即可用该对象的类型对排序算法进行调用

如果一个对象本身本身是没有继承comparable接口的, 但是该对象又有可排序的属性, 是自己定义的

那么只要使得该对象继承comparable接口, 并实现compareTo()方法接口使用排序算法, 因为上述排序算法继承了comparable接口

在compareTo()中的实现就是对该对象可排序的属性的比较

## refection based generic

通过反射来获取泛型信息

每一个类都有一个getClass()方法, 返回这个类的class属性, 表示该类的**元数据**, 是编译器为每个类创建的一个基础信息

获取泛型父类: `getGenericSuperclass()`

```java
IntegerBox interBon  = new IntegerBox();
Type superclass = integerDataHolder.getClass().getGenericSuperClass();
```

## Stream

可用其来替代大量的while for方法

stream是一个对象序列

流中的每一个方法都生成一个新的流

流操作有很强大的集合上的表达能力

### 流式编程的数据元素

- 元素: 就是特定类型的对象
- 数据源: 流的来源. 可以是集合,数组
- ...

### stream常用操作

#### 中间操作符

- map: 转换操作符, A->B
- flatmap: 展开操作, 即将int[] {1,2,3}展开为1,2,3. 类似于一个降维操作
- limit: 个数限定操作, 比如数据流只取前三个, stream.limit(3)
- skip: 略去
- dsitint: 去重
- filter: 过滤, 把不符合条件的元素剔除
- peek: 对每个元素执行操作并返回一个新的stream
- sorted: 对元素进行排序, 前提是必须实现comparable接口

#### 终止操作符

对数据进行收集, 到了这里数据就不会向下继续流动了, 终止操作符只能使用一次

- reduce: 归纳操作, 任何可执行操作都可以成为归纳
- min, max
- collect
- ...

## Lambda

就是一种**箭头函数'->'**

没有实现声明方法, 只将**element -> 执行语句**变成一种参数传入(即匿名函数作为参数进行传入)

可以在任何**函数式接口**上面使用Lambda表达式

Lambda表达式就是一种**具有函数接口定义**的**语法**