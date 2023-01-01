# SQL要点笔记（MySQL向）

[toc]

## 前言

SQL（Structured Query Language），结构化查询语言。

一种操作关系型数据库的规则，每一种数据库操作比较通用的部分，其中不一致的部分称为“方言”。

作者学习的时候主要面向mysql数据库进行学习，在下文中，详细知识点为MySQL的方言部分都有标注。

## SQL通用语法

1. SQL语句可以单行或多行书写，以分号结尾；
2. 使用空格或缩进来增强语句的可读性；
3. MySQL数据库的SQL语句不区分大小写，关键字建议使用大写。
4. 注释（3种）
   * 单行注释：`-- 注释内容` 或 `# 注释内容`  双横杠后一定要有**空格**，井号后无所谓（但#用法为MySQL特有）
   * 多行注释：`/* 注释内容 */`



### SQL分类

1. DDL（Data Definition Language），数据定义语言。用于定义数据库对象：数据库、表、列等。关键字：create drop alter 等。
2. DML（Data Manipulation Language），数据操作语言。用于数据库种表的数据的增删改。关键字：delete insert update 等。
3. DQL（Data Query Language）数据查询语言。用于查询数据库中表的记录（数据）。关键字：select where 等。
4. DCL（Data Control Language）数据控制语言。用来定义数据库的访问权限和安全级别，及创建用户。关键字：grant revoke 等。



### DDL：操作数据库、表

1. 操作数据库：CRUD

   1. **C**reate：创建
      * `create database 数据库名称;` 创建数据库
      * `create database if not exists 数据库名称;` 如果不存在名称为xx的数据库，则创建xx数据库。
      * `create database 数据库名称 character set 字符集名称;` 使用xx编码创建xx数据库。
   2. **R**etrieve：查询
      * `show databases;` 显示所有数据库名称
      * `show create database mysql;` 查询创建名称为mysql的数据库时，使用的语句/语法。
   3. **U**pdate：修改
      * `alter database 已经存在的数据库的名称 character set 编码名称;` 修改数据库字符集。 
   4. **D**elete：删除
      * `drop database 已经存在的数据库的名称;` 删除数据库。
      * `drop database if exists 已经存在的数据库的名称;` 判断是否存在xx数据库，如果存在，则删除数据库。
   5. 使用数据库（use）
      * `select database();` 查询当前正在使用的数据库。
      * `use 数据库名称;` 使用xx数据库。

2. 操作表：CRUD

   1. **C**reate：创建

      * 创建表：

        ```mysql
        create table 表名(
            列名1 数据类型1,
            列名2 数据类型2,
            ...
            列名n 数据类型n
        );
        ```

      * 复制表：`create table 表名 like 被复制的表名;`

   2. **R**etrieve：查询

      * `show tables;` 查询所有表名；
      * `desc 表名;` 查询表结构
      * `show create table 表名;` 查看创建表的语句/语法。

   3. **U**pdate：修改

      * `alter table 表名 rename to 新的表名;` 修改表名
      * `alter table 表名 character set 字符集;` 修改表的字符集
      * `alter table 表名 add 列名 数据类型;` 添加列
      * `alter table 表名 change 旧列名 新列名 新列类型;` 修改表中的列名称和类型
      * `alter table 表名 modify 旧列名 新列类型;` 修改表中的列的类型
      *  `alter table 表名 drop 列名;` 删除列

   4. **D**elete：删除

      * `drop table 表名;` 删除表
      * `drop table if exists 表名;` 判断是否存在，再删除表。

### DML：增删改表中的数据

1. 添加数据：
   * `insert into 表名(列名1, 列名2, ... , 列名n) values(值1, 值2, ... ,值n);` 向xx表插入一条数据
   * `insert into 表名 values(值1, 值2, ... , 值n);` 插入数据，需要给所有列指定对应值
   * `insert into 表名(列名1, 列名2, ... , 列名n) values(值a1, 值a2, ... ,值an),(值b1, 值b2, ... ,值bn),(值m1, 值m2, ... ,值mn);` 向xx表插入多条数据
2. 修改数据：
   * `update 表名 set 列名1 = 值1, 列名1 = 值1, ... [where 条件表达式] ` 修改条件表达式筛选的记录
3. 删除数据：
   * `delete from 表名 [where 条件表达式]` 删除条件表达式筛选的记录
   * `truncate table 表名;` 删除表，再重新创建一个一模一样的空表。 常用于删除全部数据。

### DQL：查询表中的记录

