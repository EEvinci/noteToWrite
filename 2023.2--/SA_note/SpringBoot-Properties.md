# Spring Boot-Properties

## Properties

Properties是Java语言中用于管理配置信息的类，它继承自Hashtable类，可以将属性名和属性值存储在一个文件中，用于读取和写入配置文件。Properties文件通常用于存储程序配置信息、环境变量、国际化字符串等。可以通过java.util.Properties类的load()方法从属性文件中加载配置信息，通过store()方法将配置信息写入到属性文件中。

## Try-catch 和 Try-with-resources

try-catch语句是Java中异常处理的核心语句。它允许我们在代码块中捕获和处理异常。当我们需要处理一个会抛出异常的方法时，我们需要将这个方法的调用语句放入try块中，并在catch块中处理异常。

try-with-resources语句是从Java 7开始引入的语法，它可以用于自动关闭实现了java.lang.AutoCloseable或java.io.Closeable接口的资源，比如文件、数据库连接、网络连接等等。当我们在try块中打开一个资源，这个资源会在try块执行完毕后自动关闭，无需我们手动调用close()方法来关闭它。try-with-resources语句可以让我们的代码更加简洁，避免忘记关闭资源等一系列问题。

使用try-with-resources的语法如下：

```
javaCopy codetry (Resource resource = new Resource()) {
    // do something with the resource
} catch (Exception e) {
    // handle exception
}
```

## Closeable 和 AutoCloseable 接口

Closeable和AutoCloseable都是Java中提供的接口，它们用于表示一些需要关闭的资源，如文件、流、数据库连接等等。这些资源通常需要显式地关闭，以确保释放系统资源并避免泄漏。

Closeable接口定义了close()方法，用于关闭该对象，实现Closeable接口的对象可以通过try-with-resources语句自动关闭。

AutoCloseable接口也定义了close()方法，与Closeable接口类似，但是它还可以使用try-with-resources语句自动关闭，它是从Java 7引入的，是Closeable的父接口。

在使用一个类创建对象时，如果该类实现了Closeable或AutoCloseable接口，则需要在使用完该对象后显式地关闭该对象。



## vscode-jdk设置

![image-20230301142537536](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230301142537536.png)

![image-20230301142718332](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230301142718332.png)

![image-20230301142733273](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230301142733273.png)

![image-20230301142842378](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230301142842378.png)

![image-20230301143117310](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230301143117310.png)

![image-20230301143559246](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230301143559246.png)