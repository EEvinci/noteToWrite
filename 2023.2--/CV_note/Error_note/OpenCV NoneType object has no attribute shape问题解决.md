# OpenCV NoneType object has no attribute shape问题解决

![image-20230321024346262](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20230321024346262.png)

检查使用函数`imread`时**填入的图片的路径**

保证当前工作空间**只有一个项目**, 然后使用**相对路径**没问题

如果有**多个项目都在一个文件夹下**, 又是在该文件夹的**根部**打开了项目, 那么图片的路径建议使用**绝对路径**