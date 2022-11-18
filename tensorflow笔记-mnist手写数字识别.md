# tensorflow笔记-mnist手写数字识别

[TOC]



### 导入数据集

以下是tensorflow1.x的数据集导入版本

```python
from tensorflow.example.tutorials.mnist import input_data  
```

tensorflow2.x的数据集被封装到keras中

```python
mnist = tf.keras.datasets.mnist
```

keras中有七种类型的数据集

![image-20221107140056106](E:\Typora\ty_Photo\image-20221107140056106.png)

| 数据集名称     | 数据集描述               |
| -------------- | ------------------------ |
| boston_housing | 波士顿房价数据           |
| cifar10        | 10种类别图片集           |
| cifar100       | 100种类别图片集          |
| fashion_mnist  | 10种时尚类别图片集       |
| imdb           | 电影评论情感分类数据集   |
| **mnist**      | **手写数字图片集**       |
| reuters        | 路透社新闻主题分类数据集 |

这些数据集的读取都可以使用`load_data()`方法

以mnist手写数字集为例进行数据读取:

```python
(x_train, y_train),(x_test, y_test) = mnist.load_data()
```

其中对`(x_train, y_train),(x_test, y_test)`中的各个变量进行解释:

- x_train和y_train是训练集:
  - x_train的数据形状是(60000, 28, 28), 即60000张28x28像素格的手写数字图片
  - y_train是标签, 也就是x_train中的书写数字图片真实对应的数字, 数据形状是(60000,)
- x_test和y_test是测试集
  - x_test是10000张数字图片
  - y_test是10000张与数字图片真实对应的数字

#### **参考资料:**

**csdn资料:**

