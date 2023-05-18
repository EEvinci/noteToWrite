# Spring boot中formBean包的作用

## formBean的作用

在传统的 Java Web 开发中，通常使用**表单**来收集**用户提交的数据**。

formBean 包就是用来**存放这些表单对象**的。

## formBean概念

formBean 是一个**数据传输对象**（**DTO**，Data Transfer Object）的概念，用于在**表现层**（即用户界面）和**业务逻辑层**之间传输数据。

- 在表现层中，formBean 通常表示用户提交的表单数据
- 在业务逻辑层中，formBean 则作为参数传入业务逻辑方法中，或者作为业务逻辑方法的返回值返回给表现层。

## 使用formBean层的优点

使用 formBean 的好处:

- **将用户提交的数据封装成一个对象**，可以方便地进行数据校验和处理
- 减少了业务逻辑层和表现层之间的耦合
- 提高了代码的可维护性和可扩展性

在 Spring Web MVC 框架中，通常使用 **@ModelAttribute** 注解**将表单数据绑定到 formBean 对象中**。



下面的代码演示了如何将一个名为 "user" 的表单数据绑定到一个名为 "userForm" 的 formBean 对象中：

```java
@RequestMapping("/submitForm")
public String submitForm(@ModelAttribute("userForm") UserForm userForm) {
    // 对表单数据进行处理
    // ...
    return "success";
}
```

在这个例子中，我们使用 @ModelAttribute 注解将表单数据绑定到 UserForm 对象中，并将这个对象作为参数传入 submitForm() 方法中。

通过这种方式，我们可以方便地对表单数据进行处理，而不必手动从 HttpServletRequest 对象中获取表单数据。