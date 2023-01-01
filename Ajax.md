# Ajax

[toc]

## 概念

AJAX（Asynchronous JavaScript And XML），异步的JavaScript 和 XML。

Ajax是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。

通过在后台与服务器进行少量数据交换，Ajax可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

传统的网页（不使用Ajax）如果需要更新内容，必须重载真个网页页面。

主要意义：提升用户体验。

* 异步和同步：客户端和服务器端相互通信的基础上
  * 同步：客户端必须等待服务器的响应。在等待期间客户端不能做其他操作。
  * 异步：客户端不需要等待服务器端的响应。在服务器处理请求的过程中，客户端可以进行其他操作。

## 实现方式

1. 原生JS实现方式 (了解)

   ```javascript
   // 发送异步请求
   // 创建核心对象
   var xmlhttp;
   if(window.XMLHttpRequest){ // code for IE7+, Firefox, Opera, Safari
       xmlhttp = new XMLHttpRequest();
   } else { // code for IE6, IE5
       xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
   }
   
   // 建立连接
   // 参数：
   // 1. 请求方式 GET | POST
   //   * get方式：请求参数在URL后面拼接。send方法为空参
   //   * post方式：请求参数在send方法定义
   // 2. 请求的URL
   // 3. 同步或异步请求： true(异步) | false(同步)
   xmlhttp.open("GET", "ajaxServlet?username=tom", true);
   
   // 发送请求
   xmlhttp.send();
   
   // 接受并处理来自服务器的响应结果/
   // 获取方式：xmlhttp.responseText
   // 什么时候获取？ 当服务器响应成功
   xmlhttp.onreadystatechange=function () {
       if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
           var responseText = xmlhttp.responseText;
           alert(responseText);
       }
   }
   ```

2. JQuery实现方式

   1. $.ajax()

      * 语法：`$.ajax({键值对});`

      * 案例

        ```javascript
        $.ajax({
            url: "ajaxServlet", //请求路径
            type: "POST",
            data: {
                "username":"jack",
                "age": 23
            },
            success:function (data) {
                alert(data);
            },
            error: function () {
                alert("出错了...");
            },
            dataType: Text
        });
        ```

   2. $.get() ： 发送GET请求

      * 语法：`$.get(url, [data], [callback], [type])`

      * 案例

        ```javascript
        $.get("ajaxServlet", {"username":"rose"}, function (data) {
            alert(data)
        }, "text");
        ```

   3. $post() ：发送POST请求

      * 语法：`$.get(url, [data], [callback], [type])`

      * 案例

        ```javascript
        $.post("ajaxServlet", {"username":"amy"}, function (data) {
            alert(data)
        }, "text");
        ```