# [org.springframework.web.HttpRequestMethodNotSupportedException: Request method 'GET' not supported]

## 问题

在执行controller中的方法时
```java
@PostMapping("/answer/create")
    String createSomeAnswer(@RequestBody Map<String, String> queryExample) {
        Integer uid = Integer.parseInt(queryExample.get("uid"));
        for(int i = 0; i < 3; i++){
            QaAnswerEntity answerEntity = new QaAnswerEntity();
            answerEntity.setContent("content" + RandomStringUtils.randomAlphabetic(10));
            answerEntity.setCreator(uid);
            repository.save(answerEntity);
        }

        return "done";
    }
```

出现

`[nio-8080-exec-1].w.s.m.s.DefaultHandlerExceptionResolver:Resolved [org.springframework.web.HttpRequestMethodNotSupportedException: Request method 'GET' not supported]`

报错

## 报错解释

这个错误是一个`HttpRequestMethodNotSupportedException`，表示请求的HTTP方法不受支持。

## 报错原因

根据错误日志，问题出在**请求方法**。错误提示请求方法'GET'不受支持。在你提供的代码片段中，你使用了`@PostMapping`注解，这意味着该方法仅支持HTTP POST请求。如果你尝试使用GET请求访问此端点（例如，在浏览器中输入URL或使用GET请求的工具），将会出现这个错误。

上述方法使用了`@PostMapping`注解，它**只支持HTTP POST请求**。

浏览器的地址栏通常只发送HTTP GET请求。如果你尝试在浏览器地址栏输入对应的URL来访问这个方法，将会收到一个错误，因为浏览器发送的是GET请求，而不是POST请求。

## 解决方法

需要确保使用POST请求访问此端点，而不是GET请求。你可以使用Postman、Curl或者其他支持发送POST请求的工具来测试这个端点。如果你确实想使用GET请求，那么需要将`@PostMapping`注解替换为`@GetMapping`，但这通常不建议用于创建资源的操作。

使用一个支持发送POST请求的工具，如Postman、Curl或者编写前端代码（例如，使用JavaScript的Fetch API或者jQuery的ajax方法）来发送POST请求。这样，你就可以正确调用这个方法并看到预期的响应。