# Filter

[toc]



## 概念

Filter，即过滤器，当用户访问服务器的资源时，过滤器可以将请求拦截下来，完成一些特殊的功能。

过滤器一般用于完成通用的操作，如：验证登录、统一编码处理、敏感字符过滤...

## 快速入门

步骤：

1. 定义一个类，实现`javax.servlet.Filter`接口。
2. 覆写方法
3. 配置拦截路径，两种配置方式：web.xml或注解方法。

代码实现：

```java
@WebFilter("/*") // 配置拦截路径
public class FilterDemo1 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("FilterDemo1初始化成功。。。");
    }
    
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("FilterDemo1被执行了。。。");
        // 放行
        filterChain.doFilter(servletRequest, servletResponse);
    }
    
    @Override
    public void destroy() {
        System.out.println("FilterDemo1销毁成功。。。");

    }
}
```

## 过滤器细节

1. web.xml配置

   ```xml
   <filter>
       <filter-name>demo1</filter-name>
       <filter-class>com.jamie.web.filter.FilterDemo1</filter-class>
   </filter>
   <filter-mapping>
       <filter-name>demo1</filter-name>
       <!-- 拦截路径 -->
       <url-pattern>/*</url-pattern>
   </filter-mapping>
   ```

2. 过滤器执行流程

   1. 执行过滤器
   2. 执行放行后的资源
   3. 回来执行过滤器放行代码下面的代码

3. 过滤器生命周期

   1. init：在服务器启动后就会创建Filter对象，然后执行init方法，用于加载资源。
   2. doFilter：每一次请求被拦截资源时，会执行
   3. destroy：在服务器关闭前，Filter对象被销毁.如果服务器是正常关闭的，则会执行destroy方法

4. 过滤器配置详解

   * 拦截路径配置：

     1. 具体资源路径：/index.jsp  只有访问index.jsp资源时，过滤器才会被执行
     2. 目录拦截： /user/* 访问/user下的所有资源时，过滤器都会被执行
     3. 后缀名拦截： *.jsp 访问所有后缀名为jsp资源时，过滤器都会被执行
     4. 拦截所有资源：/* 访问所有资源时，过滤器都会执行

   * 拦截方式配置：资源被访问的方式

     * 注解配置：

       设置@WebFilter()的dispatcherType属性

       1. REQUEST：默认值，浏览器直接请求资源
       2. FORWARD：转发访问资源
       3. INCLUDE：包含访问资源
       4. ERROR：错误跳转资源
       5. ASYNC：异步访问资源

     * web.xml：

       设置`<dispatcher></dispatcher>`标签即可。

5. 过滤器链（配置多个过滤器）

   * 执行顺序：如果有两个过滤器，过滤器1和过滤器2
     1. 过滤器1过去了
     2. 过滤器2过去了
     3. 资源执行
     4. 过滤器2回来了
     5. 过滤器1回来了
   * 过滤器先后顺序：
     1. 注解配置：按照类名的字符串比较规则比较，值小的先执行
     2. web.xml配置：`<filter-mapping></filter-mapping>`谁定义在前，谁先执行。