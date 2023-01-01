# 配置多邮箱 rsa 密钥多git平台



[toc]



## 前言

入职公司给我了一个工作邮箱，使用的gitlab平台为自建，需使用工作邮箱创建账号登录。

我之前使用github、gitee、gitea等其他平台使用的都是私人邮箱，不想改掉rsa-key。



## 解决方法

1. 在`~/.ssh/`目录下 使用`rsa-keygen`命令生成新添加的rsa-key。

   ```shell
   ssh-keygen -t rsa -C "Email@163.com" -f your_rsa_primary_key
   ```

   >生成ssh密钥 type: rsa  -C：添加注释	file_name: your_rsa_primary_key

2. 在`~/.ssh/`目录下创建config文件。

   ```shell
   touch config
   ```

   > 配置文件 无需文件后缀

3. 编写config文件内配置。

   ```
   # github
   Host github.com
       HostName github.com
       Port 22
       User git
       PreferredAuthentications publickey
       IdentityFile ~/.ssh/id_rsa_github
   
   # gitee 
   Host gitee.com
       HostName gitee.com
       Port 22
       User git
       PreferredAuthentications publickey
       IdentityFile ~/.ssh/id_rsa_gitee
   ```

   > 根据不同域名采用不同的 私钥文件
   >
   > 建议host和hostname一致