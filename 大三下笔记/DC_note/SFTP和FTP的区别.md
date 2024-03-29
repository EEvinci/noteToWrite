# SFTP和FTP的区别

SFTP（**Secure** File Transfer Protocol）和FTP（File Transfer Protocol）都是用于**文件传输**的**协议**，但它们有以下几个主要区别：

- 安全性：SFTP提供了一种安全的文件传输方式，它**使用SSH协议**进行数据加密和身份验证。FTP则是**明文传输**，传输的数据不加密，容易被窃听和攻击。

- 端口：SFTP使用**单一端口**（通常是22），而FTP使用**两个端口**（一个用于控制连接，另一个用于数据传输），需要开放更多的端口，容易受到网络攻击。

- 兼容性：**FTP是Internet标准，通用性较好**，几乎所有的操作系统和网络设备都支持FTP。**SFTP是SSH的一部分**，它需要安装SSH服务器和客户端，**只能在支持SSH的操作系统和设备上使用**。

- 功能：**SFTP比FTP功能更加强大**，支持对文件进行加密、压缩、校验等处理。SFTP还支持**文件和目录的远程复制、移动和删除等**高级功能，而FTP通常只能进行基本的**文件传输和管理**。


综上所述，SFTP比FTP更加安全、功能更加强大，但在某些情况下可能需**要安装SSH服务器和客户端才能使用**，而FTP则**更加通用**，但**不如SFTP安全**。