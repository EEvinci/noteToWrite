# Ioc

[TOC]

## 目的

Ioc的目的就是**降低对象之间的耦合度**

通过接口来规范不同对象之间的使用关系，对象不关心具体使用的接口，使用Ioc来分配方法

### 两种最典型的装配方法-实现自动注入

- 注解，使用@Autowired进行自动注入
- 用构造函数的参数进行注入

## 进一步降低耦合度

### Ioc 和 DI（Dependency Injection）

依赖注入是一种实现Ioc的方法

Ioc是一种设计思想，DI是一种落实的手段：

- **Ioc是一种思想：对对象生命周期进行反转控制来解耦的设计思想**
- **DI落实方法：把依赖的对象自动注入到需要的对象**



**spring既是一种Ioc的框架也是一种DI的框架**

从设计理念层次上Ioc和DI有差异：DI是在面向对象上对Ioc的一种实现方法

## 专注于解耦的原因

主要基础原则之一：**高内聚低耦合**

### 为什么一定要做低耦合？

#### 依赖关系

- 自底向上：车轮子 -> 地盘 -> 车身 -> 汽车
- 从顶向下: 

软件本身从功能数量上是比较庞大的, 成百上千很常见, 较大型的软件功能数量成千上万也很常见

### 低耦合设计要求的指导原则

做一个软件设计的核心是**把握软件的复杂度**, 这样才能保证软件不会失控, 所以软件的**最高原则就是解耦**, 要推行**自顶向下**的设计原则, 这要求软件制定出**规格**, 即先制作接口, 然后编写类, 再通过Ioc容器进行**动态的装配**, 如此对软件的管控力就比较强大

### 解耦的目的

提高软件的可扩展性, 可维护性, 可重用性

## 软件设计原则SOLID

### SRP(single responsibility principle)单一职责原则

- **高内聚**
- 在编程时不要在一个controller类中写上很多的不相关的功能
- 能够用一个名字清晰的界定出来 -> **功能单一的一个类**

### OCP(open / closed principle)开闭原则

- **上线的软件接口不要更改**, 不要进行参数的扩展, 而是**创造一个新的接口**, 因为接口使用的复杂程度较高
- 软件的可扩展性本身: **对扩展开放, 对修改关闭**
- **要避免对现有代码的大规模修改**

### LSP(liskov substitution principle)里氏替换原则

- 子类能够替换其**基类(父类)**并保持系统的行为不变
- 面向对象设计方法的**多态思想**: 一个类可以引用其子类的方法, 即子类重写了基类的方法或实现了基类的接口

### ISP(interface segregation principle)接口隔离原则

- 把庞大臃肿的接口进行拆分, 成为更小的更具体的小接口

### DIP(dependency inversion principle)依赖倒置原则

- 高层模块不要依赖于底层模块, 应该都依赖于抽象
- 核心是依赖注入, 即汽车换轮子不影响汽车的开动

## springBean

spring管理的对象就称之为SpringBean, 即Ioc管理的这些对象

## 解耦方法实践

一定要清晰的知道自己在做什么

编程水平的提高:

- 常见的编程使用的熟练
- 提升对编程思想的理解

### 工厂+接口

- 工厂本身还是在代码里包裹了依赖关系, 只不过A对象使用B对象的过程用工厂接手过去

### 反射(将工厂的灵活性提升)

- 通过配置文件取读, 工厂方法通过**反射**动态的去创建一个类

### 实现Ioc(最粗糙版的思想)

- 需要一个服务类的**分发框架入口类(Dispatcher)**, (例如spring有一个统一的入口,不断地调用和注入东西)
- 能够扫描一个服务类
- 将类创建出来注入给需要它的类, 用(**反射生成**)
- **动态注入**