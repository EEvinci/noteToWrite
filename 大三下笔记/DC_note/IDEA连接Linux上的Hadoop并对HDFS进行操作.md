# IDEA连接Linux上的Hadoop并对HDFS进行操作

[TOC]

## Windows软件准备

### 和Linux上**版本相同**的**Hadoop**

- **压缩包解压**: 将放在Linux上面的Hadoop压缩包(**hadoop_xxxx.tar.gz**)放在Windows**任意硬盘**中**任意**(建议新创建的一个Hadoop文件夹)**文件夹**, 然后**直接进行解压即可**, 不需要担心软件的系统适配问题

  ![image-20230326150748938](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326150748938.png)

- 配置`HADOOP_HOME`**环境变量**及添加`bin`和`sbin`目录的**系统路径**

  ![image-20230326151046849](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326151046849.png)

  ![image-20230326151127710](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326151127710.png)

- 验证配置是否成功, 在powershell中输入`hadoop -version`

  显示如下信息即表示配置成功

  ![image-20230326151350462](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326151350462.png)

### 与Linux**版本相同的Java**

- 同样需要**配置环境变量**

  ![image-20230326151434434](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326151434434.png)

### Windows的hadoop驱动文件`hadoop.dll `和`winutils.exe`

-  在GitHub上面下载, 如果**没有对应于自己当前Hadoop的版本**, 则选择**高一点点的**

  - [Download-link-Github](https://github.com/4ttty/winutils)

  - 然后将选择指定的版本中的`hadoop.dll`文件放到`./hadoop/bin/`下和`C:\Windows\System32\`下

    ![image-20230326152014818](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326152014818.png)

    ![image-20230326151915330](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326151915330.png)

  - 将`winutils.exe`文件也放到`./hadoop/bin/`下

    ![image-20230326152159995](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326152159995.png)

    并为该文件配置环境变量添加**系统路径**

    ![image-20230326152255414](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326152255414.png)

### 配置`Linux使用Hadoop的用户名`的环境变量`HADOOP_USER_NAME`

即在Linux中如果**使用Hadoop的用户是`hadoop`**, 则在环境变量配置的**变量值**中填入`hadoop`

![image-20230326152637938](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326152637938.png)

## IDEA中的操作

### 安装`big data tools`插件

![image-20230326152750677](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326152750677.png)

安装好之后, 右侧边栏会出现Big Data Tools的选项框

点击选项框, 在左上角的`+`中选择**添加`HDFS`**

![image-20230326152949374](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326152949374.png)

然后进行如下操作

![image-20230326153135272](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326153135272.png)

### 出现hdfs连接不上的情况

#### 第一种错误-`HADOOP_HOME`Error

出现`HADOOP_HOME`相关问题, 如果按照预先安装中的四个步骤应该没有这个问题

如果真的还有问题, 那只能再去Google

#### 第二种错误-`connectionError` Error

显示本机无法连接到Linux, `connectionError`

##### 使用`telnet`进行测试

`telnet <Linux-IP> 9000`

因为Hadoop服务的默认端口是`9000`, 这个是在`core-site.xml`中手动指定的

如果上述命令执行过之后显示**连接不成功**, 那么就是**端口的监听问题**

##### 问题原因

因为当前Hadoop的9000端口**仅允许本地访问**，需要更改为**允许远程访问**。

![image-20230326145454267](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326145454267.png)

##### 解决方法

在`core-site.xml`文件中更改`fs.defaultFS`属性的值为Linux服务器的IP地址，例如hdfs://192.168.1.100:9000，然后**重启Hadoop服务**以使更改生效。

![image-20230326154126678](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326154126678.png)

重启Hadoop步骤（假设已经为Hadoop中的bin和sbin目录配置了环境变量）：

- `stop-all.sh`
- `start-all.sh`
- `jps`查看相关进程以确认启动成功

- 

另外，确保防火墙设置允许来自Windows机器的流量通过9000端口。

完成这些步骤后，就能够从Windows机器上的IDE中成功连接到Linux服务器上的Hadoop并对HDFS进行操作。

![image-20230326150215149](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326150215149.png)

然后即可连接成功

![image-20230326145914858](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326145914858.png)

### 创建maven项目

#### 新建项目即可

![image-20230326154825837](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326154825837.png)

#### 删除相关无用文件和目录

![image-20230326155019244](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326155019244.png)

### 导入hadoop配置文件到resources中

从Linux中的`./hadoop/etc/hadoop/`目录中取出`core-site.xml`和`hdfs-site.xml`文件放入idea的`resources`目录中

![image-20230326161247524](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326161247524.png)

### 配置pom.xml文件

在`hadoop-client`中将版本改为自己的hadoop版本

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
		 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>org.example</groupId>
	<artifactId>JavaHadoopProJectS</artifactId>
	<version>1.0-SNAPSHOT</version>

	<packaging>jar</packaging>

	<dependencies>
		<dependency>
			<groupId>org.apache.hadoop</groupId>
			<artifactId>hadoop-client</artifactId>
			<version>2.10.2</version>
		</dependency>

        <!--解决关于slf4j的mutiple bindings问题 -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.7.25</version>
			<exclusions>
				<exclusion>
					<groupId>org.slf4j</groupId>
					<artifactId>slf4j-api</artifactId>
				</exclusion>
			</exclusions>
		</dependency>

	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-jar-plugin</artifactId>
				<version>2.4</version>
				<configuration>
					<archive>
						<manifest>
							<mainClass>org.hhrz.mapreduce.demo.JobMain</mainClass>
						</manifest>
					</archive>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>
```

PS:我在上述文件中加入了以下关于slf4j冲突所以去除依赖的语句

如果你没有该冲突问题, 并且该语句让你的程序运行出了问题, 你可以将其去掉

```xml
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.7.25</version>
			<exclusions>
				<exclusion>
					<groupId>org.slf4j</groupId>
					<artifactId>slf4j-api</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
```

### 编写WordCount代码进行测试

在其中对`hadoop.dll`文件的**路径设置**和`HADOOP_USER_NAME`的**value值设置**做自己的更改

```java
package hadoop;
import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;
import org.apache.log4j.BasicConfigurator;

public class WordCount {
    public static class Map extends Mapper<Object,Text,Text,IntWritable>{
        private static IntWritable one = new IntWritable(1);
        private Text word = new Text();
        public void map(Object key,Text value,Context context) throws IOException,InterruptedException{
            StringTokenizer st = new StringTokenizer(value.toString());
            while(st.hasMoreTokens()){
                word.set(st.nextToken());
                context.write(word, one);
            }
        }
    }

    public static class Reduce extends Reducer<Text,IntWritable,Text,IntWritable>{
        private static IntWritable result = new IntWritable();
        public void reduce(Text key,Iterable<IntWritable> values,Context context) throws IOException,InterruptedException{
            int sum = 0;
            for(IntWritable val:values){
                sum += val.get();
            }
            result.set(sum);
            context.write(key, result);
        }
    }

    static {
        try {
            // 填入自己的文件的路径
            System.load("E:\\Hadoop\\hadoop-2.10.2\\hadoop-2.10.2\\bin\\hadoop.dll");//建议采用绝对地址，bin目录下的hadoop.dll文件路径
        } catch (UnsatisfiedLinkError e) {
            System.err.println("Native code library failed to load.\n" + e);
            System.exit(1);
        }
    }

    public static void main(String[] args) throws Exception{
        BasicConfigurator.configure(); //自动快速地使用缺省Log4j环境。
        
        // 填入自己的环境变量中配置的value值
        System.setProperty("HADOOP_USER_NAME", "value");
        
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf,args).getRemainingArgs();
        if(otherArgs.length != 2){
            System.err.println("Usage WordCount <int> <out>");
            System.exit(2);
        }
        Job job = new Job(conf,"word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(Map.class);
        job.setCombinerClass(Reduce.class);
        job.setReducerClass(Reduce.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

### 返回Linux中在hdfs中创建输入文件和输出目录

基本命令

- `hadoop fs -mkdir /data` 创建输入目录

- `hadoop fs -mkdir /out` 创建输出目录

- `hadoop fs -put test.txt /data` 上传测试文件到data目录 

- `hadoop fs -cat /data/test.txt` 显示test.txt文件中的内容

  ```txt
  test file
  hello world
  ```

可在`Big Data Tools`中看到文件树

![image-20230326162452987](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326162452987.png)

### 指定程序在HDFS中的输入输出路径

#### 在**编辑配置**中进行设置

![image-20230326161728398](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326161728398.png)

#### 以`空格`为分割指定输入输出路径

![image-20230326161852211](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326161852211.png)

### 执行程序 查看输出结果

输出语句有几十行 最后几行程序输出如下

![image-20230326162926127](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326162926127.png)

同时查看侧边栏中的`HDFS` 可看到`out`目录中已经出现了输出结果

![image-20230326163038995](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230326163038995.png)

## 参考文档

[win10下IDEA连接虚拟机上的HDFS实现文件操作](https://blog.csdn.net/weixin_43387852/article/details/116698927)

[利用IDEA通过创建Maven项目来实现hadoop相关项目](https://blog.csdn.net/hxhxhzjz/article/details/124805608)

[在windows系统中安装配置hadoop环境变量](https://blog.csdn.net/weixin_41639302/article/details/107043562)

[idea连接本地虚拟机Hadoop集群运行wordcount](https://www.cnblogs.com/HusterX/p/14162985.html)

[Windows下IntelliJ IDEA远程连接服务器中Hadoop运行WordCount](https://blog.csdn.net/qq_44491709/article/details/107881124?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-3-107881124-blog-116698927.235%5Ev27%5Epc_relevant_3mothn_strategy_and_data_recovery&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-3-107881124-blog-116698927.235%5Ev27%5Epc_relevant_3mothn_strategy_and_data_recovery&utm_relevant_index=6)



[hadoop 9000端口只能本地127.0.0.1访问解决方案](https://blog.csdn.net/qq_43518182/article/details/115408409)

## `hadoop.dll `Download

[Download-link-Github](https://github.com/4ttty/winutils)