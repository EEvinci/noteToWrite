# Linux-Hadoop配置总结

[TOC]

## Hadoop是什么

Hadoop是一个开源的分布式计算平台，旨在处理大规模数据集。它的核心是分布式文件系统HDFS和分布式计算框架MapReduce。Hadoop可以运行在由成百上千台服务器组成的集群上，能够处理海量的数据。它最初是由Apache软件基金会开发的，目前已成为处理大数据的主要解决方案之一。

Hadoop的设计基于Google发表的一系列论文，描述了如何在成百上千台普通服务器组成的集群上，快速地处理大规模数据集。Hadoop能够自动地将数据分散存储在集群中的多个节点上，并利用MapReduce框架来分布式计算和处理数据。Hadoop还支持许多其他的数据处理和分析工具，如Hive、Pig和Spark等，使得用户能够使用各种编程语言和工具来处理和分析数据。

## Hadoop不推荐使用root用户进行配置

启动Hadoop集群的用户不建议使用root用户，而应该使用一个专门的Hadoop用户来启动集群。

这是因为在Hadoop运行过程中，会涉及到大量的文件读写和网络通信等操作，如果使用root用户启动Hadoop，可能会带来安全性和权限控制等问题。

而应该创建一个专门的Hadoop用户，用于启动和管理Hadoop集群。

创建Hadoop用户的具体步骤如下：

1. 以root用户登录Linux系统。

2. 使用以下命令创建一个新的用户和用户组，用于启动和管理Hadoop集群：

   ```
   groupadd hadoop
   useradd -g hadoop hadoop
   ```

3. 设置Hadoop用户的密码：

   ```
   passwd hadoop
   ```

4. 修改Hadoop用户的home目录的权限：

   ```
   chown hadoop:hadoop /home/hadoop
   ```

   这里假设Hadoop用户的home目录为/home/hadoop。

5. 将Hadoop用户添加到sudoers文件中，以便它可以使用sudo命令执行需要root权限的操作：

   ```
   visudo
   ```

   在打开的sudoers文件中，添加以下行：

   ```
   hadoop ALL=(ALL) ALL
   ```

6. 切换到Hadoop用户，使用以下命令测试是否可以使用sudo命令：

   ```
   sudo whoami
   ```

如果以上步骤执行正确，就可以使用Hadoop用户来启动Hadoop集群了。

## 如果使用root用户来安装Hadoop，普通用户将无法启动Hadoop

因为Hadoop所需的文件和目录的权限只授予了root用户。为了让普通用户能够启动Hadoop， 需要在安装Hadoop时使用sudo命令来授权普通用户安装Hadoop，并在安装和配置过程中，确保所有的文件和目录都授予了普通用户相应的权限。

如果 使用了root用户来安装Hadoop，并希望普通用户也能够启动Hadoop， 可以使用以下命令修改Hadoop所需文件和目录的权限：

```
sudo chown -R hadoop:hadoop /path/to/hadoop
sudo chmod -R 755 /path/to/hadoop
```

其中，/path/to/hadoop是指Hadoop的安装目录。这将把Hadoop的安装目录和其子目录的所有者和组设置为hadoop，并设置文件和目录的权限，以便普通用户也可以读取、写入和执行它们。

## 查看一个用户所属的用户组

```shell
groups <username>
```

## 如果使用root用户对压缩包进行解压，那么解压后的文件和文件夹的所属用户和用户组都会是root用户

因为在Linux系统中，文件和文件夹的所有者和组是在创建时指定的。

如果希望解压后的文件和文件夹的所有者和组为其他用户，可以在解压之后使用chown命令更改文件和文件夹的所有者和组。例如，要将解压后的文件和文件夹的所有者和组设置为hadoop用户和hadoop组，可以使用以下命令:

```
sudo chown -R hadoop:hadoop /path/to/unzip/folder
```

其中，/path/to/unzip/folder是指要更改所有者和组的解压后的文件夹路径。

