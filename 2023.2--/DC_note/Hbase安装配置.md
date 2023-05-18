# Hbase安装配置

[TOC]

## Hbase安装前提

- JDK
- Hadoop(Hadoop自带了zookeeper, 所以不需要额外进行下载)

## 下载Hbase压缩包

[Hbase下载](https://archive.apache.org/dist/hbase/)

## 软件版本兼容性

### Hadoop和Hbase

Hadoop和[Hbase](https://so.csdn.net/so/search?q=Hbase&spm=1001.2101.3001.7020)的匹配关系可以查看Hbase官方文档，搜索‘Hadoop version support matrix’： http://hbase.apache.org/book.html#basic.prerequisites

HBase与Hadoop版本对应关系如下：

[![图片描述](http://wiki.xuwei.tech/allpic/5fa365cb0961c7d613210938.jpg)](http://wiki.xuwei.tech/allpic/5fa365cb0961c7d613210938.jpg)

查看自己的hadoop版本为**2.10.2**

![image-20230410151919747](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230410151919747.png)

release中的Hbase的**2.3.x系列版本**

![image-20230410151800846](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230410151800846.png)

### Hbase和JDK

HBase与JDK版本对应关系如下：

[![图片描述](http://wiki.xuwei.tech/allpic/5fa365c509bf51ce12480471.jpg)](http://wiki.xuwei.tech/allpic/5fa365c509bf51ce12480471.jpg)

![image-20230410155934961](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230410155934961.png)

## 软件安装

### 软件位置

`/usr/loca/hbase`

![image-20230417155107218](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230417155107218.png)

### 创建数据保存和日志保存文件夹

`hbase_data` 用于保存hbase产生数据的目录

`hbase_log` 用于记录hbase操作的日志目录

`zookeeper_data` 用于保存zookeeper产生数据的目录

![image-20230417155300092](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230417155300092.png)

## 修改配置文件

### 修改`hbase-site.xml`文件

> 在Hadoop、HBase等Apache项目中，通常有一些默认的配置文件，如`hadoop-default.xml`或`hbase-default.xml`。这些默认的配置文件包含了项目的默认设置。
>
> 在部署这些项目时，为了避免直接修改默认配置文件，我们通常会创建一个名为`<软件名>-site.xml`的新配置文件，如`hadoop-site.xml`或`hbase-site.xml`。这个"site"后缀表示这些配置是针对您特定部署环境的。这样，在项目更新时，您可以保留您的部署特定设置，而不必担心与默认设置发生冲突。这些"site"配置文件中的设置会覆盖默认配置文件中的相应设置。

```xml
  <property>
    <name>hbase.cluster.distributed</name> <!--是否是分布式配置-->
    <value>true</value>
  </property>

  <property>
    <name>hbase.tmp.dir</name> <!-- 缓存文件的保存目录 -->
    <value>./tmp</value>
  </property>

  <property>
    <name>hbase.unsafe.stream.capability.enforce</name> <!-- 不用管 -->
    <value>false</value>
  </property>

  <property>
    <name>hbase.rootdir</name>
    <value>file:///usr/local/hbase/hbase-2.3.1/hbase_data</value> <!-- hbase的data保存目录,需要手动创建 -->
  </property>

  <property>
    <name>hbase.zookeeper.quorum</name> <!-- 表示使用hbase自带的zookeeper -->
    <value>localhost</value>
  </property>

  <property>
    <name>hbase.zookeeper.property.clientPort</name> <!-- zookeeper的端口号 -->
    <value>2181</value>
  </property>

  <property>
    <name>hbase.zookeeper.property.dataDir</name> <!-- zookeeper的data保存目录 -->
    <value>/usr/local/hbase/hbase-2.3.1/zookeeper_data</value>
  </property>
```

### 修改`hbase-env.sh`文件

添加自己的**Java和Hadoop变量**

以及**日志保存目录变量**

```sh
export JAVA_HOME=/usr/local/Java/jdk1.8.0_361
export HADOOP_HOME=/usr/local/hadoop
export HBASE_MANAGES_ZK=false
export HBASE_LOG_DIR=/usr/local/hbase/hbase-2.3.1/hbase_log
```

### 修改~/.bashrc文件

添加hbase的环境变量

使得能够全局使用bin中的命令

```sh
#hbase
export HBASE_HOME=/usr/local/hbase/hbase-2.3.1
export PATH=$PATH:$HBASE_HOME/bin
```

## 启动hbase并验证

使用`start-hbase.sh`进行启动

使用`jps`进行验证, 如果出现hbase相关的`HRegionServer`和`HMaster`进行则说明**启动成功**

![image-20230417160239413](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230417160239413.png)

## 权限问题Permission denied

### 修改用户和用户组

更改目标目录的所属用户和所属用户组为当前用户

```shell
sudo chown -R yourUserName:yourUserName <floderName>
```

### 修改目标目录的权限

通常出现报错都是因为当前用户对目标文件夹**没有写权限**

所以需要开放该目录对当前用户的**写权限**

`chmod -R 755 folder`： 这个命令用于更改指定文件夹（以及其子文件夹和文件）的权限。

权限是以三位数字表示的，每个数字分别代表**文件所有者**、**文件所属组**和**其他用户**的权限。

例如`755`表示：

- 所有者（第一个数字，即7）：具有读、写和执行权限（7 = 4 + 2 + 1，其中**4表示读权限，2表示写权限，1表示执行权限**）
- 用户组（第二个数字，即5）：具有读和执行权限（5 = 4 + 1）
- 其他用户（第三个数字，即5）：具有读和执行权限（5 = 4 + 1）

所以在**确定当前用户在目标目录或文件的用户组中**，可以使用一下命令进行权限的修改

```shell
sudo chown -R 775 <floderName>
```

## SLF4J问题: Class path contains multiple SLF4J bindings.

### SLF4J的多重绑定问题：

这个问题是由于在类路径中找到了**多个SLF4J绑定**导致的。虽然这个问题不会直接导致HBase启动失败，但建议解决它以避免潜在的问题。

要解决这个问题，**可以从Hadoop或HBase的`lib`目录中删除其中一个绑定。**

可以删除HBase的`lib/client-facing-thirdparty`目录下的`slf4j-log4j12-1.7.30.jar`文件。这样SLF4J就只会使用Hadoop中的`slf4j-reload4j-1.7.36.jar`绑定。

## Hbase应用结合

Hbase + Redis

Hbase + solr

构建用户画像

## 参考文档

[Hadoop、Hbase、Hive和zookeeper版本兼容关系](https://blog.csdn.net/qq_39261894/article/details/105134340)

[linux上部署最新版本zookeeper伪分布式集群](https://juejin.cn/post/7077892208560455716)

[HBase 伪分布式模式安装与启动](https://cloud.tencent.com/developer/article/1545320)

[HBase集群教程](http://wiki.xuwei.tech/hbase/install.html)