1. 查询语句模板：

   `select 字段列表 from 表名 where 条件列表 group by 分组字段 having 分组之后的条件 order by 排序 limit 分页限定`

2. 字段列表

   1. **去重** distinct

      用法：`select distinct 字段列表 from 表名`

   2. 数据值运算 （**运算时进行非空判断**） 见下述案例1

      null参与计算的结果都为null

   3. **别名**： 字段 as 别名  见案例1
   
   ````mysql
   # 案例1：
   # 查询姓名，各科成绩及总成绩
   select 
   	name,   					-- 姓名
   	score1, 				    -- 成绩1
       score2, 				    -- 成绩2
       score1 + ifnull(score2, 0) as sum_score   -- 总成绩 如果成绩2为null，则按照0参与计算 
from 
   	表名
   ````
   
3. 条件查询

   1. where子句后接条件

   2. 运算符

      * `>、>=、<、<=、=、<>`  大于，大于等于，小于，小于等于，等于，不等于(mysql中可以使用`!=`)

      * between ... and ...  区间查询  见案例2

      * in(集合) 集合表示多个值，使用逗号分开  见案例3

      * like ‘% _ ’ 用于字符串查询及匹配 见案例4

        通配符：`% _`  %表示0或多字符匹配 _表示单字符匹配

      * is null 判空

      * and 或 &&

      * or 或 ||

      * not 或 !

   ```mysql
   # 案例2
   select * from user where age between 20 and 30;
   # 案例3 
   select * from user where age in(18, 25, 22, 23);
   # 案例4
   select * from user where name like '';
   ```

4. 分组字段 （group by）

   * 语法：`group by 分组字段` 

   * 注意：

     1. 分组之后查询的字段：分组字段、聚合函数。

     2. where和having的区别：

        1. where在分组前进行限定，如果不满足条件，则不参与分组；

           having在分组之后进行限定，如不满足条件则不会被查询出来。

        2. where后不可以进行聚合函数判定；having后可以使用聚合函数判定

   ```mysql
   # 案例5  要求：按照性别分别计算成绩1的平均成绩，仅70分以上的人参与计算，若计算平均成绩的人数少于三个，则无效
   select sex, avg(score1), count(id) from user where score1 > 70 group by sex having count(id) > 2;
   ```

5. 排序查询

   语法：`order by 排序字段1 排序方式1, 排序字段2 排序方式2`

   * asc : 升序    默认排序方式
   * desc : 降序

6. 分页查询

   * 语法：`limit 开始的索引, 每页查询的条数` 

   * 注意：索引从0开始。**limit为mysql的方言。**
   * 公式：开始的索引 = （页码 -1）* 每页查询的条数。

7. 聚合函数：将一列数据作为一个整体，进行纵向的计算。 用于字段列表中。

   * count() : 计数
   * max() : 最大值
   * min() : 最小值
   * sum() : 求和
   * avg() : 平均值

   注意：聚合函数将排除NULL值。
   
8. 多表查询：

   1. 内连接查询

      1. 隐式内连接：使用where条件消除无用数据  详见案例

      2. 显式内连接：

         语法：`select 字段列表 from 表1名 [inner] join 表2名 on 连接条件`

   2. 外连接查询

      1. 左外连接：查询的是左表的全部数据及其交集部分

         语法：`select 字段列表 from 表1名 left [outer] join 表2名 on 连接条件`

      2. 右外连接：查询的是右表的全部数据及其交集部分

         语法：`select 字段列表 from 表1名 right [outer] join 表2名 on 连接条件`

   3. 子查询：查询中嵌套查询，将嵌套的查询称为子查询。

      1. 	子查询结果是单行单列的：可以作为条件，使用普通运算符进行操作/判断等。
      2. 	子查询结果是多行单列的：可以作为条件，使用 in 进行条件判断更方便。
      3. 	子查询结果是多行多列的：可以作为虚拟表，进行表的查询。

   ```mysql
   # 内连接
   # 案例 隐式内连接
   select 
   	t1.name		-- 用户表的用户名
   	t1.gender	-- 用户表的性别
   	t1.gender		-- 角色表的角色名
   from
   	user t1,	-- 用户表
   	role t2		-- 角色表
   where
   	t1.role_id = t2.id;
   # 案例 显式内连接
   select t1.name, t1.gender, t1.gender from user t1 inner join role t2 on t1.role_id = t2.id;
   select t1.name, t1.gender, t1.gender from user t1 join role t2 on t1.role_id = t2.id;
   # 外连接
   # 案例 左外连接
   select t1.name, t1.gender, t1.gender from user t1 left outer join role t2 on t1.role_id = t2.id;
   select t1.name, t1.gender, t1.gender from user t1 left join role t2 on t1.role_id = t2.id;
   # 案例 右外连接
   select t1.name, t1.gender, t1.gender from user t1 right outer join role t2 on t1.role_id = t2.id;
   select t1.name, t1.gender, t1.gender from user t1 right join role t2 on t1.role_id = t2.id;
   # 子查询
   # 案例 子查询
   select * from user where money = (select max(money) from user);
   select * from user where id in (select id from score where math >= 60);
   select * from user left join (select * from role limit 0,2) on role.id = user.role_id;
   ```

