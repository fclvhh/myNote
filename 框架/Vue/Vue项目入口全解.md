# 入口文件

## 分类

⑴main.js

⑵ index.html

⑶ App.vue

## 全解

㈠ 原则

**我们希望入口文件越简单越好**

㈡ 实现

 本来的构建模式

① 在main.js中构建 Vue实例对象, 并且传入option

②el 完成对html的挂载 , 相应的index.html  需要配合如下

```html
<div id="app">
    
</div>
```

```javascript
const app = new Vue({
    el:"#app",
    template:`<div>xxx</div>`
})
```

③将template模板在打包时 , 替换掉 el挂载的代码

```html
<div>
    xxx
</div>
```

因此可以看出 , 如果模板越复杂 , 就会导致main.js越复杂, index.js越复杂

㈢ 解决思路 : 

将template作为组件抽取出去, 组件元素相当简单

抽取过程思考图

![](assets\vue后缀文件的历史.png)



㈣ 结果

组件.js 的三级模板 .vue组件文件

实例根 和组件根

实例根为了抽离template,采用的组件抽离的方式 , 

在抽离template的过程中,模板所用的相关选项信息也要一并抽离出去 

所以实例根 变成了 组件根和index.js的一个连接点

el: 联系html

conponnets: {  App }  : 连接App.vue

App.vue作为根组件 连接 其他的分支组件



# main.js的变化

## Vue程序运行过程

**==template - - >  ast  - - >  render - - >  vdom  - - > realdom==**



## runtime-compiler 和 runtime-only 

**runtime-compiler**

```javascript
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```

Vue执行过程: 

template - - >  ast  - - >  render - - >  vdom  - - > realdom



**runtime-only**

```javascript
new Vue({
  render: h => h(App),
}).$mount('#app')
```

Vue执行过程

 render - - >  vdom  - - > realdom

跳过了模板解析成 ast语法树, ast语法树编译成render()  这样的两部过程



## render属性解析

①属性的结构

```javascript
render:function(createElement) {
    return creteElement(组件对象)
}
```

②createElement 回调函数

- 作为 render属性函数的参数
- 一般 简写成 h
- 所以有了以上箭头函数

```javascript
render: (h)=>{
    h(组件对象)
}
```

③组件对象

组件在被Vue解析后,就是一个对象