[keras中的数据集](https://blog.csdn.net/mogoweb/article/details/81740133)

**keras官网对数据集的说明**

[MNIST digits classification dataset (keras.io)](https://keras.io/api/datasets/mnist/)

------

### 划分数据集

keras中的mnist数据集只有**训练集**和**测试集**

但是应在分组数据之外创建第三个数据集, 即**验证集**

`验证集和测试集应该来源于同一分布，且与真实需求一致`(所以验证集在严格方面上, 应该从测试集中进行划分, 但是因为一般训练集和测试集都是**相同类型的数据**, 所以从训练集中划分验证集也是合理的)

为的是在模型训练过程中**观察模型的泛化能力**, 即模型的效果

从而**调整超参数的值**, 让模型性能达到最佳，这个调节过程需要**使用模型在验证集上的性能表现作为反馈信号**。

> 因为一般手中拿到的都是历史数据，通过历史数据的学习去预测未知新数据。如果用全量历史数据进行学习，并进行验证，得到的验证结果是学习数据的准确率。
>
> 模型的调优方向，让模型具有更强的对未知数据的预测能力，也叫做泛化能力。
>
> 通过从历史数据中抽出一部分作为验证集（测试集），对当前学习到的模型进行泛化能力的验证。如果验证集的准确率和训练集的准确率相当，那么说明模型的泛化能力是足够的.

调节超参数的过程本质上就是一种学习：**在某个参数空间中寻找良好的模型配置**。

#### 以下是纯纯的个人理解:

训练集和测试集应该是相同类型的数据, 两种数据集拥有**一定程度上共同的特征**. 

我们想要机器做的事情是通过在训练集的特征和标签学习**最大程度上提取出来这些共同的特征**, 从而在相同数据类型的测试集上有着良好的预测表现

但是如果只在训练集上进行训练, 不用验证集来判断模型的训练进行到了一个什么样的程度, 那么很容易对训练集**训练过度**

即提取了好多训练集中因为**数据的随机分布而产生的独有的细微的特征**, **模型不仅在共同的特征上面拟合了, 而且在训练集的这些细微的特征上面也拟合了**, 所以在面对测试集的特征时就发现好多特征不对应, 从而无法得到相应的正确的预测结果, 这就是由于在训练集上的盲目训练而导致的**过拟合现象**, 导致模型的**泛化能力很差**.



#### 三种数据集的作用:

训练集: 训练模型内参数

验证集: 调整超参数, 根据几组模型验证机上的表现, 决定哪组超参数拥有最好的性能, 同时用来监控模型是否出现了过拟合

测试集: 评价模型的泛化能力



#### 训练集、测试集、验证集三者区别

##### 三者区别举例： 

我们把学生能力类比模型能力 训练集就像是学生的课本，学生根据课本里的内容来掌握知识， 验证集就像是练习作业， 通过作业可以知道不同学生学习情况、进步的速度快慢， 而最终的测试集就像是考试， 考的题是平常都没有见过，考察学生**举一反三**的能力。

##### 具体区别:

训练集直接参与了**模型调慘**的过程， 显然不能用来反映模型真实的能力， 这样一些对课本**死记硬背**的学生将会拥有最好的成绩， 这是过拟合，显然不对。

同理，由于验证集参与了**人工调参**的过程， 也不能用来最终评判一个模型， 就像**刷题库**的学生也不能算是学习好的学生是吧。 所以要通过最终的**考试**， 也就是**测试集**来考察一个学生模型的真正的能力。

对于用于比赛的公开数据集， 验证集会公开标注，测试集不会公开。

千万不要用测试数据来训练模型！ 不然就监守自盗了！

### 数据预处理



### 构建模型



### 定义损失函数



### 定义优化器

优化器之间的比较和深入理解



### 定义超参数



### 训练模型







### 训练



#### 对于epochs/batch_size/Iteration的概念判别

- epoch代表**一代训练**: 即使用训练集的全部数据对模型进行一次完整的训练
- batch代表**一批数据**: 即当数据量很大时, 无法一次将所有数据全部加载到内存中进行训练, 此时要将数据进行分批次
  - batch_number = training_set_Size / batch_size
- iteration(迭代)代表**一次训练**: 使用一个batch数据对模型进行一次训练的过程, 称之为一次训练



​	

mooc上用的方法还是线性模型, 使用梯度下降进行优化的模型

但是手写数字识别可以使用卷积神经网络来进行实现



首先将mooc中的模型使用搞明白, 也是一种模型的训练和使用流程

重点是要知道函数的使用方法和功能

关于模型的训练模块如下所示

搞清楚在模型训练的时候做的事情

```python
for epoch in range(epochs):
    for step in range(steps):
        xs = train_x[step*batch_size:(step+1)*batch_size]
        ys = train_y[step*batch_size:(step+1)*batch_size]

        grads = grad(xs, ys, W, B)
        optimizer.apply_gradients(zip(grads, [W, B]))

    train_loss = loss(train_x, train_y, W, B).numpy()
    valid_loss = loss(valid_x, valid_y, W, B).numpy()
    train_accuracy = accuracy(train_x, train_y, W, B).numpy()
    valid_accuracy = accuracy(valid_x, valid_y, W, B).numpy()

    train_losses.append(train_loss)
    valid_losses.append(valid_loss)
    train_accuracys.append(train_accuracy)
    valid_accuracys.append(valid_accuracy)

    print('epoch={:3d}, train_loss={:.4f}, train_acc={:.4f}, val_loss={:.4f}, val_acc={:.4f}'.format(\
        epoch+1, train_loss, valid_loss, train_accuracy, valid_accuracy))
```

要搞清楚模型的训练过程, 免不了在上面回顾模型的定义过程:

模型定义过程中, 用了一些高级框架封装的函数, 要去搜索这些函数的功能和使用方法, 搞清楚内部做的细节

```python
#构建神经网络模型
def model(x, w, b):
    pred = tf.matmul(x, w) + b
    return tf.nn.softmax(pred)

#定义损失函数
def loss(x, y, w, b):
    pred = model(x, w, b)
    loss_ = tf.keras.losses.categorical_crossentropy(y_true=y, y_pred=pred)
    return tf.reduce_mean(loss_)

#计算梯度
def grad(x, y, w, b):
    with tf.GradientTape() as tape:
        loss_ = loss(x, y, w, b)
    return tape.gradient(loss_, [w, b])

#定义准确率
def accuracy(x, y, w, b):
    pred = model(x, w, b)
    corrections = tf.equal(tf.argmax(pred, axis=-1), tf.argmax(y, axis=-1))
    return tf.reduce_mean(tf.cast(corrections, tf.float32))
```

