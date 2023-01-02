# Servlet 学习笔记

[toc]

## 概念

Servlet是运行在服务器端的一个小程序(server applet)。

	* Servlet即一个规则(接口)，定义了Java类被浏览器访问(tomcat识别)的规则
	* 后续实现一个类实现Servlet接口。

## 快速入门

1. 创建JavaEE项目

2. 定义一个类实现Servlet接口

   `public class ServletDemo1 implements Servlet{}`

3. 实现接口中的抽象方法

4. **配置Servlet**: 将下列代码添加到web.xml中

   ```xml
   <!-- 配置Servlet -->
   <servlet>
       <servlet-name>demo1</servlet-name>
       <servlet-class>com.jamie.web.servlet.ServletDemo1</servlet-class>
   </servlet>
   <!-- Servlet映射 -->
   <servlet-mapping>
       <servlet-name>demo1</servlet-name>
       <url-pattern>/demo1</url-pattern>
   </servlet-mapping>
   ```

## 执行原理

1. 服务器接收客户端浏览器请求时，解析请求URL路径，获取访问的Servlet的资源路径
2. 查找web.xml文件，是否有对应的\<url-pattern\>标签体内容 
3. 如果有，则找到对应的\<servlet-class\>全类名
4. tomcat会将字节码文件加载进内存，并创建其对象
5. 调用对应方法

## Servlet中的生命周期

1. **被创建**：执行init()方法，仅执行一次

   * Servlet被创建的时间：默认为第一次被访问时，可以通过web.xml配置执行Servlet的创建时间。

   * 配置执行Servlet的创建时机的方法：

     在Servlet标签下创建load-on-startup标签。如果想要设置为第一次被访问时创建，\<load-on-startup\>值为负数；如果想要设置为服务器启动时创建，\<load-on-startup\>值为0或正整数。

   * Servlet的init方法，只执行一次，说明一个Servlet在内存中仅存在一个对象，**Servlet是单例的**。

     如果存在多个用户同时访问，可能存在线程安全问题。因此，我们尽量不在Servlet的继承类中定义成员变量，即使定义成员变量，也不对其进行修改操作。

2. **提供服务**：执行service()方法，可执行多次

   * 每次访问Servlet时，Service()方法都会执行一次

3. **被销毁**：执行destroy()方法，仅执行一次

   * Servlet被销毁时执行。服务器关闭时，Servlet被销毁。
   * 只有服务器正常关闭，才会执行destroy()方法 。**遗言方法，执行完destroy()方法再销毁Servlet**。

## Servlet3.0

* 好处：

  * 支持注解配置，不需要手动书写web.xml。

* 步骤：

  * 创建JavaEE项目，选择Servlet的版本在3.0以上，可以不创建web.xml。

  * 定义一个实现了Servlet接口的类

  * 覆写方法

  * 在类上使用@WebServlet注解进行配置

    标准格式：` @WebServlet(url-pattern="/demo")`  

    精简格式(**常用**)：` @WebServlet("/demo")` 

## Servlet的体系结构

Servlet是一个接口，通常我们在使用Servlet时，仅仅需要调用Service()方法，所以我们可以通过继承Servlet接口的实现类，来减少我们的代码长度。通过查阅API，我们可以知道Servlet接口有两个实现类，分别是GenericServlet和HttpServlet这两个抽象类，而且HttpServlet是GenericServlet的子类。

* GenericServlet将Servlet接口中的其他方法都做了空实现，只将Service()方法作为抽象方法。以后我们在实现Servlet接口时，可以继承GenericServlet，实现service()即可。
* HttpServlet: 对Http协议的一种封装，简化操作
  1. 定义类继承HttpServlet
  2. 覆写doGet/doPost方法

## Servlet相关配置

* url-Pattern: Servlet访问路径

  一个Servlet可以定义多个访问路径：`@WebServlet({"/xxx", "/xxxx/xxx", "/*", "*.do"})`