### DCL：管理用户、授权

1. 管理用户
   1. 添加用户：`create user '用户名'@'主机名' identified by '密码';`
   2. 删除用户：`drop user '用户名'@'主机名';`
   3. 修改用户密码：
      1. `update user set password = password('新密码') where user = '用户名';`
      2. `set password for '用户名'@'主机名' = password(新密码);`
   4. 查询用户：`select * from mysql.user;`
2. 授权
   1. 查询权限：`show grants for '用户名'@'主机名';`
   2. 授予权限：`grant 权限列表 on 数据库.表 to '用户名'@'主机名';`
   3. 撤销权限：`revoke 权限列表 on 数据库.表 from '用户名'@'主机名';`

## 约束

* 概念：对表中数据进行限定，保证数据的正确性、有效性和完整性。

* 分类：

  1. 主键约束：primary key

     描述：非空且唯一；一张表只能有一个主键；主键即为表中记录的唯一标识；

  2. 非空约束：not null

     描述：该列值不可为空

  3. 唯一约束：unique

     描述：该列值不可重复 （该列值可以为null，但只能有一个null）

  4. 外键约束：foreign key

     描述：

* 非空约束增删：

  1. 添加非空约束：`alter table 表名 modify 列名 列类型 not null;`
  2. 删除非空约束：`alter table 表名 modify 列名 列类型;` 

* 唯一约束增删：

  1. 添加唯一约束：`alter table 表名 modify 列名 列类型 unique;`
  2. 删除唯一约束：`alter table 表名 drop index 列名;` (删除索引？)

* 主键约束增删：

  1. 添加主键约束：`alter table 表名 modify 列名 列类型 primary key;`
  2. 删除主键约束：`alter table 表名 drop primary key;`

* 自动增长：

  语法：`auto_increment`

  注意：使用在数值型字段上；详细细节特性见文末。

  1. 添加自动增长：`alter table 表名 modify 列名 列类型 auto_increment;`
  2. 删除自动增长：`alter table 表名 modify 列名 列类型;`
  
* 外键约束增删：

  1. 添加外键约束：`alter table 表名 add constraint 外键名 foreign key 外键列名 references 主表名(主表主键名);`
  2. 删除外键约束：`alter table  表名 drop foreign key 外键名;`

* 添加级联操作： （级联更新/删除）

  * 语法：`constraint 外键名 foreign key 外键列名 references 主表名(主表主键名) on [update/delete] cascade;`
  * 分类：
    * 级联更新：`on update cascade`
    * 级联删除：`on delete cascade`

```mysql
# 创建表时添加约束
create table admin(
    id int primary key auto_increment,	-- id     主键约束 自动增长
    name varchar(20) not null,			-- 名字    非空约束
    phone_number varchar(20) unique,	-- 手机号  唯一约束
    role_id varchar(10),				-- 角色id  外键约束
    constraint admin_role_fk foreign key role_id references role(id) on update cascade
);
```

## 数据库的设计

### 多表关系

* 关系分类
  1. 一对一
  2. 一对多（多对一）
  3. 多对多
* 建立关系
  1. 一对多（多对一）
     * 如：部门和员工
     * 实现方式：在“多”的一方建立外键，指向“一”的一方的主键。
  2. 多对多
     * 如：学生和课程
     * 实现方式：建立中间关系表，中间表至少包含两个字段（两表主键）
  3. 一对一
     * 如：学生和身份证
     * 实现方式：可以在任意一方添加外键指向另一方的主键（外键建议添加 unique唯一约束）

### 范式

在设计数据库时，遵循的不同的规范，设计出合理的关系型数据库，这些不同的规范要求被称为不同的**范式**。各种范式呈递次规范，越高的范式数据库冗余越小。**通常我们设计数据库时，实现第三范式即可。**

