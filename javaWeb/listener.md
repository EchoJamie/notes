# Listener

[toc]

## 概念

Listener，监听器

## 事件监听机制

* **事件**：一件事
* **事件源**：事件发生的位置
* **监听器**：一个对象
* **注册监听**：将事件、事件源、监听器绑定在一起。当事件源上发生某个事件后，执行监听器代码。

## ServletContextListener

* 作用：监听ServletContext对象的创建与销毁

  * `void contextDestroyed(ServletContextEvent sce)`：ServletContext对象销毁之前会调用该方法
  * `void contextInitialized(ServletContextEvent sce)`：ServletContext对象创建之后会调用该方法

* 使用步骤：

  1. 定义一个类，实现ServletContextListener接口

  2. 覆写方法

  3. 配置

     1. web.xml

        ```xml
        <listener>
            <listener-class>com.jamie.web.listener.ContextLoaderListener</listener-class>
        </listener>
        ```

     2. 注解配置

        ``@WebListener``

* 这个监听通常用于加载利用ServletContext服务器初始配置

  * 指定初始化配置路径

    ```xml
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/classes/applicationContext.xml</param-value>
    </context-param>
    ```

