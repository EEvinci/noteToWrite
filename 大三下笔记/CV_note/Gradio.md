# Gradio-Learning

[TOC]

## Interface函数-展示单个function

通常适用于一个功能的程序, 即对一个函数的功能进行展现

通过调用定义并调用接口`gr.Interface()`进行实现

`Interface`接口中有三个必须的参数:

- `fn` - function - 即功能函数
- `inputs` - 输入根据函数的输入参数, 可以是一个也可以是多个, 多个就用数组进行表示
- `outputs` - 输出, 输出和输入一样, 可以有一个或多个, 多个则用数组表示

### 程序示例

如下是一个`三个输入 / 两个输出`的一个打招呼功能函数

其中最后一个参数`temperature`使用了gradio中的`gr.Slider`模块, 进行了一个滑动的效果展示

```python
import gradio as gr

def greet(name, is_morning, temperature):
    salutation = "Good morning" if is_morning else "Good evening"
    greeting = f"{salutation} {name}. It is {temperature} degrees today"
    celsius = (temperature - 32) * 5 / 9
    return greeting, round(celsius, 2)

demo = gr.Interface(
    fn=greet,
    inputs=["text", "checkbox", gr.Slider(0, 100)],
    outputs=["text", "number"],
)
if __name__ == "__main__":
    demo.launch()

```

![image-20230321002520012](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230321002520012.png)

### 参数

| Parameter                                                    | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `fn`<br />Callable<br />required                             | the function to wrap an interface around. Often a machine learning model's prediction function. Each parameter of the function corresponds to one input component, and the function should return a single value or a tuple of values, with each element in the tuple corresponding to one output component. |
| `inputs`<br />str \|<br / IOComponent \|<br /> List[str \|<br / IOComponent] \| None<br />required | a single Gradio component, or list of Gradio components. Components can either be passed as instantiated objects, or referred to by their string shortcuts. The number of input components should match the number of parameters in fn. If set to None, then only the output components will be displayed. |
| `outputs`<br />str \|<br /IOComponent \|<br /> List[str \|<br / IOComponent] \|<br /> None<br />required | a single Gradio component, or list of Gradio components. Components can either be passed as instantiated objects, or referred to by their string shortcuts. The number of output components should match the number of values returned by fn. If set to None, then only the input components will be displayed. |
| `examples`List[Any] \| List[List[Any]] \| str \| None<br />default: None | **sample inputs for the function**; <br />if provided, **appear below the UI components** and **can be clicked to populate the interface**. <br />Should be nested list, in which the outer list consists of samples and each inner list consists of an input corresponding to each input component.<br />**A string path to a directory of examples** can also be provided, but it should be **within the directory with the python file running the gradio app**. <br />If there are multiple input components and a directory is provided, a log.csv file must be present in the directory to link corresponding inputs.numpy.core._exceptions._UFuncNoLoopError: ufunc 'multiply' did not contain a loop with signature matching types (dtype('<U1'), dtype('float64')) -> None |
| `cache_examples`<br />bool \| None<br />default: None        | If True, caches examples in the server for fast runtime in examples. The default option in HuggingFace Spaces is True. The default option elsewhere is False. |
| `examples_per_page`int<br />default: 10                      | If examples are provided, how many to display per page.      |
| `live`bool<br />default: False                               | whether the interface should automatically rerun if any of the inputs change. |
| `interpretation`<br />Callable \|<br /> str \| None<br />default: None | function that provides interpretation explaining prediction output. Pass "default" to use simple built-in interpreter, "shap" to use a built-in shapley-based interpreter, or your own custom interpretation function. For more information on the different interpretation methods, see the Advanced Interface Features guide. |
| `num_shap`<br />float<br />default: 2.0                      | a multiplier that determines how many examples are computed for shap-based interpretation. Increasing this value will increase shap runtime, but improve results. Only applies if interpretation is "shap". |
| `title`<br />str \| None<br />default: None                  | **a title for the interface**; <br />if provided, **appears above the input and output** components in large font. <br />Also used as the **tab title** when opened in a browser window. |
| `description`<br />str \| None<br />default: None            | **a description for the interface**; <br />if provided, appears **above the input and output** components and beneath the title in regular font. <br />Accepts Markdown and HTML content. |
| `article`<br />str \| None<br />default: None                | **an expanded article explaining the interface**;<br />if provided, appears below the input and output components in regular font. <br />Accepts Markdown and HTML content. |

### interface函数的子函数

- **launch**
- **load**
- **from_pipeline**
- **integrate**
- **queue**

它们的功能都很强大

