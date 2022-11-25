# SpringBoot-demo

### 首先在`resources`中检查端口

![image-20221110175744444](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221110175744444.png)

![image-20221110175751768](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221110175751768.png)

### 创建一个`controller`, 在里面创建一个`helloworld.java`文件

![image-20221110175849902](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221110175849902.png)

```java
package com.example.seconddemo.controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloWorld {
    @RequestMapping("/")
    public String helloWorld() {
        return "hello world";
    }
}
```

### 然后在`Application`中运行

![image-20221110175958667](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221110175958667.png)

运行成功

### 在浏览器中打开

输入`localhost:端口号`, 显示页面, 说明demo运行成功

![image-20221110180117229](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221110180117229.png)