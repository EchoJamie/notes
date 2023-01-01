# JSON

[toc]

## 概念

JSON（JavaScript Object Notation），JavaScript对象表示法。

## 特点

1. JSON现在多用于存储和交换文本信息的语法。
2. 进行数据传输。
3. JSON比XML更小、更快、更易解析。

## 语法

1. 基本规则
   1. 数据v在名称/值对中：JSON数据是由键值对构成的。
      * 键用引号（单双都行）引起来，也可以不使用引号
      * 值的取值类型：
        1. 数字：整数或浮点数
        2. 字符串：在双引号中
        3. 逻辑值：true | false
        4. 数组：在方括号中
        5. 对象：在花括号中
        6. null
   2. 数据由逗号分隔：多个键值对由逗号分隔
   3. 花括号保存对象：使用{}定义JSON格式
   4. 方括号保存数组：[ ]
2. 获取数据
   1. JSON对象.键名
   2. JSON对象["键名"]
   3. 数组对象[索引]

## JSON数据和Java对象的相互转换

1. JSON转为Java对象

   1. jackson使用步骤：
      1. 导入jackson的相关Jar包

         * jackson-annotations
         * jackson-core
         * jackson-databind

      2. 创建Jackson核心对象 ObjectMapper

      3. 调用ObjectMapper的相关方法进行转换

         **转换方法**

         1. readValue(json数据字符串, Class类型)

2. Java对象 转换JSON

   1. jackson使用步骤：

      1. 导入jackson的相关Jar包

         * jackson-annotations
         * jackson-core
         * jackson-databind

      2. 创建Jackson核心对象 ObjectMapper

      3. 调用ObjectMapper的相关方法进行转换

         **转换方法**

         1. writeValue(参数, obj)

            参数：

            1. File: 将obj对象转换为JSON字符串，并保存到指定的文件中
            2. Writer: 将obj对象转换为JSON字符串，并将JSON数据填充到字节输出流中
            3. OutputStream: 将obj对象转换为JSON字符串，并将JSON数据填充到字符输出流中

         2. writeValueAsString()：将对象转换为JSON字符串

   2. 注解：

      1. `@JsonIgnore`:排除属性
      2. `@JsonFormat`:属性值的格式化


注：JSON常见解析器：Jsonlib， Gson， fastjson，jackson

**使用时注意**：

1. 服务器响应的数据，在客户端使用时，要相当做json数据格式使用

   1. `$.get(type)` : 将最后一个参数type指定为"json"

   2. 在服务器端设置MIME类型

      ``response.setContentType("application/json;charset=utf-8");``