# Vue快速笔记

## 简介

> Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

## 入门案例

```html
<body>
  <div id="app">
    {{meg}}
  </div>
  <script src="vue.js"></script>
  <script>
    var app = new Vue({
      el:"#app",
      data:{
        meg:"Hello World!"
      }
    })
  </script>
</body> 
```

```
Hello World!
```

## v-text设置标签内容

```html
<body>
  <div id="app">
    <p v-text="msg"></p>
  </div>
  <script src="vue.js"></script>
  <script>
    new Vue({
      el: '#app',
      data: {
        msg:'Hello World'
      } 
    })
  </script>
</body>
```

## v-html设置html语言

```html
<body>
  <div id="app">
    <p v-html="msg"></p>
  </div>
  <script src="vue.js"></script>
  <script>
    new Vue({
      el: '#app',
      data: {
        msg:'<a href="http://skyblog.top/">博客</a>'
      } 
    })
  </script>
</body>
```
## v-on添加事件监听

> v-on:事件名

```html
<body>
  <div id="app">
    <p>{{msg}}</p>
    <button v-on:click="change">点击翻转</button>
  </div>
  <script src="vue.js"></script>
  <script>
    new Vue({
      el: '#app',
      data: {
        msg:'Hello World'
      },
      methods:{
        change:function(){
          this.msg = this.msg.split('').reverse().join('')
        }
      }
    })
  </script>
</body>
```
## v-bind绑定标签属性

```html
<body>
  <div id="app">
    <span v-bind:title="msg">
      悬停鼠标,查看动态绑定信息！
    </span>
  </div>
  <script src="vue.js"></script>
  <script>
    new Vue({
      el: '#app',
      data: {
        msg: '现在时间:' + new Date().toLocaleString()
      }
    })
  </script>
</body>		
```

## v-for循环

```html
<body>
  <div id="app">
    <p v-for="list in lists">
      {{list.text}}
    </p>
  </div>
  <script src="vue.js"></script>
  <script>
    new Vue({
      el: '#app',
      data: {
        lists:[
          {text:'学习SpringBoot'},
          {text:'学习Java'},
          {text:'学习Vue'}
        ]
      }
    })
  </script>
</body>
```

```
学习SpringBoot
学习Java
学习Vue
```



## v-model控制表单输入和应用状态

```html
<body>
  <div id="app">
    <p>{{msg}}</p>
    <input v-model="msg">
  </div>
  <script src="vue.js"></script>
  <script>
    new Vue({
      el: '#app',
      data: {
        msg:'Hello World'
      }
    })
  </script>
</body>
```

