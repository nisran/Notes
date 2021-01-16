# Vue基本语法



## 一，插值操作

- mustache语法：数据是响应式的

    ``` vue
    <h2>{{myName(直接使用data里面的值，也可以使用一个表达式)}}</h2>
    ```

- v-once：只渲染一次，不会随着myName的变化而变化

    ``` vue
    <h2 v-once>{{myName}}</h2>
    ```

- v-html：将变量中的html进行渲染

    ``` vue
    <h2 v-html="link"></h2>
    ```

- v-text：与v-html相反原生展示

- v-pre：忽视mustache语法

    ``` vue
    <h2 v-pre>{{myName}}</h2>  //=>{{myName}}
    ```

- v-cloak: 斗篷，防止直接出现未编译的Mustache语法，防止闪烁

    ``` vue
    <h2 v-cloak> Hello {{name}}</h2>
    ```

## 二，绑定属性

除了直接插入模板内容外，我们还需要动态绑定元素的属性。如：a标签的href，img标签的src以及class样式等

- v-bind基础

    ``` vue
    <a v-bind:href="link">Vueu官网</a>
    <img v-bind:src="imgUrl">
    ```

- v-bind语法糖，也就是简写方式：直接写":"省略v-bind

- 绑定class

    - 对象语法

        ``` vue
        用法一：直接通过{}绑定一个类
        <h2 :class="{'active': isActive}">Hello World</h2>
        
        用法二：也可以通过判断，传入多个值
        <h2 :class="{'active': isActive, 'line': isLine}">Hello World</h2>
        
        用法三：和普通的类同时存在，并不冲突
        注：如果isActive和isLine都为true，那么会有title/active/line三个类
        <h2 class="title" :class="{'active': isActive, 'line': isLine}">Hello World</h2>
        
        用法四：如果过于复杂，可以放在一个methods或者computed中
        注：classes是一个计算属性
        <h2 class="title" :class="classes">Hello World</h2>
        ```

    - 数组语法

        ``` vue
        用法一：直接通过{}绑定一个类
        <h2 :class="['active']">Hello World</h2>
        
        用法二：也可以传入多个值
        <h2 :class=“[‘active’, 'line']">Hello World</h2>
        
        用法三：和普通的类同时存在，并不冲突
        注：会有title/active/line三个类
        <h2 class="title" :class=“[‘active’, 'line']">Hello World</h2>
        
        用法四：如果过于复杂，可以放在一个methods或者computed中
        注：classes是一个计算属性
        <h2 class="title" :class="classes">Hello World</h2>
        ```

- 绑定style

    有两种方式，①驼峰式：fontSize②原始：font-size（要加单引号）

    - 对象方法

        ``` vue
        <h2 :style="{color: currentColor, fontSize: fontSize + 'px'}"></h2>
        ```

    - 数组方法

        ``` vue
        <div v-bind:style="[baseStyles, overridingStyles]"></div>
        ```

        

## 三，计算属性

我们可以再插值语法中进行简单的运算，但是运算比较复杂是就不适合再mustache语法中编写，于是计算属性来了。

- 基本使用

    ``` vue
    <div id="app">
        <h2>{{fullName}}</h2>
    </div>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                firstNmae: 'Hello',
                lastName: 'World'
            },
            computed: {
            	fullName(){
            		return this.firstname + this.lastName
       			}
        	}
        })
    </script>
    ```

- getter和setter函数

    一个计算属性有getter和setter方法，通常默认我们使用getter函数。

- 计算属性与method的区别

    计算属性有缓存，如果多次使用只计算一次

## 四，事件监听

- v-on的语法糖：@
- 参数：默认参数event，需要传多个参数且需要event参数时通过$event传递

```vue
<div>
    <h2>点击次数：{{counter}}</h2>
    <button v-on:click="counter+1">加一</button>
    <! 语法糖简写>
    <button @click="counter+1">加一</button>
    <! 无参函数>
    <button @:click="add">加一</button>
     <! 多个参数时需要默认的even参数>
    <button @:click="addTen(10, $event)">减10</button>
</div>
<script>
    const app = new Vue({
        el: '#app',
        data: {
          counter: 0  
        },
        method: {
         	//无参方法有一个默认的参数：event   
            btnClick(event) {
                this.counter——
            }
        }
    })
</script>
```

- v-on修饰符

    - p.stop：调用 event.stopPropagation()。
    - p.prevent：调用 event.preventDefault()。
    - p.{keyCode | keyAlias} ：只当事件是从特定键触发时才触发回调。
    - p.native：监听组件根元素的原生事件。
    - p.once：只触发一次回调。

    

## 五，条件判断

- v-if、v-else-if、v-else：跟js里面的语法类似

- 渲染效果问题

    假如v-if和v-else对应的dom元素时一样的话，我们改变条件时因为Vue将进行复用Dom，我们看不到切换效果。解决方案：添加不一样的key，这样重新刷新Dom。

- v-show

    控制元素是否显示，与v-if的区别是前置display属性为none，后置压根没有Dom元素

## 六，循环遍历

- v-for

    - 数组遍历

        ``` vue
        <ul>
            <li v-for="item in itemList">{{item}}</li>
            <!-- 获取index -->
            <li v-for="(item, index) in itemList">{{index}}：{{item}}</li>
        </ul>
        ```

    - 便利对象

        ``` vue
        <ul>
            <li v-for="(value, key, index) in itemObj">{{index}}-{{key}}：{{value}}</li>
        </ul>
        ```

## 表单绑定，v-model

v-model指令实现表单元素和数据的双向绑定。

- v-model原理：其背后本上时两个操作

    - v-bind绑定一个value属性

    - v-on指令给当前元素绑定input事件

        ``` vue
        <input type="text" v-model="message">
        <!--  相当于-->
        <input type="text" v-bind:value="message" v-on:input="message= $event.taget.value">
        ```

- radio：使用同一个v-model值

- checkbox

    - 单个是v-model的值为布尔值
    - 多个是值为数组

- select

    - 单个是v-model的值为一个字符串
    - 多选是为数组

- 修饰符

    - lazy：默认情况是绑定的数据发生变化就立马更新，lazy修饰后失去焦点或Enter后改变

        ``` vue
        <input type="text" v-module.lazy="message">
        <p>当前内容：{{message}}</p>
        ```

    - number：限制数据类型

        ``` vue
        <input type="text" v-module.number="age">
        <p>年龄：{{age}}，类型{{typeof age}}</p>
        ```

    - trim：去掉左右空格

        ``` vue 
        <input type="text" v-module.lazy="message">
        <p>当前内容：{{message}}</p>
        ```