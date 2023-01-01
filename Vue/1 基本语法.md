# 基本语法

[toc]



## 入门案例

```html
<html>
    <body>
        <div id="app">
            <p>姓名：{{ name }}</p>
            <p>性别：{{ sex }}</p>
        </div>
        <script>
            // 创建Vue对象
            const app = new Vue({
                // 绑定作用域
                el: "#app",
                // 声明数据
                data: {
                    name: "zhangsan",
                    sex: "male"
                }
            });
        </script>
	</body>
</html>
```



## 指令

指令分六大类，主要用于在html标签内使用。基本类似于模板引擎。

### 内容渲染指令

* `v-text`
* `{{  }}` : 插值表达式  内部可以使用JS表达式，但不可使用JS语句
* `v-html`

### 属性绑定指令

* `v-bind:`

  例：`v-bind:src=""`   也可写为 `:src=""`   

### 事件绑定指令

* `v-on:`

  例：`v-on:click="add()"`  也可写为 `@click="add"`

* `$event` : 内置函数，用于事件参数传递

* 事件修饰符：

  * `.prevent` 阻止默认行为
  * `.stop`  阻止冒泡事件
  * `.capture`  以捕获模式触发当前事件处理函数
  * `.once` 绑定的事件仅触发一次
  * `self`  只有在event.target是当前元素自身时触发事件处理函数

  例：`@click.stop="add()"`

* 按键修饰符

  常见的按键事件为`keyup`。比如我们常用的回车键提交，可以使用案件修饰符绑定按键事件。

  * `.enter`
  * `.esc`

  例：`<input @keyup.enter="submit" />`

### 双向绑定指令

* `v-model`
* 指令修饰符
  * `.number`  自动将用户输入的值转化为数值类型
  * `.trim`  自动过滤用户输入的首尾空白字符
  * `.lazy`  在"change"而非"input"时更新

例：`<input v-model.number="num1" />`

### 条件渲染指令

* `v-if`  通过操作DOM树来控制显示与隐藏  可与`v-else`或`v-else-if` 一起使用
* `v-show`  通过修改style中的display属性来显示与隐藏

例：

```html
<body>
    <div id="app">
        <p v-if="grade >= 90">优秀</p>
        <p v-else-if="grade >= 80">良好</p>
        <p v-else-if="grade >= 60">合格</p>
        <p v-else>不合格</p>
        <hr/>
        <p v-show="flag">我出现了</p>
    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                grade: 88，
                flag: true
            }
        });
    </script>
</body>
```

### 循环渲染指令

* `v-for`  循环渲染数据

  可以用于遍历数组、对象

例：

```html
<body>
    <div id="app">
        <ul>
            <li v-for="(value, index) in list">{{ index + 1 }}.{{ value }} </li>
        </ul>
        <hr/>
        <ol>
            <li v-for="(value, key) in Obj"> {{ key }} : {{ value }} </li>
        </ol>
    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                list: ["张三", "李四", "麦克", "小明"],
                Obj: {
                    name: "Jamie",
                    age: 22,
                    gender: "man"
                }
            }
        });
    </script>
</body>
```



## 过滤器（Vue 2.x ）

过滤器功能在Vue 3.x版本中已经被废弃，仅可以在2.x及以下版本使用。

* 过滤器分为私有和全局两种，私有仅能在单个Vue作用域使用，全局可在多个Vue作用域使用。
* 默认将管道符前数据传递给过滤器作为第一个参数，同时使用时也可以传递其他多个参数。
* 过滤器使用方法为使用管道符号"|"。
* 必须使用return返回值。
* 私有过滤器需要声明在Vue构造器内的filters区域。
* 允许全局过滤器与私有过滤器同名，调用时优先使用私有过滤器。
* 可串联使用。

例：

```html
<body>
    <div id="app">
        <!-- 过滤器串联使用 传递参数 -->
        <h1 :title="msg | func(' ^_^') | filterFunc">????</h1>
    </div>
    <script>
        // 全局过滤器
        Vue.filter('filterFunc', (value) => {
            // 字符串反转
            return value.toString().split('').reverse().join('');
        })
        
        const app = new Vue({
            el: "#app",
            data: {
                msg: "hello Vue.js!"
            },
            filters: { // 其内部声明全部为私有过滤器
                // 首字母大写 并拼接字符串
                func(msg, str) {
                    msg = msg.toString();
                    let first = msg.charAt(0).toUpperCase();
                    let other = msg.slice(1);
                    
                    return first + other + str;
                }
            }
        });
    </script>
</body>
```



## 侦听器

侦听器作用为监听数据变化，并根据根据变化对新数据或旧数据进行处理。

* 侦听器侦听的是data内的某一变量或对象。（对象中可以包含多个变量或对象）
* 侦听器实质是一个函数，需要声明在Vue构造器内的watch区域内。
* 默认会传递两个参数，第一个是修改后的值，第二个是修改前的值。
* 侦听器存在两个配置属性，其类型为布尔型。
  * immediate：侦听器是否自动触发一次，默认为false
  * deep：是否深度检测侦听对象，即对象内的各个属性变化，默认为false

例：

```html
<body>
    <div id="app">
        <input type="text" v-model="msg" />
        <p v-text="msg"></p>
    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                info: {
                    msg: "hello Vue.js!"
                }
            },
            watch: {
                // 1 对象写法，可以配置属性
                'info.msg': {
                    // 处理函数
                    handler: function (newVal, oldVal) {
                        console.log(newVal, oldVal);
                    },
                    // 侦听器是否自动触发一次
                    immediate: true,
                    // 是否深度检测侦听对象，即对象内的各个属性变化
                    deep: true
                }
                
                // 2 函数写法，不可配置属性
                'info.msg'(newVal, oldVal) {
            		console.log(newVal, oldVal);
        		}
            }
        });
    </script>
</body>
```



## 计算属性

计算属性，顾名思义即通过一系列计算得到的属性值。实质上也是函数。

我们可以将其理解为一种特殊的属性，它可以根据某种固定的方式对data内数据进行计算，每次data内数据变化时，属性也会发生变化。

例：

```html
<body>
    <div id="app">
        r:<input type="text" v-model="r" /> <br>
        g:<input type="text" v-model="g" /> <br>
        b:<input type="text" v-model="b" /> <br>
        <hr>
        <div style='width: 150px; height: 150px;' :style="{ backgroundColor: rgb }">
            {{ rgb }}
        </div>
    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                r: 0,
                g: 0,
                b: 0
            },
            computed: {
                rgb() {
                    return `rgb(${this.r}, ${this.g}, ${this.b})`;
                }
            }
        });
    </script>
</body>
```

