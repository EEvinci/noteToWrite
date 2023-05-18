# IDEA连接Linux服务器进行文件操作

[TOC]

## 连接的作用和意义

在IDEA中添加SFTP连接到Linux服务器，可以实现在IDEA中**直接对服务器上的文件**进行编辑、保存和上传操作，避免了**频繁切换**终端窗口进行文件操作的麻烦。

具体来说，添加SFTP连接后，可以使用IDEA中的**远程工具窗口**，**查看和编辑服务器上的文件**，**直接在IDEA中运行服务器上的脚本**，以及**在本地和远程之间拷贝文件**。

这样就可以方便地**在本地的IDEA环境中操作服务器上的文件**，提高了开发效率。同时，也避免了使用不同的终端窗口进行文件操作可能出现的不同步和错误问题。

## 安装openssh

`sudo apt-get install openssh-server`

## 开启openssh服务

`sudo /etc/init.d/ssh start`

![image-20230320160607106](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230320160607106.png)

## 验证是否开启服务

`ps -e | grep ssh`

![image-20230320160622130](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230320160622130.png)

## 安装网络工具包

`sudo apt install net-tools`

![image-20230320160645885](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230320160645885.png)

## 查看虚拟机IP地址

`ifconfig`

注意不是用在windows的`ipconfig`

![image-20230320160842119](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230320160842119.png)

## Idea连接Linux虚拟机

### 打开配置页面

![image-20230320161007877](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230320161007877.png)

### 配置SFTP

![image-20230320161104157](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230320161104157.png)

然后输入名称

### 配置SSH

![image-20230320161301186](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230320161301186.png)

![image-20230320161440844](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230320161440844.png)

测试连接成功之后即已经完成虚拟机Linux的连接

### 完成后出现的配置文件

![image-20230320161528325](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230320161528325.png)

## 安装`big data tools`插件

![image-20230326152750677](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326152750677.png)

安装好之后, 右侧边栏会出现Big Data Tools的选项框

点击选项框, 在左上角的`+`中选择**添加`SFPT`**

![image-20230326163335162](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326163335162.png)

将刚才配置的SFPT进行添加即可

![image-20230326163513728](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326163513728.png)

然后在侧边栏中即可看到Linux中的文件

![image-20230326163612003](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326163612003.png)