## 将root添加到hadoop用户的用户组中

要将root用户添加到Hadoop用户组中，可以使用以下命令：

```
sudo usermod -a -G hadoop root
```

其中，`-a`选项表示添加用户到组中，`-G`选项指定要添加用户到的组名称，这里是hadoop，`root`是要添加到hadoop组中的用户名。

执行上述命令后，root用户就会被添加到hadoop组中，可以使用hadoop用户组的权限访问Hadoop服务所需的文件和目录，并可以运行Hadoop服务。注意，为了确保root用户在添加到hadoop组中后可以访问Hadoop服务所需的文件和目录，确保这些文件和目录具有适当的文件权限和所有者/组。

## 在将root添加到hadoop用户组之后, 仍然无法运行hadoop用户进行的配置

如果在将root用户添加到Hadoop用户组之后仍然无法运行Hadoop用户进行的配置，检查以下几个方面：

1. 确认root用户已经成功地添加到hadoop用户组中。可以使用以下命令来验证：

   ```
   groups root
   ```

   如果输出结果包含hadoop组名称，则表示root用户已经成功添加到hadoop组中。

2. 检查Hadoop配置文件和文件夹的权限。确保hadoop用户组具有读取和执行Hadoop配置文件和文件夹的权限。可以使用以下命令来检查权限：

   ```
   ls -l /path/to/hadoop/config/file
   ```

   确保hadoop用户组具有读取和执行权限（r-x）。

3. 确认Hadoop服务正在以hadoop用户身份运行。可以使用以下命令来验证：

   ```
   ps -ef | grep hadoop
   ```

   确保所有Hadoop服务都正在以hadoop用户的身份运行。

## 在hadoop用户中配置了java, 并将root添加进了hadoop的用户组中, 但是我在以root进行登录时无法使用Java相关的命令

在Linux中，用户的环境变量通常是在登录时设置的，而在切换用户后，用户的环境变量通常不会自动更新，因此 可能需要手动更新root用户的环境变量才能使用hadoop用户配置的Java。

可以尝试以下步骤：

1. 以root用户登录到系统中。

2. 执行以下命令来切换到hadoop用户并加载其环境变量：

   ```
   su - hadoop
   ```

3. 然后，执行以下命令来查找Java可执行文件的位置：

   ```
   which java
   ```

4. 复制Java可执行文件的路径。

5. 然后，退出hadoop用户并以root用户身份打开root用户的`.bashrc`文件。

6. 将以下行添加到`.bashrc`文件的末尾：

   ```
   export JAVA_HOME=/path/to/java
   export PATH=$JAVA_HOME/bin:$PATH
   ```

   其中，`/path/to/java`应替换为步骤3中查找到的Java可执行文件的路径。

7. 保存并关闭`.bashrc`文件。

8. 执行以下命令来重新加载root用户的`.bashrc`文件并更新其环境变量：

   ```
   source ~/.bashrc
   ```

现在， 应该能够在以root用户身份登录的情况下使用hadoop用户配置的Java了。

## 在 Ubuntu 中，默认情况下，root 账户是禁用的，因此不能直接以 root 用户身份登录系统

可以使用以下两种方法之一以 root 用户身份执行命令：

1. 使用sudo命令：

   sudo 是一种在 Ubuntu 中以超级用户或其他用户身份运行命令的方式。要使用 sudo 命令，需要在 Ubuntu 中使用一个已授权的用户帐户（通常是具有管理员权限的帐户）。执行以下命令以使用 sudo 身份运行命令：

   ```
   sudo command
   ```

   其中，command 是要运行的命令。

   比如，要以 root 用户身份编辑文件 /etc/hosts，可以执行以下命令：

   ```
   sudo nano /etc/hosts
   ```

