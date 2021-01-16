# Vue入门



## 一.初步认识Vue

- Vue是一个渐进式的前端框架，
    - 渐进式指Vue作为应用的一部分嵌入到其中，也可以将用组件化思想管理整个项目，用其核心库以及周边生态。如：Vue+Router+Vuex等
- Vue有很多web开发中常见的高级功能
    - 解耦试图和数据
    - 可复用组件
    - 前端路由技术
    - 状态管理
    - 虚拟DOM

## 二. 安装与使用

### 1.安装方式

- CDN引入
- 本地下载引入
- NPM安装

### 2. Hello World

``` vue
<div id='app'>
    <h2>
        Hello {{name}}
    </h2>
</div>
<script>
	let app = new Vue({
        el: '#app',
        data: {
            name: 'VueJS'
        }
    })
</script>
```

## 三，Vue中的MVVM

MVVM是Model-View-ViewModel的简写，本质是UI视图与业务逻辑分开，简化理解。

- View：视图层，通常指DOM元素，给用户展示各种信息。
- Model：数据层，可以是死数据更是通过网络获取到的数据
- VueModel：视图模型层，View和model沟通的桥梁
    - 一方面它实现了Data Binding，Model中的数据反映到Model上
    - 另一方面它实现了DOM Listener，DOM发生一些事件，监听到并反馈到Model上。

## 四，Vue的生命周期

<img src="https://cn.vuejs.org/images/lifecycle.png" alt="Vue生命周期" style="zoom:25%;" />