# aarch64架构下docker安装mysql

[toc]

## 前言

本文档记录使用docker容器建立mysql服务

适用于aarch64架构的服务器，M1/M2芯片的Macbook。

镜像地址：https://hub.docker.com/r/mysql/mysql-server



## 步骤

1. 拉取镜像，运行容器：

   ```bash
   docker pull mysql/mysql-server:8.0-aarch64
   docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=mysql3306 --name mysql mysql/mysql-server:8.0-aarch64
   ```

2. 先进入容器：

   ```bash
   #进入容器内部，并且输入密码
   docker exec -it mysqldemo mysql -u root -p
   ```

3. 修改数据库可连接IP

   ```sql
   use mysql
   select 'host' from user where user='root';
   update user set host = '%' where user ='root';
   flush privileges;
   ```

4. 可以使用工具连接了！

## 问题

1. **为什么不使用mysql/mysql镜像？**

   mysql/mysql镜像没有aarch架构的镜像包。

2. **出现1130 主机不允许连接到mysql服务。**

   需要使用3步骤 修改mysql允许连接的主机IP

3. **启动mysql容器时，不想将密码显示在命令行 或 忘记修改密码了怎么办？**

   mysql若未设置环境变量`MYSQL_ROOT_PASSWORD`，则服务启动时会生成随机密码，可以在日志中查看，具体查看命令如下：

   ```shell
   docker logs mysql 2>&1 | grep GENERATED
   ```

4. **我本地没有安装mysql的连接工具，如何连接到mysql容器？**

   可以使用以下命令，使用容器内的mysql连接工具：

   ```bash
   docker exec -it mysql mysql -uroot -p
   ```

   

