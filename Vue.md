# 基本结构

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
				
      	<!-- 导包 -->
        <script src="lib/vue.js"></script> 
    </head>

    <body>
      	<!-- 创建容器 -->
        <div id="app">
            <p>{{ msg }}</p>
        </div>

        <script>
          	// 创建 Vue 实例
            var vm = new Vue({
                el: '#app', //指的是 Vue 实例控制的元素区域
                data: { //储存 Vue 实例操作的数据
                    msg: '12345'
                },
              	methods: {
                		//事件方法
              	},
            })
        </script>
    </body>
</html>
```



# 常用指令

插值表达式和v-text 的区别：

- 默认 v-text 指令不会存在闪烁问题 

- 插值表达式可以在标签中加上任意内容而不会被覆盖，即只会替换这个占位符中的内容；v-text 会覆盖元素中原有的内容



v-cloak：解决插值表达式的闪烁问题

v-bind：Vue 的属性绑定机制，缩写为"："

v-on：Vue 的事件绑定机制，缩写为"@"

v-html：将文本解析为 html 格式



```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>

        <script src="lib/vue.js"></script>

        <style>
            [v-cloak]{
                display: none;
            }
        </style>
    </head>

    <body>
        <div id="app">
            <!-- v-cloak是一个指令能够解决插值表达式因加载速度问题而闪烁的问题 -->
            <p v-cloak>{{ msg }}</p> 

            <!-- v-text和插值表达式的作用相似 -->
            <h4 v-text="msg"></h4>

            <!-- 默认 v-text 指令不会存在闪烁问题 -->
            <!-- 插值表达式可以在标签中加上任意内容而不会被覆盖，即只会替换这个占位符中的内容；v-text 会覆盖元素中原有的内容 -->
            <div>_______{{msg}}------------</div>
            <div v-text="msg">+++++++++++++</div>

            <!-- v-html 指令可以将内容解析成 html 格式输出到页面上 -->
            <div v-html="msg2"></div>

            <!-- v-bind 指令是用来提供属性绑定的指令 -->
            <!-- v-bind 指令会将引号中的内容解析为 JS表达式 -->
            <input type="button" value="按钮" title="mytitle">

            <input type="button" value="按钮" v-bind:title="mytitle + '123'">
            <input type="button" value="按钮" :title="mytitle + '123'"> <!-- 简写形式 -->

            <br><br>

            <!-- v-on : 事件绑定机制 -->
            <input type="button" value="按钮" :title="mytitle2" v-on:click="show">
            <input type="button" value="按钮" :title="mytitle2" v-on:mouseover="show">



        </div>

        <script>
            var vm = new Vue({
                el: '#app',
                data: {
                    msg: '12345',
                    msg2: '<h1>suousouosuosusousosuosus</h1>',
                    mytitle: '这是一个自定义的标题',
                    mytitle2: '点击弹框'
                },
                methods: { //这个 methods 属性里定义了当前 Vue 实例所有可用的方法
                    show: function(){
                        alert('hello!')
                    }
                }
            })

            // 使用 DOM 操作
            // document.getElementById("btn").onclick = function(){
            //     alert("hello");
            // }
        </script>
    </body>
</html>
```



## 点击事件案例：跑马灯

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>

        <script src="lib/vue.js"></script>
    </head>

    <body>
        <div id="app">
            <input type="button" value="开启效果" @click="turnOn">
            <input type="button" value="关闭效果" @click="turnOff">
            <input type="button" value="重置效果" @click="reset">
            <h4>{{ msg }}</h4>
        </div>

        <script>
            // 如果需要使用 vm 实例中 data 属性里的数据，或者调用 methods 中的方法，必须通过 this.属性名 或者 this.方法名来访问。
            // 这里的 this 指的就是我们创建出来的 Vue 实例
            var vm = new Vue({
                el: '#app',
                data: {
                    msg: '猥琐发育，别浪!',
                    IntervalId: null //在 data 中赋值 IntervalId 定时器 Id
                },
                methods: {
                    turnOn: function(){
                        // console.log(this.msg)

                        //判断此时是否存在定时器，如果存在则返回
                        if(this.IntervalId != null){
                            return
                        }
                        
                        // var start = this.msg.substring(0,1)
                        // var end = this.msg.substring(1)
                        // this.msg = end + start

                        // // 添加定时器
                        // var _this = this // this 对象不明确，需要复制一份以作区分
                        // setInterval(function(){
                        //     var start = _this.msg.substring(0,1)
                        //     var end = _this.msg.substring(1)
                        //     _this.msg = end + start
                        // }, 400)

                        // 箭头表达式写法（避免了 this 对象的不明确）
                        this.IntervalId = setInterval( () => {
                            var start = this.msg.substring(0,1)
                            var end = this.msg.substring(1)
                            this.msg = end + start
                        }, 400)
                    },
                    turnOff() {
                        // 清除定时器
                        clearInterval(this.IntervalId)
                        //将定时器 id 重新赋值为 null
                        this.IntervalId = null
                    },
                    reset() {
                        this.turnOff()
                        this.msg = '猥琐发育，别浪!'
                    }
                    
                }
            })
        </script>
    </body>
</html>
```



## 事件修饰符

事件修饰符可以串联使用

- .stop：阻止冒泡
  - 冒泡：指的是如果外层也有相同的事件时，先调用自身的事件之后也会调用外部的事件，称为"冒泡"
