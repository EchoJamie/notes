# ZooKeeper 安装

[toc]

## 前言

本人正在学习Dubbo，在Dubbo中有组件叫注册中心，官网推荐使用ZooKeeper进行实现。所以，有了这次的ZooKeeper安装教程。

## 版本说明

* Cent OS 7.9
* ZooKeeper 3.7.0 (2021.7.26 最新版本)

## 获取资源

获取ZooKeeper3.7.0的安装包

1. 从apache官网获取 获取速度相对较慢   点击[此处](https://archive.apache.org/dist/zookeeper/)查看Apache官方各版本zookeeper

   ```shell
   wget -P /home/download https://archive.apache.org/dist/zookeeper/zookeeper-3.7.0/apache-zookeeper-3.7.0-bin.tar.gz
   ```

   

2. 从aliyun镜像服务器中获取，不过aliyun镜像中的版本不是很全，一般只包含官方提供的 Top 3 版本，因为作者所安装的是最新版本，所以可以镜像获取。   点击[此处](https://mirrors.aliyun.com/apache/zookeeper/)查看aliyun的镜像文件

   ```shell
   wget -P /home/download https://mirrors.aliyun.com/apache/zookeeper/zookeeper-3.7.0/apache-zookeeper-3.7.0-bin.tar.gz
   ```

## 安装过程

作者下载的ZooKeeper是直接可用的，因此只需要解压即可。

```shell
mkdir /opt/zookeeper
tar -zxvf /home/download/apache-zookeeper-3.7.0-bin.tar.gz -C /opt/zookeeper/
```

执行完上述命令，zookeeper就已经安装在`/opt/zookeeper/`文件夹了。

## 修改配置文件

通常我们会修改ZooKeeper的数据存储位置

```shell
cd /opt/zookeeper/apache-zookeeper-3.7.0-bin/
mkdir zkdata
cd conf
cp zoo_sample.cfg zoo.cfg
vi zoo.cfg
```

执行完上述命令后，我们需要修改文件中的dataDir项

![zoo.cfg](..\imgs/ZooKeeper/zoo.cfg.png)

## 启动和关闭

```shell
# 启动zookeeper
/opt/zookeeper/apache-zookeeper-3.7.0-bin/bin/zkServer.sh start
# 查看zookeeper状态
/opt/zookeeper/apache-zookeeper-3.7.0-bin/bin/zkServer.sh status
# 关闭zookeeper
/opt/zookeeper/apache-zookeeper-3.7.0-bin/bin/zkServer.sh stop
```

## 开启防火墙端口

```shell
# 开启相应端口
sudo firewall-cmd --zone=public --add-port=2181/tcp --permanent
```