2. 在Ubuntu中启用root账户：

   如果希望在 Ubuntu 中直接以 root 用户身份登录系统，执行以下步骤启用 root 账户：

   a. 打开终端并执行以下命令以设置 root 用户的密码：

   ```
   sudo passwd root
   ```

   系统会提示输入当前用户的密码，然后提示输入新的 root 密码并确认该密码。

   b. 执行以下命令以启用 root 账户：

   ```
   sudo usermod -p '*' root
   ```

   此命令将 root 用户的密码设置为星号，这意味着任何人都可以使用 root 用户身份登录系统。

   c. 现在，可以注销当前用户并在登录屏幕上选择 root 用户进行登录。

注意，在启用 root 账户后，其他用户也可以使用 root 用户身份登录系统，因此确保仅授权给可信用户访问 root 用户身份。

## 怎么设置Ubuntu的默认进入用户

1. 以 root 或具有管理员权限的用户身份登录 Ubuntu。

2. 打开终端并执行以下命令以编辑 `/etc/gdm3/custom.conf` 文件：

   ```
   sudo nano /etc/gdm3/custom.conf
   ```

3. 找到以下行：

   ```
   #  AutomaticLoginEnable = true
   #  AutomaticLogin = user1
   ```

4. 将 `AutomaticLoginEnable` 行取消注释（删除 `#` 符号）并设置其值为 `true`：

   ```
   AutomaticLoginEnable = true
   ```

5. 将 `AutomaticLogin` 行取消注释并将其值设置为要作为默认用户登录的用户名：

   ```
   AutomaticLogin = username
   ```

   其中，`username` 是要作为默认用户登录的用户名。

6. 保存并关闭文件。

7. 重启 Ubuntu。

现在，当启动 Ubuntu 时，系统会自动登录到指定的用户账户中。注意，自动登录可能会降低系统安全性，因为任何人都可以访问该用户账户。因此，建议仅在安全环境下使用自动登录功能。

### 可以在这里默认root登陆吗

虽然在某些情况下需要以 root 用户身份登录系统，但通常不建议将 root 用户设置为默认登录用户。这是因为 root 用户拥有系统中的最高权限，如果默认以 root 用户身份登录系统，会增加系统被攻击的风险。

如果仍然希望将 root 用户设置为默认登录用户，可以按照上述步骤编辑 `/etc/gdm3/custom.conf` 文件，并将 `AutomaticLogin` 行的值设置为 `root`，如下所示：

```
AutomaticLoginEnable = true
AutomaticLogin = root
```

注意，这样设置可能会降低系统安全性，因此建议仅在必要的情况下使用。

## 为什么默认root登录会降低系统安全性, 并增加系统被攻击的风险

将 root 用户设置为默认登录用户会降低系统安全性的原因如下：

1. 以 root 用户身份登录系统会暴露系统的最高权限。如果系统被攻击者入侵，攻击者将可以获得完全控制系统的权限，这将对系统的安全性和机密性产生极大的威胁。
2. root 用户在使用时需要特别小心。由于 root 用户拥有系统中的最高权限，因此在使用时需要特别小心，以避免意外的损坏或删除系统文件等操作。
3. root 用户往往不受限制。一些安全措施可能无法限制 root 用户的操作。例如，如果普通用户被禁止执行某些操作，root 用户仍然可以执行这些操作。
4. root 用户可能被其他攻击者滥用。如果系统中存在其他漏洞或安全漏洞，攻击者可以使用这些漏洞以 root 用户身份入侵系统。

以下是一个实际案例：在 2019 年，一个名为“Sudo”的漏洞在许多 Linux 系统中被发现，这个漏洞可以允许攻击者以 root 用户身份入侵系统。该漏洞被利用来攻击多个 Linux 系统，包括一些重要的公共服务，例如网络服务器和数据库。这一事件表明，即使是 Linux 等安全性较高的操作系统，也不免存在安全漏洞和被攻击的风险。因此，我们建议仅在必要时以 root 用户身份登录系统，并遵循最佳安全实践以保护系统。

