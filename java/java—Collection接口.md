### java—Collection接口

List、Set、Queue都是Colleciton的子接口

不能用接口创建接口对象

所以下面的写法是不对的

```java
List<String> list = new List();
```

应该用List下面的