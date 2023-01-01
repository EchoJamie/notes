# Git常用命令

[toc]

---

## 第一次使用Git

* 设置全局用户名：

  ``git config --global user.name "username"``

* 设置全局邮箱：

  ``git config --global user.email "emailname@163.com"``
  

## 本地仓库

* 本地仓库初始化：

  ``git init``

* 查看当前仓库状态：

  ``git status``

* 将文件提交到缓存：

  ``git add filename1.md filename2.txt``

  ``git add .``

* 上传缓存：

  ``git commit -m "your comment"``

## 版本回退

* 查询历史提交版本：

  ``git log``

  ``git log --pretty=oneline``

* 回滚版本：

  ``git reset --hard VersionNum 	//VersionNum至少写4位``

  ``git reset --hard head^ 			//head^指head的上一个版本 ``

* 查看历史操作：

  ``git reflog``

## 线上仓库

* 本地配置
  * 如果是Http协议，无需任何可跳过此步操作
  * 如果是ssh协议，则需要先进行下列操作
    1. 安装openSSH
    2. 创建公私密钥对：``ssh-keygen -t rsa -C "Email@163.com"``
    3. 在Github中配置公钥

* 克隆线上仓库到本地：

  ``git clone RepositoryURL		//Example: http://github.com/UserName/RepoName.git  git@ipAddr:/name.git``

* 连接线上仓库：

  ``git remote add origin RepositoryURL ``

* 提交缓存：

  ``git push origin master`` 

* 拉取线上仓库：

  ``git pull``

## 分支控制

* 查看分支

  ``git branch``

* 创建分支

  ``git branch 分支名 ``

* 切换分支

  ``git checkout 分支名``

* 删除分支

  ``git branch -d 分支名``

* 合并分支

  ``git merge 被合并的分支名``

* 创建并切换分支

  ``git checkout -b 分支名``

## 过滤文件

在git中创建.gitignore文件 **Windows中不允许创建无文件名的文件**

过滤规则：

1. ``/DirectoryName/``	过滤该文件夹
2. ``*.zip`` 利用通配符过滤某类文件
3. ``FileName.suffix`` 过滤单个文件
4. ``!FileName.suffix`` 不过滤某个文件
5. ``# This is a Comment`` 过滤文件中的注释行