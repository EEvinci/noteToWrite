[TOC]

## B/S结构

### httpservlet & servlet

通过servlet和HTTPservlet规范web开发的结构

tomcat十一个标准的Java服务器，调用servlet方法并实现方法

- 收到某种请求，根据servlet的规则实现一组接口（投递到一个servlet的实现）

通过写httpservlet的各种do方法来响应各种的请求

通过写servlet写service服务

用spring的机制更方便的写接口，底层是servlet

### 实现httpservlet

相应一个echo方法，并返回一个最简单的html文档

**理解http的工作方式**（充分了解http的工作流程）

- 得到user-Agent 的头部（头部显示请求的客户端软件的信息）
- 在response报文中设置信息
  - 响应的状态（200/404/502）
  - 相应的文本格式（html）
    - html是一套（规则）完整的标签体系，为浏览器工作，即brower以其为基础进行网页的构建
      - h5在html的基础上进行了一定的扩展
      - https://developer.mozilla.org

## 前后端不分离的时代

前后端一体, 前端是后端的一部分

- 后端收到request
- 生成html
- 发送到browser

## JSP/ASP模式

后端采用MVC模式

- M: 包含着数据的实体对象
- C: 控制请求的逻辑运算
- V: 提供用户请求和交互的支持界面(页面在前后端分离的时代中, 不在后端代码中进行体现, 而是由纯粹的js代码构建, 由此诞生了前端程序员(前端工程化))

## SpringMVC

> 控制反转（Inversion of Control，缩写为IoC），是**面向对象编程中的一种设计原则，可以用来减低计算机代码之间的耦合度**。 其中最常见的方式叫做依赖注入（Dependency Injection，简称DI），还有一种方式叫“依赖查找”（Dependency Lookup）。

Spring本体提供基于控制反转的开发框架

springboot: 快速开发的框架

SpringMVC: 前后端分离的后端的框架



view: 就是以一种结构化的格式返回数据的方式

- html
- json
- 以web service为代表的xml
- 甚至可以是纯文本

Java对象可以自动返回为json, 所以如果返回的是json就不需要写view了



### 在springboot中的的依赖



## restcontroller and controller

- 返回json: restcontroller
- 返回页面: controller

两者的返回类型不同

### springMVC怎么自动json报文

springMVC有一个自动的报文转换机制

当返回一个对象时, 会尝试去构造一个view, 如果构造不出来, 就会启动转换机制



引入依赖

## mapping

GetMapping and RequestMapping

用不同的mapping返回不同的url, 和请求的链接联系起来

## 模板系统

模板系统, 引入脚本, 构建与Java之间的关系, 从而动态的控制脚本

在html中嵌入脚本标记, 将脚本的运行结果插入特定变量位置然后进行页面的返回

**thymeleaf模板**

## 两种遵守

- 框架
- 约定: getter and setter

## springboot中的html

在resources中的templates中

进行前端页面的html展示

在controller中进行调用, 以此进行前后端的连接

## 接口定义

功能说明

请求方法

body参数说明

返回参数说明   你

**面向接口的编程**: 通过写注释生成接口文档