# Redis

[toc]

## 概念

redis是一款高性能的NoSQL系列的非关系型数据库。

### 什么是NoSQL

NoSQL（NoSQL = Not Only SQL），意为“不仅仅是SQL“，是一项全新的数据库理念，泛指非关系型的数据库。

随着互联网web2.0网站的兴起，传统的关系数据库在处理web2.0网站，特别是超大规模和高并发的SNS类型的web2.0纯动态网站已经显得力不从心，出现了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，特别是大数据应用难题。

### 什么是Redis

Redis是用C语言开发的一个开源的高性能键值对(key-value)数据库，官方提供测的测试数据，50个并发执行100000个请求，读的速度是110000次/s，写的速度是81000次/s，且Redis通过多种键值数据类型来适应不同场景下的存储需求，目前为止Redis支持的键值数据类型如下：

* 字符串类型 string
* 哈希类型 hash
* 列表类型 list
* 集合类型 set
* 有序集合类型 sortedset

Redis的应用场景

* 缓存（数据查询、短连接、新闻内容、商品内容）
* 聊天室的在线好友队列
* 任务队列（秒杀、抢购、12306等）
* 应用排行榜
* 网站访问统计
* 数据过期处理（可以精确到毫秒）
* 分布式集群架构中的Session分离

## 下载安装

官网：https://redis.io

中文网：http://www.redis.net.cn	https://www.redis.com.cn/

1. ubuntu系统

   ```shell
   sudo apt update
   sudo apt full-upgrade
   sudo apt install build-essential tcl		  	//clang环境依赖（redis是clang开发的）
   sudo apt-get install redis-server				//安装Redis服务器
   redis-server														//启动服务器
   redis-cli																 //启动客户端
   // 在客户端程序中输入ping，得到PONG则连接成功
   ```

   配置文件路径：/etc/redis/redis.conf

2. windows系统

   教程链接：https://www.redis.com.cn/redis-installation.html

   服务器启动文件：redis-server.exe

   客户端启动文件：redis-cli.exe

   配置文件：redis.windows.conf

## 命令操作

1. 通用命令
   * `keys *` : 查看所有key
   * `type key` : 查看key的数据类型
   * `del key` : 删除键及对应值
2. 字符串类型操作
   * 存储：`set key value`
   * 获取：`get key`
   * 删除：`del key`
3. 哈希类型操作
   * 存储：`hset key field value`
   * 获取：`hget key field `  |  `hgetall key`
   * 删除：`hdel key field `
4. 列表类型操作
   * 存储：`lpush key value`  |  `rpush key value`  添加并显示数据变化数量
   * 获取：`lrange key start end`  获取所有数据：`lrange key 0 -1`
   * 删除：`lpop key`  |  `rpop key` 移除并返回
5. 集合类型操作
   * 存储：`sadd key value`
   * 获取：`smembers key`
   * 删除：`srem key value`
6. 有序集合类型操作
   * 存储：`zadd key score value`
   * 获取：`zrange key start end`
   * 删除：`srem key value`

### 数据结构(面试重点！！！)

1. redis存储的是：key，value格式的数据，其中key都是字符串，value有5种不同的数据结构：
   * 字符串类型 string
   * 哈希类型 hash : map格式
   * 列表类型 list : LinkedList格式
   * 集合类型 set : HashedSet 不允许重复数据
   * 有序集合类型 sortedset : 自排序不允许重复

## 持久化操作

Redis是个内存数据库，当Redis服务器重启，数据将会丢失。我们可以将Redis内存中的数据持久化保存到硬盘的文件中。

Redis持久化机制：

1. RDB：默认方式，不需要进行配置，默认就是这种机制。

   * 在一点间隔时间中，检测key的变化情况，然后持久化数据。
   * 使用方法：
     1. 编辑配置文件中`save 900 1`类似的代码，其含义为：每隔十五分钟有一次操作就持久化
     2. 重新启动服务器时，使用redis-server启动服务器并在启动命令后指定配置文件

   数据文件名称：`dump.rdb`

2. AOF：日志记录方式，可以记录每一条命令操作。在每次操作后，持久化数据。

   使用方法：

   1. 编辑配置文件中 appendonly属性   `appendonly no(关闭aof)    -->    appendonly yes(开启aof)`

   2. 编辑配置文件中 appendfsync属性

      参数及含义：always --> 每次操作都进行持久化  |  everysec  -->  每隔一秒进行一次持久化  |  no  -->  不进行持久化

   3. 重新启动服务器时，使用redis-server启动服务器并在启动命令后指定配置文件

   数据文件名称：`appendonly.aof`

## Jedis(Java操作Redis数据库)

Jedis是一款Java操作Redis数据库的工具。

### 使用方法

1. 下载Jar包：jedis、commons-pool2(连接池所需Jar)

2. 使用：

   ```java
   @Test
   public void test1() {
       // 获取连接
       Jedis jedis = new Jedis("localhost", 6379);
       // 操作
       jedis.set("username", "zhangsan");
       // 关闭连接
       jedis.close();
   }
   ```

3. 操作各种数据结构

   jedis中操作函数与命令名称一致。

### Jedis连接池

使用方法：

1. 创建JedisPool连接池对象
2. 调用方法getResource()方法获取Jedis连接

案例：

```java
public class JedisPoolUtils {
    private static JedisPool jedisPool;
    static {
        InputStream is = JedisPoolUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
        Properties pro = new Properties();
        try {
            pro.load(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
        config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));
        jedisPool = new JedisPool(config, pro.getProperty("host"), Integer.parseInt(pro.getProperty("port")));
    }

    public static Jedis getJedis() {
        return jedisPool.getResource();
    }
}
```
