# JSP详解

[toc]

## 概念

JSP即***Java Server Pages***的缩写，从语义上可直接解读为**Java服务器端页面**。我们可以将其理解为一个特殊的页面，其中既可以指定定义html标签，也可以使用Java代码，主要是用于简化我们的代码书写。

## 原理

**JSP本质上是一个Servlet**。当我们部署我们的web项目时，会在我们的Web服务器工作目录中的work文件夹中生成与我们自己写的jsp文件相对应的.java文件和.class文件。用Tomcat服务器的默认主页index.jsp文件来举例，我们可以在上述操作后看到命名为index_jsp.java的文件，在编辑器中打开我们可以看到文件内的代码头部如下图所示：

![index_jsp.java](imgs\index_jsp.png)

那么，他继承了HttpJspBase类，这个类存在于Apache公司的Tomcat源码中。追查其源码我们可以发现，源码中HttpJspBase类如下图所示：

![HttpJspBase.java](imgs\HttpJspBase.png)

由此我们上面的说法——**JSP本质上是一个Servlet**已经得到了验证。

接下来我们继续看index_jsp.java文件，由于这是一个Servlet，所以我们可以去看看Servlet的Service方法。在文件中搜索`Service`我们只发现了`public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response) throws java.io.IOException, javax.servlet.ServletException`这样一个方法，在方法中我们可以看见如下所示的类似代码：

```java
out.write("\n");
out.write("\n");
out.write("<html>\n");
out.write("  <head>\n");
out.write("    <title>$Title$</title>\n");
out.write("  </head>\n");
out.write("  <body>\n");
out.write("  $END$\n");
out.write("  </body>\n");
out.write("</html>\n");
```

结合我们已经学过的servlet中有关于response(响应)的知识，输出流响应可以将我们手写的一部分html代码传递给浏览器客户端，这个思路符合JSP页面在在实际打开时的反馈。由此可见JSP确实**简化了我们的代码书写**。

## JSP脚本

脚本，即JSP定义Java代码的方式，共有三种如下：

1. `<% 代码 %>` 

   定义的Java代码在service方法中执行。  

2. `<%! 代码 %>`

   定义的Java代码在JSP转换后，会成为Java类的成员位置。

3. `<%= 代码 %>`

   定义的Java代码会输出到页面上，可以等同于`<% out.write(代码); %>`，简化了我们输出代码。 

## JSP指令

**作用**：用于配置jsp页面，导入资源文件

**格式**：

`<%@ 指令名称 属性名1=属性值1 属性名2=属性值2 ... %>`

**分类**：

1. page：配置JSP页面的

   * `contentType`：等同于response.setContentType()，用于设置响应体的mime类型以及字符集
   * `pageEncoding`：设置jsp文件的编码
   * `language`：设置语言，最初想要支持多平台语言开发，不过目前(2021.05.16)仍仅支持Java。
   * `buffer`：设置缓冲区的大小，默认是8kb。out的缓冲区
   * `import`：导入jsp内所写的Java程序需要导入的包。
   * `errorPage`：错误页面，当前页面发生异常后，会自动跳转到指定的错误页面
   * `isErrorPage`：标识当前页面是否是错误页面。如果设定为true，将可以使用内置对象exception。
   * `isELIgnored`：标识当前页面是否忽略EL表达式，如果设定为ture，将不解析EL表达式。

2. include：页面包含	导入页面的资源文件

   `<%@ include file="navigate.jsp"%>` 导入导航栏元素