## 如果以root登录, 这时候正好被攻击了, 那么攻击者就会以root登录状态入侵吗

如果攻击者成功入侵系统并获得 root 权限，他们将可以以 root 用户身份执行任何操作，包括获取 root 用户的密码或会话令牌。因此，如果已经以 root 用户身份登录并且系统已被入侵，攻击者将可以获得 root 用户的访问权限。

此外，使用 root 用户登录还存在其他风险，例如可能会意外地修改或删除系统文件，或者在未经授权的情况下获取敏感信息。因此，我们建议仅在必要时使用 root 用户登录系统，并尽可能使用普通用户来执行日常任务。同时，确保系统的安全性和更新操作系统和应用程序等软件补丁，以减少系统被攻击的风险。

## 以单台机器运行hadoop伪分布模式的系列命令

- 配置 Hadoop：

```
sudo nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh
```

找到下面的行：

```
# export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

将其修改为：

```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

保存并关闭文件。

- 启动 Hadoop：

```
/usr/local/hadoop/sbin/start-dfs.sh
/usr/local/hadoop/sbin/start-yarn.sh
```

- 确认 Hadoop 是否正常运行：

访问 http://localhost:50070/ ，确认 NameNode 是否正常运行。

访问 http://localhost:8088/ ，确认 ResourceManager 是否正常运行。

- 停止 Hadoop：

```
/usr/local/hadoop/sbin/stop-dfs.sh
/usr/local/hadoop/sbin/stop-yarn.sh
```

这些命令将启动 Hadoop 的分布式文件系统（HDFS）和资源管理器（YARN）。可以通过访问 Hadoop Web 界面来确认它们是否正常运行。停止 Hadoop 时，使用 stop-dfs.sh 和 stop-yarn.sh 命令。

### 在以hadoop用户运行start-dfs.sh时输入密码并显示信息

`localhost: starting namenode, logging to /usr/local/hadoop/logs/hadoop-hadoop-namenode-evinci-virtual-machine.out`

是正常的启动消息，表明 Hadoop NameNode 正在启动并将日志记录到指定的日志文件 `/usr/local/hadoop/logs/hadoop-hadoop-namenode-evinci-virtual-machine.out` 中。

可以在另一个终端窗口中使用以下命令来查看 NameNode 进程是否正在运行：

```
jps
```

如果 NameNode 进程正在运行，则会显示类似以下内容的输出：

```
code1528 DataNode
1702 Jps
1284 NameNode
1654 ResourceManager
1384 SecondaryNameNode
```

## ResourceManager是通过yarn启动的

`ResourceManager` 是 Hadoop 集群中的一个重要组件，它负责管理集群资源，例如内存、CPU、磁盘等，并协调和监控集群中运行的所有应用程序。

当一个应用程序需要在集群中运行时，它会向 `ResourceManager` 提交一个作业请求，并分配所需的资源。

在 Hadoop 集群中，有两个主要的资源管理器：`ResourceManager` 和 `NodeManager`。

`ResourceManager` 运行在主节点上，而 `NodeManager` 运行在每个工作节点上。

`NodeManager` 负责在每个工作节点上启动和监控容器（container），并负责向 `ResourceManager` 报告节点资源使用情况。

`ResourceManager` 是通过 Hadoop 的资源管理框架 YARN 来启动的。

在 YARN 中，`ResourceManager` 负责协调和管理集群中的资源分配和任务调度，而 `NodeManager` 则负责监控和管理各个节点上的任务执行情况。

YARN 通过**将计算资源和存储资源分离**来提高 Hadoop 集群的灵活性和可伸缩性。

**计算资源**由 YARN 的资源管理器（`ResourceManager`）进行管理; **存储资源**则由 HDFS 的名称节点和数据节点进行管理。这种分离使得 Hadoop 集群能够更加高效地利用计算资源，并支持更广泛的应用场景。