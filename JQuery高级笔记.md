# JQuery高级笔记

[toc]

## 动画

三种方式显示和隐藏元素

1. 默认显示和隐藏方式
   * show([speed, [easing], [fn]]) : 显示
   * hide([speed], [easing], [fn]) : 隐藏
   * toggle([speed], [easing], [fn]) : 切换
2. 滑动显示和隐藏方式
   * slideDown([speed], [easing], [fn]) : 显示
   * slideUp([speed], [easing], [fn]) : 隐藏
   * slideToggle([speed], [easing], [fn]) : 切换
3. 淡入淡出显示和隐藏方式
   * fadeIn([speed], [easing], [fn]) : 显示
   * fadeOut([speed], [easing], [fn]) : 隐藏
   * fadeToggle([speed], [easing], [fn]) : 切换

上述显示隐藏函数的参数列表 ：

1. speed 速度 "slow" | "normal" | "fast" | 动画时长的毫秒数值
2. easing 切换效果 默认是"swing" 可用参数为"linear"
   * swing : 动画执行效果：先慢，中间快，最后又慢
   * linear : 动画执行效果：匀速变化
3. fn 动画结束时执行的函数 

## 遍历

1. jq对象.each(callback)

   Ex: 

   ```javascript
   	var citys = $("#city li");
   	citys.each(function (index, element) {
       	 // console.log(this.innerHTML);
            // console.log($(this).html());
           console.log(index + ":" + $(element).html());
           if ("上海" == $(element).html()) {
               // 回调特性
               // 如果当前function返回为false，则结束循环（break）
               // 如果当前function返回为true，则进入下次循环（continue）
               return false;
           }
       });
   ```

2. $.each(object, [callback])

   Ex:

   ```javascript
   $.each(citys, function () {
   	// 使用方法与1一致
   })
   ```

3. for..of : 仅限于JQuery3.0以上版本

   Ex:

   ```javascript
   for (li of citys) {
       console.log(li);
       console.log($(li).html());
   }
   ```

## 事件绑定

1. JQuery标准的绑定方式

   * `jq对象.事件方法(回调函数);`   注：可以使用链式编程

2. on绑定事件/off接触绑定

   * `jq对象.on("事件名称", 回调函数);`
   * `jq对象.off("事件名称");`  注：无参时，将组件上的全部事件解绑

   Ex:

   ```javascript
   $("#btn").on("click", function () {
       alert("我被点击了...");
   });
   $("#btn2").click(function () {
       // $("#btn").off("click");
       $("#btn").off(); // 将组件上的全部事件解绑
   });
   ```

3. 事件切换: toggle

   * `jq对象.toggle(fn1, fn2, ...);` ：在JQuery1.9版本后不可用，如有需要，可使用jquery-migrate插件。



## 案例





## 插件

增强JQuery的功能

1. 实现方式：

   1. $.fn.extend(object) 
      * 增强通过JQuery获取的对象的功能  -->对象插件
   2. $.extend(object)
      * 增强通过JQuery自身的功能  --> 全局插件

2. 案例：

   ```javascript
   // 对象插件
   $.fn.extend({
       check:function () {
           // 让复选框选中
           this.prop("checked", true);
       },
       uncheck:function () {
           this.prop("checked", false);
       }
   });
   
   $(function () {
       $("#btn-check").click(function () {
           $("input[type='checkbox']").check();
       });
       $("#btn-uncheck").click(function () {
           $("input[type='checkbox']").uncheck();
       });
   });
   
   // 全局插件
   $.extend({
       max:function (a, b) {
           return a > b ? a :b;
       },
       min:function (a, b) {
           return a > b ? b : a;
       }
   });
   
   var max = $.max(3, 4);
   alert(max);
   var min = $.min(1, 2);
   alert(min);
   ```

   