3. taglib：导入(标签库)资源

   ``<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>`

   prefix：前缀，自定义的

## JSP注释

1. html注释

   `<!-- -->` 只能注释html，html注释最终会在html页面(客户端页面源码中)显示出来

2. jsp注释

   `<%-- --%>` 可以注释所有，jsp注释不会在客户端页面源码显示

## JSP内置对象

在JSP中不需要定义即可直接使用的对象（可以结合JavaScript内置对象的概念进行理解）。JSP共有9个内置对象。

| 变量名      | 真实类型            | 作用                                    |
| ----------- | ------------------- | --------------------------------------- |
| pageContext | pageContext         | 当前页面共享数据，还可以获取其他8个元素 |
| request     | HttpServletRequest  | 一次请求访问的多个资源（转发）          |
| session     | HttpSession         | 一次会话的多个请求间                    |
| application | ServletContext      | 所有用户间共享数据                      |
| response    | HttpServletResponse | 响应对象                                |
| page        | Object              | 当前页面(Servlet)的对象，this           |
| out         | JspWriter           | 输出流对象，数据输出到页面上            |
| config      | ServletConfig       | Servlet的配置对象                       |
| exception   | Throwable           | 异常对象                                |

注：

1. 前四个可视为域对象，

2. out：字符输出流对象，可以将数据输出到页面上。*response.getWriter().write()*

   **注意**：response.getWriter().write()与out.print()都可以将数据输出到页面上，它们的区别在于，无论response.getWriter().write()写在什么位置，都会在头部输出，而out.print()则是按照所写的顺序输出。

## EL表达式

* **概念**：EL表达式（Expression Language，表达式语言）

* **作用**：主要用来替换和简化Jsp页面中Java代码的编写。

* **语法**：`${表达式}`

* **注意**：

  1. Jsp默认支持EL表达式
  2. 不解析EL表达式的方式
     * 配置`isELIgnored`属性，将不解析整个Jsp页面的所有EL表达式。
     * 不解析单个EL表达式的方法：`\${表达式}`

* **使用**：

  * 运算：

    1. 算术运算符：+  -  *  /(div)  %(mod)

    2. 比较运算符：>  >=  <  <=  ==  !=

    3. 逻辑运算符：&&(and)  ||(or)  !(not)

    4. 空运算符：empty

       用于判断字符串、集合、数组对象是否为null，或长度为0。

       `${empty list}`

  * 获取值：

    EL表达式只能从域对象中获取值

    语法：

    1. `${域名称.键名}`：从指定域中获取指定键的值

       | 域名称           | 对应域                      |
       | ---------------- | --------------------------- |
       | pageScope        | pageContext                 |
       | requestScope     | request                     |
       | sessionScope     | session                     |
       | applicationScope | application(ServletContext) |

    2. `${键名}`：表示依次从最小的域中查找是否有该键对应的值，直到找到为止。

    3. 获取对象、List集合、Map集合的值

       * 对象：`${(域名称.)键名.属性名}`  --->实质上是调用对象的getter方法。
       * List集合：`${(域名称.)键名[索引]}`
       * Map集合：`${(域名称.)键名.key名称}`   或   `${(域名称.)键名["key名称"]}`

  * 隐式对象：

    EL表达式中有11个隐式对象（与内置对象类似）

    1. pageContext：可以获取Jsp其他8个内置对象---**重点掌握**：`${pageContext.request.contextPath}` --> 动态获取虚拟目录

## JSTL

* **概念**：JSTL(JavaServer Page Standard Tag Library，Java服务器页面标准标签库)由Apache组织提供的开源的免费的Jsp标签。

* **作用**：用于简化和替换Jsp页面上的Java代码。

* **使用步骤**：

  1. 导入JSTL相关Jar包
  2. 引入标签库：taglib指令
  3. 使用标签

* **常用标签**：

  1. if：相当于Java代码的if语句

     属性：test，必须属性，用于接收boolean表达式。一般情况下，与EL表达式一起使用

     如果为true，则显示标签体内的内容；如果为false，则不显示标签体内的内容。

     前提：需要导入标签库`<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> `

     格式：`<c:if test="true">标签体内容</c:if>`

  2. choose：相当于Java代码的switch语句

     格式：

     ```jsp
     <c:choose>
         <c:when test="true">标签体内容</c:when>
         <c:when test="false">标签体内容</c:when>
         <c:otherwise>相当于Java中的default</c:otherwise>
     </c:choose>
     ```

     

  3. forEach：相当于Java代码的for语句

     * 完成重复性操作

       属性：

       1. begin：var的开始值
       2. end：var的结束值（var可==end）
       3. var：临时变量
       4. step：步长值
       5. varStatus：循环状态对象
          * index：下标(基本相当于var)
          * count：第n次循环，从1开始

       案例：

       ```jsp
       <c:forEach begin="1" end="10" step="2" var="var" varStatus="vars" >
       	var:${var}  s.index:${vars.index}  s.count:${vars.count} <br />
       </c:forEach>
       ```

     * 遍历容器

       属性：

       1. items：集合容器对象
       2. var：临时变量
       3. varStatus：循环状态对象
          * index：下标，从0开始
          * count：第n次循环，从1开始

       案例：

       ```jsp
       <C:forEach items="${list}" var="item" varStatus="s">
       	index:${s.index} count:${s.count} item:${item} <br />
       </C:forEach>
       ```