目前关系型数据库有**六种范式**：第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式（4NF）、第五范式（5NF，又称完美范式）。

* 分类：
  1. 第一范式(1NF) : 每一项都是不可分割的原子数据项。
  2. 第二范式(2NF) : 在1NF的基础上，非码属性必须依赖于候选码。（在1NF基础上消除非主属性对主码的部分函数依赖）
  3. 第三范式(3NF) : 在2NF的基础上，任何非主属性不依赖于其他非主属性。（在2NF基础上消除传递依赖）
  4. 巴斯-科德范式(BCNF) : 在3NF基础上，任何非主属性不能对主键子集依赖（在3NF基础上消除对主码子集的依赖）

## 数据库的备份和还原

1. 命令行

   * 备份

     `mysqldump -u用户名 -p密码 数据库名称 > 保存的路径/xxx.sql`

   * 还原
     1. `create database 数据库名;`
     2. `use 数据库名;`
     3. `source 数据库备份文件路径/xxx.sql`

2. 图形化工具：转储为sql文件

## 事务管理

1. 事务的基本介绍

   * 概念：如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败。
   * 操作：
     1. 开启事务：`start transaction;`
     2. 回滚：`rollback;`
     3. 提交：`commit;`
     4. 修改事务的默认提交方式：
        1. 查询默认提交方式：`select @@autocommit;`  结果：1 代表自动提交  0 代表手动提交
        2. 修改默认提交方式：`set @@autocommit = 0;`
   * 注意：
     1. MySQL数据库事务默认自动提交；
     2. 事务提交的两种方式：
        * 自动提交：
          * MySQL是默认自动提交的
          * 一条DML语句就会自动提交一次事务
        * 手动提交
          * Oracle是默认手动提交的
          * 需要先开启事务再提交

2. 事务的四大特征

   1. 原子性：是不可分割的最小操作单位。要么同时成功，要么同时失败。
   2. 持久性：当事务提交或回滚后，数据库会持久化的保存数据。
   3. 隔离性：多个事务之间，相互独立。（实际上是会相互影响的，因此需要理解事务的隔离级别）
   4. 一致性：事务操作前后，数据总量不变。

3. 事务的隔离级别**（面试重点）**

   * 概念：多个事务之间是隔离的，相互独立的。但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题了。

   * 存在问题：

     1. 脏读：一个事务，读取到另一个事务中没有提交的数据。
     2. 不可重复读（虚读）：在同一个事务中，两次读取到的数据不一致。
     3. 幻读：一个事务操作（DML）数据表中的所有记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改。

   * 隔离级别：

     1. read uncommited : 读未提交

        产生问题：脏读、不可重复读、幻读。

     2. read commited : 读已提交 （Oracle默认隔离级别）

        产生问题：不可重复读、幻读。

     3. repeatable read : 可重复读 （MySQL默认隔离级别）

        产生问题：幻读。

     4. serializable : 串行化

        可以解决所有问题。

     注意：隔离级别从小到大，安全性越来越高，但是效率越来越低。

   * 数据库隔离级别操作

     * 查询隔离级别：`select @@tx_isolation;`
     * 设置隔离级别：`set global transaction isolation level 级别字符串;`

## 附表

**1. 数据库常用数据类型**

| 数据类型  | 数据类型名称                                                 | 用法               |
| --------- | ------------------------------------------------------------ | ------------------ |
| int       | 整数(长度)                                                   | age int(3),        |
| double    | 小数类型(小数总长度，小数点后长度)                           | score double(5,2), |
| data      | 日期 yyyy-MM-dd                                              |                    |
| datatime  | 日期 + 时分秒   yyyy-MM-dd HH:mm:ss                          |                    |
| timestamp | 时间戳类型   yyyy-MM-dd HH:mm:ss   若未赋值，则默认为当前时间 |                    |
| varchar   | 字符串(最大字符长度<65535)                                   | name varchar(20)   |
| text      | 长文本      最大长度65535字节 （如有需要，可使用更长的长度） |                    |
| binary    | 二进制(长度)     定长字节                                    |                    |
| varbinary | 二进制(长度)     变长字节                                    |                    |

**2. auto_crement 细节详解 **

**3. MySQL忘记root密码 解决方案**

1. 停止MySQL当前服务 （管理员权限）
2. 终端以无验证方式启动MySQL （管理员权限） 命令：`mysqld --skip-grant-tables`
3. 新终端直接连接mysql，修改密码。
4. 重启MySQL服务。