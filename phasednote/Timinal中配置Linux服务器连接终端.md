# Timinal中配置Linux服务器连接终端

## 效果演示

![image-20221115105458361](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221115105458361.png)

## 配置步骤

### 在teminal中添加新配置面板

**![image-20221115105551123](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221115105551123.png)**

### 添加新配置文件

![image-20221115105608363](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221115105608363.png)

### 修改名字，更换图标

![image-20221115110704043](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221115110704043.png)



### 修改新建终端的JSON配置文件

一定要先保存，JSON文件中才会显示这个终端的配置信息

![image-20221115110354574](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221115110354574.png)

![image-20221115111006225](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221115111006225.png)

可以直接通过修改Json文件来修改终端页面的各方面配置

放一下Data-Linux的配置信息

```json
{
                "backgroundImage": null,
                "commandline": "ssh DataLinux", 
                "experimental.retroTerminalEffect": false,
                "font": 
                {
                    "face": "Cascadia Mono",
                    "weight": "extra-black"
                },
                "guid": "{fc0a9e6a-ba23-4047-820f-7138c138bf65}",
                "hidden": false,
                "icon": "D:\\Include\\\u56fe\u7247\\QQ\u56fe\u724720210811171052.jpg",
                "name": "lc@Data_Linux",
                "opacity": 56,
                "padding": "16",
                "tabTitle": "Data_Linux",
                "useAcrylic": true
            }
```

直接根据上面的配置信息修改Tencent-Linux的配置

![image-20221115111533960](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221115111533960.png)

### 配置.ssh目录中的config文件

如果没有config文件, 用powershell通过命令`new-item "config" -type file`创建config文件

这个config文件没有文件类型后缀, 就只是config

创建好之后, 用powershell在`.ssh`目录下通过命令`notepad config`命令, 在config文件中添加以下内容:

```txt
# Tencent-Linux
Host    这个地方的名称一定要和teminal的 Json配置文件中的 Commandline属性内容中的 ssh后面的 名字相同      
HostName      服务器的公网ip
Port          22
User          登录服务器的用户名
```

保存配置, 然后就可以在teminal中输入密码登录服务器了

![image-20221115112414773](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221115112414773.png)

登录有两种方式, 一种是通过密钥登录, 另一种是通过密码登录

如果是通过密钥登录, 在config文件中要配置密钥的存放位置, 也就是IdentityFile属性后面的内容, 在登陆的时候进行读取, 直接登录

如果是密码登录就是开头的展示界面, 要自己输入密码才能够登录

![image-20221115112748156](http://evinci.oss-cn-hangzhou.aliyuncs.com/evinci/image-20221115112748156.png)

配置好之后保存

然后在teminal中就可以进行服务器的访问了