# node.Js 安装

[toc]

## 前言

本人准备安装dubbo-admin，一个vue+springboot的项目，需要使用node.js环境，因此制作了本篇node.js安装教程。

## 版本说明

* Cent OS 7.9
* node.js 14.17.3 (2021.7.27 最新版本)

## 获取资源

```shell
wget -P /home/download  https://nodejs.org/dist/v14.17.3/node-v14.17.3-linux-x64.tar.gz
```

注：下载资源比较慢，作者是用主系统下载，然后上传到服务器的。

## 安装步骤

1. 进入下载目录 解压nodejs的tar包

   ```shell
   cd /home/download
   tar -zxvf node-v14.17.3-linux-x64.tar.gz
   ```

2. 进入解压完成的nodejs的bin目录，测试是否可用，出现v14.17.3即为成功。

   ```shell
   # 进入bin目录
   cd node-v14.17.3-linux-x64/bin
   # 查看node版本
   ./node -v
   ```

3. 接下来，将nodejs复制到/usr/local/nodejs目录下，并做软链使命令全局可用。

   ```shell
   # 进入软件下载根目录
   cd /home/download
   # 将node复制到/usr/local/nodejs文件夹下
   cp -a /home/download/node-v14.17.3-linux-x64 /usr/local/nodejs
   # 制作软链使命令全局可用（node的bin目录下共包含三个命令 node npm npx）
   ln -s /usr/local/nodejs/bin/node /usr/bin/node
   ln -s /usr/local/nodejs/bin/npm /usr/bin/npm
   ln -s /usr/local/nodejs/bin/npx /usr/bin/npx
   ```

4. 完成，接下来可以在任意目录下测试`node -v`指令，显示版本信息即说明全局配置成功。





