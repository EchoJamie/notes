# 会话技术

[toc]

## 何谓会话技术

* **会话**：一次会话中包含多次请求和响应。
  * **一次会话**：浏览器第一次给服务器发送请求，会话建立，直至一方断开为止。
* **功能**：在一次会话的范围内的多次请求间，共享数据
* **方式**：
  1. 客户端会话技术：Cookie
  2. 服务器会话技术：Session
* **两种方式的区别**：
  1. session存储在服务器端，cookie存储在客户端
  2. session没有数据大小限制，cookie有数据大小限制
  3. session数据更安全，cookie相对来书不安全

## Cookie

### 概念

客户端会话技术，将数据保存在客户端。

### 快速入门

1. 创建cookie对象， 绑定数据

   ``new Cookie(String name, String value)``

2. 发送cookie对象

   ``response.addCookie(Cookie cookie)``

3. 获取cookie对象，拿到数据

   ``Cookie[] request.getCookies()``

### 实现原理

基于响应头set-cookie和请求头cookie实现

### cookie的细节

* 一次可不可以发送多个cookie?

  可以，使用response调用多次``addCookie()``即可。

* cookie在浏览器中可以保存多久？

  1. 默认情况，浏览器关闭后，Cookie数据被销毁

  2. 持久化存储：

     ``setMaxAge(int seconds)``

     参数变量取值及意义：

     1. 正数 --- 将Cookie数据写入到硬盘中，持久化存储seconds秒
     2. 负数 --- 默认值，即浏览器关闭后，Cookie数据即被销毁
     3. 零 --- 立即删除Cookie

* cookie可不可以存中文？

  在Tomcat 8 之前，cookie不能直接存储中文数据，需要中文数据转码---一般采用URL编码。

  在Tomcat 8 之后，cookie支持中文数据。

* cookie共享问题？

  * 在同一个Tomcat服务器中，多个Web项目之间，默认不可以共享Cookie数据。

    setPath(String path)方法可以设置cookie的获取范围，默认情况下，设置当前的虚拟目录。

    如果想要共享，可以将path设置为“/”，即Web服务器虚拟目录的根路径。

  * 不同Tomcat服务器间cookie共享方法。

    setDomain(String domain)方法可以设置域名，域名相同时即可多个服务器间共享cookie。

### Cookie的特点和作用

* 特点
  1. cookie存储在客户端浏览器
  2. 浏览器对于单个cookie的大小有限制(一般为4Kb)，对于同一域名下的总cookie数量也有限制(20个)
* 作用
  1. cookie一般用于存储少量不太敏感的数据
  2. 在不登陆的情况下，完成服务器对客户端的身份识别

## Session

### 概念

服务器端会话技术，在一次会话的多次请求间共享数据，将数据保存在服务器端的对象中。HttpSession

### 快速入门

1. 获取Session对象

   ``HttpSession session = request.getSession();``

2. 使用Session对象

   ``Object getAttribute(String name)``

   ``Object setAttribute(String name, Object value)``

   ``Object removeAttribute(String name)``

### 实现原理

Session的实现是依赖于Cookie的。

### Session的细节

* 当客户端关闭后，服务器不关闭，两次获取的session是不是同一个？

  默认情况下，不是同一个；如果需要相同，可以创建Cookie，键为JSESSIONID，值为`session.getId()`并设置最大存活时间，让cookie持久化保存。

* 客户端不关闭，服务器重启后，两次获取的session是不是同一个？

  不是同一个，如果需要确保数据不丢失，有两种方式

  1. session的钝化：

     在服务器正常关闭之前，将session对象序列化到硬盘上

  2. session的活化：

     在服务器重启后，将session对象文件转化为内存中的session对象

* session什么时候被销毁？

  1. 服务器关闭

  2. session对象调用`invalidate()`

  3. session默认失效时间——30分钟

     session生存时间修改方法：web.xml中session-config项的session-timeout配置。

### Session的特点

1. session用于存储一次会话中的多次请求的数据，存在服务器端。
2. session可以存储任意类型，任意大小的数据。