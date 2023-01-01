# OpenResty 安装教程(CentOS)

[toc]

## 前言

入职培训期间，导师教学Nginx安装，推荐了openResty这款软件，是基于Nginx开发的。在外部进行了一次封装，实现了Nginx针对于Lua语言插件的拓展性。特此制作本篇openResty安装教程。

## 版本信息

OS Version： CentOS 7

## 在线安装

本次安装使用root用户进行操作，由于nginx设计机制。因此，无需担心使用root权限启动nginx服务会导致的安全问题。

```shell
# 1.安装yum工具
yum install -y yum-utils
# 2.添加yum仓库
yum-config-manager --add-repo https://openresty.org/package/centos/openresty.repo
# 3.安装openresty
yum install -y openresty
# 4.安装openresty工具
yum install -y openresty-resty
# 5.验证是否安装成功 查看版本
openresty -v
# 6.启动nginx
openresty
# or
/usr/local/openresty/nginx/sbin/nginx
# 7.关闭nginx
/usr/local/openresty/nginx/sbin/nginx -s stop
# or
openresty -s stop
```

## 离线安装

待补充...