- .prevent：阻止默认事件
- .capture：添加事件监听器时使用事件捕获模式
  - 事件捕获模式：按照文本的解析顺序进行，先捕获的事件先运行
- .self：只当事件在该元素本身(比如不是子元素)触发时触发事件
  - 使用 self 修饰符之后，就不会存在冒泡或者捕获的问题
- .once：只触发一次



.stop和.self的区别：

​	.self只会阻止自身事件的冒泡行为，不会真正阻止冒泡行为



```html
<!DOCTYPE html>
<!-- 
    事件修饰符
    .stop:阻止冒泡
    .prevent:阻止默认事件
    .capture:添加事件侦听器时使用事件捕获模式
    .self:只当事情在该元素本身（比如不是子元素）触发时触发回调
    .once:时间只能触发一次
 -->
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
        <style>
            .inner{
                width: 100%;
                height: 150px;
                background: lawngreen;
            }
        </style>
    </head>
    <body>
        <div id='app'>
            <h2>原来的（有冒泡）</h2>
            <div class="inner" @click="div1Handler">
                <input type="button" value="点我" @click="btnHandler">
            </div>

            <h2>.stop阻止冒泡</h2>
            <div class="inner" @click="div1Handler">
                <!-- 使用.stop阻止冒泡 -->
                <input type="button" value="点我" @click.stop="btnHandler">
            </div>

            <h2>.prevent阻止默认行为</h2>
            <a href="http://baidu.com" @click.prevent="linkClick">百度一下</a>

            <h2>.capture:添加事件侦听器时使用事件捕获模式</h2>
            <!-- 先外层后内层 -->
            <div class="inner" @click.capture="div1Handler">
                <input type="button" value="点我" @click="btnHandler">
            </div>

            <h2>.self:只当事情在该元素本身（比如不是子元素）触发时触发回调</h2>
            <div class="inner" @click.self="div1Handler">
                <input type="button" value="点我" @click="btnHandler">
            </div>

            <h2>.once:时间只能触发一次</h2>
            <div class="inner" @click.once="div1Handler">
                <input type="button" value="点我" @click="btnHandler">
            </div>
            <a href="http://baidu.com" @click.prevent.once="linkClick">百度一下</a>

        </div>
    </body>
    <script>
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                
            },
            methods:{
                div1Handler(){
                    console.log("这是触发了div1的click事件")
                },
                btnHandler(){
                    console.log("这是触发了btn的click事件")
                },
                linkClick(){
                    console.log("点击了百度一下")
                }
            }
        })
    </script>
</html>
```





## v-model和双向数据绑定

双向数据绑定，当 data 改变时，视图也会相应发生改变，反之亦然。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>


    <script src="lib/vue.js"></script>
  </head>


  <body>
    <div id="app">
      <h4>{{ msg }}</h4>

      <!-- v-bind 只能实现数据的单向绑定，从 M 到 V -->
      <!-- <input type="text" v-bind:value="msg" style="width: 90%;"> -->

      <!-- 使用 v-model 可以实现数据的双向绑定 -->
      <!-- v-model 只能运用在表单元素中 -->
      <!-- input(radio, text, address, email...), select, textarea, checkbox -->
      <input type="text" v-model:value="msg" style="width: 90%;">
    </div>


    <script>
      var vm = new Vue({
        el: '#app',
        data: {
          msg: '测试1',

        },
        methods: {


        },
      })
    </script>
  </body>
</html>
```



### 实例：简易计算器

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
        

        <script src="lib/vue.js"></script>
    </head>
    

    <body>
        <div id="app">
            <input type="text"  id="firstNum" v-model:value="num1">

            <select v-model="opt">
                <option value="+">+</option>
                <option value="-">-</option>
                <option value="*">*</option>
                <option value="/">/</option>
            </select>

            <input type="text" id="secondNum" v-model:value="num2">
            <input type="button" value="=" @click="calc">
            <input type="text" id="result" v-model:value="result">


        </div>
        

        <script>
            var vm = new Vue({
                el: '#app',
                data: {
                    num1: 0,
                    num2: 0,
                    result: 0,
                    opt: '+'
                },
                methods: {
                    calc(){
                        // if(this.opt == '+'){
                        //     this.result = this.add()
                        // }
                        // else if(this.opt == '-'){
                        //     this.result = this.subtract()
                        // }
                        // else if(this.opt == '*'){
                        //     this.result = this.multiply()
                        // }
                        // else {
                        //     this.result = this.divide()
                        // }
                        switch (this.opt){
                            case '+':
                                this.result = this.add()
                                break
                            case '-':
                                this.result = this.subtract()
                                break
                            case '*':
                                this.result = this.multiply()
                                break
                            case '/':
                                this.result = this.divide()
                                break
                        }
                    },
                    add(){
                        return parseInt(this.num1) + parseInt(this.num2);
                    },
                    subtract(){
                        return parseInt(this.num1) - parseInt(this.num2);
                    },
                    multiply(){
                        return parseInt(this.num1) * parseInt(this.num2);
                    },
                    divide(){
                        if(this.num2 == 0){
                            return 'error';
                        }
                        return parseInt(this.num1) / parseInt(this.num2);
                    },
                },
            })
        </script>
    </body>
</html>
```



# 在 Vue 中使用样式

## 使用 class 样式



## 使用内联样式