[Gradio Docs](https://gradio.app/docs/#interface-launch-header)

![image-20230321003012402](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230321003012402.png)

回来慢慢看

## Flagging函数-返回错误case

在使用过程中如果遇到输出不符合预期值的点击flag即可向该程序的创建机器发送一条错误数据, 这些数据会在主机上保存为一个csv文件

balabala的剩下的自己慢慢看

Flagging函数也有三个子函数

![image-20230321004920682](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230321004920682.png)

## Combining-Interface功能-That's what i need!!!

### combining-interface的功能维度考虑

- **独立性** - 各个接口之间的功能独立, 要分别独自查看功能输出效果, **分多个标签页**进行功能展示
- **整体性** - 各个接口之间功能类似但不相同, 即输入相同输出不同, 要放在一起对比输出效果, 放在**一个标签页**中将输出进行组合展示
- **连续性** - 接口之间的功能有承接性, 即上一个接口的输出是下一个接口的输入, 此时可以将一众有前后逻辑连续性的接口放在一个队列中, 有序的协调好它们之间的前后关系, 然后通过一个输入和输出进行功能的展现

### 简介

当有**多个单独的interface**之后, 可以使用combining-interface功能将他们**组合**起来, **展示到同一个页面上**(不是同一个标签页)

如果各个接口之间的**输入是相同**的, 并且**输出是相关的**, 还可以使用其中的功能对**并行**比较接口之间的输出效果

如果各个接口的输入和输出是有**前后关联**的, 还可以将众多接口组合成一个更完整且庞大的功能

### TabbedInterface-选项卡式界面!!!-I love it!

#### 函数

一个选项卡式界面由**好多个接口**创建, 每一个接口的功能都在**一个单独的标签页面**中进行展现

```python
gradio.TabbedInterface(interface_list, ···)
```

#### 参数!

| Parameter                                                    | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `interface_list`<br />List[Interface]<br />**required**<br />必需的参数, 一系列的功能接口 | **a list of interfaces** to be rendered in tabs.             |
| `tab_names`<br />List[str] \| <br />Nonedefault: None<br />也比较需要, 需要自定义的给每一个标签页进行命名 | a list of tab names. <br />**If None, the tab names will be "Tab 1", "Tab 2", etc.**<br />所以还是自己定义一下吧 |
| `title`<br />str \| <br />Nonedefault: None<br />可有可没有, 有了更好, 进行一个简短的描述<br />但是我很疑惑为什么不是一个list, 难道不是每一个接口都需要一个自己的描述吗 | **a title for the interface**; <br />if provided, **appears above the input and output** components in large font. <br />Also used as the **tab title** when opened in a browser window. |

#### 示例

看了示例明白了, 接口中可以定义自己接口的`title`, 所以不用在`TabbedInterface`中定义了

```python
import gradio as gr

title = "GPT-J-6B"

tts_examples = [
    "I love learning machine learning",
    "How do you do?",
]

tts_demo = gr.Interface.load(
    "huggingface/facebook/fastspeech2-en-ljspeech",
    title=None,
    examples=tts_examples,
    description="Give me something to say!",
)

stt_demo = gr.Interface.load(
    "huggingface/facebook/wav2vec2-base-960h",
    title=None,
    inputs="mic",
    description="Let me try to guess what you're saying!",
)

demo = gr.TabbedInterface([tts_demo, stt_demo], ["Text-to-speech", "Speech-to-text"])

if __name__ == "__main__":
    demo.launch()

```

![image-20230321012216913](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230321012216913.png)

### Parallel !!! - I love it more!!😭

并行计算输出

考虑到你有相同类型的功能函数, 但是处理过程不同, 所以可以同时计算比较**不同函数功能之间输出的差异**

即输入一定要相同, 输出可以不同

更好的是这是构建在同一个页面上的, 而不是一个页面的多个标签页

#### 函数

```python
gradio.Parallel(interfaces, ···)
```

#### 参数

| Parameter                  | Description                                                  |
| -------------------------- | ------------------------------------------------------------ |
| `interfaces`<br />required | **any number of Interface objects** that are to be compared **in parallel**<br />任意的😭 |

#### 示例

示例中是文本, 就是不知道如果是图像网页要怎么进行排版, 或者是图像能不能进行并行输出

```python
import gradio as gr

greeter_1 = gr.Interface(lambda name: f"Hello {name}!", inputs="textbox", outputs=gr.Textbox(label="Greeter 1"))
greeter_2 = gr.Interface(lambda name: f"Greetings {name}!", inputs="textbox", outputs=gr.Textbox(label="Greeter 2"))
demo = gr.Parallel(greeter_1, greeter_2)

if __name__ == "__main__":
    demo.launch()
```

![image-20230321012301700](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230321012301700.png)

### 补充: lambda表达式

lambda表达式就是一个**简单的函数**, 不用进行一般的**函数的显示定义**, 即创建一个**匿名函数**

> lambda表达式是一种创建**匿名函数**的简便方式，它**不需要定义一个函数名或使用def关键字**。
>
> lambda表达式的语法是：
>
> **lambda 参数列表: 表达式**
>
> ```python
> append_nice = gr.Interface(lambda greeting: f"{greeting} Nice to meet you!",
>                            inputs="textbox", outputs=gr.Textbox(label="Greeting"))
> ```
>
> 
>
> 在上述语句中, lambda表达式**接受一个参数greeting**，它是一个字符串，然后**返回一个新的字符串**，它**在greeting后面加上" Nice to meet you!"**。
>
> 这个lambda表达式被传递给gr.Interface的fn参数，它是创建Gradio界面的主要函数。
>
> 这意味着当用户在输入框中**输入一个问候语**时，**Gradio界面会调用这个lambda表达式，并显示它的返回值。**

很清晰, chatbing很厉害

### Series

功能前面已经介绍过了

#### 函数

```python
gradio.Series(interfaces, ···)
```

#### 参数

| Parameter            | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| `interfaces`required | any number of Interface objects that are to be connected in series |

#### 示例

```python
import gradio as gr

get_name = gr.Interface(lambda name: name, inputs="textbox", outputs="textbox")
prepend_hello = gr.Interface(lambda name: f"Hello {name}!", inputs="textbox", outputs="textbox")
append_nice = gr.Interface(lambda greeting: f"{greeting} Nice to meet you!",
                           inputs="textbox", outputs=gr.Textbox(label="Greeting"))
demo = gr.Series(get_name, prepend_hello, append_nice)

if __name__ == "__main__":
    demo.launch()
```

![image-20230321014525106](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230321014525106.png)

## Blocks函数

