# Tomcat安装教程(CentOS)

[toc]



## 前言

入职培训期间，导师要求在虚拟机中安装Tomcat，特此制作出本篇Tomcat安装配置教程。

本文中路径对应关系可能与读者的安装路径不同，读者须自行对应自己的路径。

## 版本说明

* OS Version：Cent OS 7.9 
* Tomcat Version：tomcat 8.5.81 (2022.06.28 最新版本)
* JDK Version： jdk8

> 1. Tomcat支持Servlet、JSP、EL表达式、WebSocket、JASPIC等多种特性，这些特性具有多种版本，同时，Tomcat版本需要与Java版本对应 。详细对应关系可见[Tomcat官方版本对照表](https://tomcat.apache.org/whichversion.html)（`https://tomcat.apache.org/whichversion.html`）。

## 获取资源

如果选择centOS 的yum在线安装，可直接跳转到[在线安装部分](###yum在线安装)

Tomcat官方网站：[https://tomcat.apache.org](https://tomcat.apache.org)

8.5版本官方下载地址：[https://tomcat.apache.org/download-80.cgi](https://tomcat.apache.org/download-80.cgi)

## 安装步骤

### 离线安装

1. 检查当前机器内是否安装了jdk，并配置了环境。

   ```shell
   # 查看当前java版本信息
   java -version 
   # 若显示该命令不存在，仅可说明未配置JAVA环境变量。
   ```

   如下所示，可**查看当前已下载但未配置环境的jdk**

   ```shell
   [root@centos7 bin]# which java
   /usr/bin/java
   [root@centos7 bin]# ls -lrt /usr/bin/java
   lrwxrwxrwx. 1 root root 22 Jul 24  2021 /usr/bin/java -> /etc/alternatives/java
   [root@centos7 bin]# ls -lrt /etc/alternatives/java
   lrwxrwxrwx. 1 root root 64 Jul 24  2021 /etc/alternatives/java -> /usr/lib/jvm/OpenJDK8U-jdk_x64_linux_hotspot_8u332b09/bin/java
   ```

   可见，`/usr/lib/jvm/OpenJDK8U-jdk_x64_linux_hotspot_8u332b09` 即为本机的jdk位置。

2. 下载 jdk

   ```shell
   # 获取jdk -P后路径为下载到指定目录
   wget -P /home/download https://mirrors.tuna.tsinghua.edu.cn/Adoptium/8/jdk/x64/linux/OpenJDK8U-jdk_x64_linux_hotspot_8u332b09.tar.gz
   # 解压jdk -C后路径为 解压的所有文件的生成目录
   tar -txvf /home/download/OpenJDK8U-jdk_x64_linux_hotspot_8u332b09.tar.gz -C /path/to/yourJdkPath
   ```

3. 配置JAVA_HOME

   * 配置用户环境变量：

     ```shell
     # 配置用户环境变量
     vi ~/.bash_profile
     # 在该文件尾部添加以下内容
     JAVA_HOME=/path/to/yourJdkPath
     export PATH=$JAVA_HOME/bin:$PATH
     export JAVA_HOME
     ```

   * 配置系统环境变量：

     ```shell
     # 配置系统环境变量
     vi /etc/profile
     # 在该文件尾部添加以下内容
     JAVA_HOME=/path/to/yourJdkPath
     JRE_HOME=$JAVA_HOME
     CLASS_PATH=.:$JRE_HOME/lib
     PATH=$PATH:$JAVA_HOME/bin
     export JAVA_HOME JRE_HOME CLASS_PATH PATH
     ```

   > 当配置用户变量时，其他用户不可使用jdk。
   >
   > 当配置系统变量时，不应该在用户变量中重复配置jdk。

4. 解压Tomcat

   ```shell
   # 获取Tomcat安装包 -P后路径为下载到指定目录
   wget -P /home/download https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.81/bin/apache-tomcat-8.5.81.tar.gz
   # 解压 -C后路径为 解压的所有文件的生成目录
   tar -txvf /home/download/apache-tomcat-8.5.81.tar.gz -C /usr/local/tomcat
   ```

**启动 关闭命令**：

离线安装Tomcat时，启动命令位于Tomcat目录下的bin目录内

启动命令为 `/usr/local/tomcat/startup.sh`

关闭命令为 `/usr/local/tomcat/shutdown.sh` 

### yum在线安装

```shell
# 检查之前是否通过yum安装过tomcat 
# 该命令也用于查询tomcat的运行状态
systemctl status tomcat
# 安装tomcat 会自动安装jdk
yum install -y tomcat
# 安装完成后目录为 /usr/share/tomcat

# 启动tomcat
systemctl start tomcat
# 关闭tomcat
systemctl stop tomcat
```

