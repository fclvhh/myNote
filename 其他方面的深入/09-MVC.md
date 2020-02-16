# MVC是什么?

一个设计模式



定义:

1. 每个模块都可以写成三个对象 , 分别是M、V、C
2. M-Model  : 数据模型 , 负责操作所有数据
3. V-View : 视图 , 负责所有的UI界面
4. C-Control : 控制器 , 负责其他





# 为什么要有设计模式?

## 是什么?

回顾jQuery:

1. 解决浏览器兼容   适配器模式
2. jQuery()方法返回api对象       不用new
3. jQuery( ) 可以传多样参数    参数的重载
4. 链式调用
   1. api对象中的每一个方法属性都会返回  一个新的api对象
   2. 公共的数据放在jQuery 内部 , 通过闭包向外暴露
   3. 做到api对象的切换 

这些写代码的优点 , 就是设计模式





## 设计模式是怎么出现的?

> 关键字 : 重复重复重复重复重复重复重复重复重复重复重复重复重复重复!!!!!!



设计模式就是为了减少重复!!!!



1. 代码级别的重复

相同的代码你写了很多遍 , 那么你就应该重构它

2. 页面级别

类似的页面做了10遍 , 就应该有一个万金油实现方案

3. MVC 就是一个页面级别万金油

**所有的页面都**可以使用MVC来优化代码结构





# ㈠模块化

> 四个需求 =>四个模块 =>四个.js文件
>
> 1. .js文件所需要的资源怎么拿到?
>
> import 语法 , 导入需要的各类资源
>
> 2. .js文件作为模块如何给别人用?
>
> export语法 , 将模块导出 , 给别人使用



留下的问题 :  依然高度依赖index.html文件 , 因为html代码在.html文件中



# ㈡ 把html代码放到js文件中

> 思路:
>
> ​	在js中定义一个string变量 , 添加到DOM树中

```js
var html = `<div>hhhhh</div>`
var app = document.querySelector("#app")
app.appendChild(html)
```

上述代码 , 就是为了将js内存的东西 , 放到DOM内存中 , 

这些步骤就是我们认为的"渲染"



上述代码要写无数遍 , 那么进行重构

**render() 就是上述逻辑的重构**

```js
render(selector , string) {
    // 同样的代码
}
```



# ㈢缓存重构

> 浏览器自带5m的缓存 
>
> window.localstorage 属性 , 可以访问storage对象 , 这个对象里的一个hash 称为一个item , 可以setItem() 和getItem 记录数据



通过这个属性 , 功能模块可以记录用户的操作的状态 

用这个重构 代码 , 记住用户的状态 , 提升用户体验







# 小总结

## 抽象思维1

- 最小知识原则
  - 引入一个模块需要引入html、css、js
  - ==>引入html、js
  - ==>js
- 模块化 , 最小引入

## 代价

> 这样做 , 会让页面一开始是空白的 , 没内容没样式



## 解决思路

- loading 动画
- 骨架
  - 如:手机淘宝网速慢 , 会先刷出一个骨架

- 加占位内容
- SSR技术



目的: 留住用户!!!



# ㈣页面结构

> 代码逻辑:

1. 初始化html    「render(selector,string)」

2. 待使用的重要元素
3. 初始化数据   「存在storage中」
4. 将数据渲染到页面中
5. 绑定事件



# ㈤开始MVC重构

> 开始重构

## M

```js
const m = {
    //初始化数据
    data:{
        n:parseInt(localStorage.getItem(key:"n"))
    }
}
```







## V

```js
const V = {
    html:`<div>...
                  {{n}}
		  </div>`,
    // 第一次渲染
    render() {
        const xx = $(v.html.replace("{{n}}",m.data.n)).appendTo("#app") 
    },
    //将数据渲染到页面中
    update() {
        c.ui.number.text(m.data.n || 100)
    }
}
```







## C

> 简单分析:
>
> 1. 待使用的元素 是为了绑定事件用的
> 2. 而绑定事件是应该放到C中的 , 所以待使用的元素也应该放到C中去

```js
const c = {
    // init()是为了解决后面js剩余代码的问题
   init() {
    	ui:{
       		 button1:$("#id1"),
			...

    	}   
      } ,
    bindEvents() {
        
    }
}
```





# js剩余代码

```js
v.render()
c.bindEvents()
```

问题:

1. 事件绑定问题

c.ui在c中 , render在js文件末尾 

所以单render()执行时 , c.ui已经提前执行了, 在c.ui代码执行的时候 ,DOM中是未被渲染的

所以c.ui自然找不到元素

> 解决思路:
>
> 搞一个  init()

```js
v.render()
c.init()
c.bindEvents()
```



2. 渲染问题

首次渲染render() 和 之后的渲染updata() 是可以提取出来的

```js
const v = {
    el:null;
    render() {
        if(v.el === null) {
            v.el = $(v.html.replace("{{n}}",m.data.n)).appendTo("#app") 
        }else() {
         	cosnt newEl = $(v.html.replace("{{n}}",m.data.n))
            // 用新的元素替换掉旧的元素就好了
        }
    }
}
```

 这样updata()就可以省去了

```js
const c = {
    bindEvents:{
        //绑定事件 = add() {
         render()   
     }
    }
}
```



3. 关于render() DOM渲染hook的优化

```js
//之前render(selecrtor,DOMHook) hook是写死的
// #app
// 可以改成用户传入的
```

  首先 , render可以放到init()方法内部

js剩余代码:

```js
init(container) {
    render(container) ,
    ui:{
        
    }
}
// 当然了v的render(container) 也要改写
```

控制权交给用户了 , 那么需要暴露给用户

.js文件末尾

```js
export c from "xxx.js"
```





4. 页面刷新带来的事件绑定失效问题

render()的优化 , 是创建新的元素 , 替换掉原本DOM中的元素 , 会导致原本绑定事件的节点都消失了

那么绑定的事件也出问题了

采用事件委托 , 绑定到原本就存在dom中的元素 如: #app , 就可以解决问题了



一旦事件委托了 ,那么c.ui就不需要了 

全被都在bindEvnets中了







# 完整版本代码

[MVC思想重构页面](https://github.com/FrankFang/mvc-demo-2/blob/master/src/app1.js)



## 三个对象的经典属性:

```js
let m = {
    data:{}
}
let v = {
    el:null,
    html:`<p></p>`,
    init(el) {
        v.el = $('el的选择器')  //用户传入的选择器
        render()  //渲染页面 
    },
    render(v.el) {
        //将html的js节点 , 插入到DOM的Hook中
        //或者将新建的js节点 , 替换掉原本的节点 , 这叫做更新
    }
}
let c = {
    init(el) {
        
    },
    bindEvents:{
        
    }
}
```

## bindEvents的重构:

```js
events:{
    //记录需要绑定的事件 , 和待绑定的元素
}
methods:{
    // 事件的回调
}
autoBindEvent;{
    // 遍历events和methods
    // 实现自动绑定
}
```



## 自定调用render()

监听data对象的属性 是否发生了改变 , 一旦改变就自动调用 render() 方法



Object.defineProperty() 可以做到



> 此外可以 eventsBus对象也可以

```js
const eventBus = $(window)
// 数据相关都放到m
const m = {
  data: {
    n: parseInt(localStorage.getItem('n'))
  },
  create() {},
  delete() {},
  update(data) {
    Object.assign(m.data, data)
    eventBus.trigger('m:updated')
    localStorage.setItem('n', m.data.n)
  },
  get() {}
}
```

对数据的增删改查操作 , 进行完善 , 并用eventBus对象