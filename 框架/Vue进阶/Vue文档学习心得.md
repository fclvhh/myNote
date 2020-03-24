# 选项/数据

## data和响应式

1. data对象

> 遍历data对象的key , 用getter/setter 重新定义属性 , 并增加监听

```js
Obejct.keys(data).forEach(key=>{
    let value = data[key]
    Object.defineProperty(data,key,{
        get(){
            
        },
        set(newValue){
            this.value = newValue
        }
    })
})
```

2.  观察者模式

> data对象的每个属性都是一个subject

```js
data.forEach((x)=>{
    x = new Dep()
})
```

3. 解析html文档

```js
var useData = []
用的data的元素 , 添加到数组里
useData.forEach((y)=>{
    y = new Watcher()
})
```

4. data的流程

> data是被监听的
>
> 一旦data.xxx 被修改 , 那么xxx的subject 会调用自己的notisfy()
>
> 遍历watcher缓存 , 调用watcher的updata() 更新视图
>
> watcher是在Vue的html解析的时候就生成好的





## data与双向绑定

> 现在有一个问题:
>
> ​	当多个组件 , 共用了同一个data.xxx的数据的时候
>
> 如果按照双向绑定 : 
>
> - xxx属性是被监听的
> - 那么所有的组件都应该是观察者 , 会被通知的
>
> 但是实际上 , 因为技术因素 , 双向绑定并不智能 , 存在很多问题:
>
> 所以实际上是:
>
> ​	单向绑定实现的



单向绑定:

1. 必须通过js改变数据 , ui界面是无法修改的
2. 用户在ui的输入数据如何传递到 js呢?

> 利用事件传递

给ui「也就是html模板」, 绑定好input事件

当用户输入 , input事件触发 , 执行回调

```js
$xxx.input = function(e) {
    this.value = e.target.value
}
```

通过这样的方式 , 就把 uidata传递给了父组件



## data要点

1. 遍历data对象 , 创建subject的
2. 所以要想响应式 , 必须在data里初始化好
3. 才可以遍历 ,  创建subject ,  实现数据监听



## Vue的data和组件

1. 单个组件的data在组件内部是双向绑定的
2. 父子组件传递data时, 是单向绑定的
   1. 父组件可以直接传值给子组件
   2. 子组件需要通过触发父组件的事件 , 通过函数传值
   3. 这是半自动的双向绑定 , 借鉴了React的单向绑定







## props与单向绑定

> 组件之间数据是单向绑定的
>
> 1. 多个组件共用数据
> 2. 把数据放到父组件
> 3. 当子组件修改数据时 , 会有第三者 , 记录修改 , 通知父组件 , 父组件真实的修改 , 所有的子组件就都被通知了



**其实从这里就可以感觉到 , 单向绑定实际上更好用**



## computed与数据加工

> 需求:
>
> ​	将加入响应式的数据 , 进行一层加工

```js
<p>{{(n1+n2)*n3}}<p>
```

这样貌似能解决需求 ,但是每次更新页面的时候 , 都会重新调用这个计算逻辑 , 

这样是需要占用资源的



> 解决 : 
>
> ​	为加工好的数据 , 提供缓存 , 这样当页面刷新好以后
>
> ​	也不用重新计算

computed: 这个选项就是提供数据缓存功能的

1. 依托于data里面的数据 , 所以也是响应式的
2. 里面的属性是 虚拟属性 , get/set 方法模拟的
3. 当data里的简单数据被修改时 , 会调用setter()
4. 当计算属性被读取时 , 调用getter()方法

## methods与事件

> 事件绑定的回调



## watch \ 定制专属响应式

> 当监听到data.xxxs属性变化时 ,  Vue有一套自己的处理逻辑
>
> 需求:
>
> ​	我想添加一些 , 自己的需求 ,
>
> ​	就可以到这个选项 , 给xxx的subject的notisfy()
>
> ```js
> function notisfy(watcher){
>     watchers.forEach(watcher=>{
>         watcher.updata()
>         // 自定义方法
>         watch.sayhi()
>     })
> }
> ```
>
> 





# 选项/DOM

## el

> 挂载容器

1. view层的功能
   1. 创建html模板
   2. render(data)
      1. 拿到数据 
      2. 进行模板字符串替换
      3. 插入到文档中
2. 文档插入需要已经在浏览器渲染线程中的DOM元素
3. 这个就是el容器 ,  通过el获取这个容器





## template

> 字符串模板





## render()

1. 模板字符串替换
2. 插入到DOM 容器中

当监听到数据改变 , Vue帮我们自动调用的, 

是watcher的update()

```js
update() {
    render(data)
    // 
    ....
}
```



# 选项/生命周期钩子函数

## 理解几个关键词

| 关键字    | 含义                                                |
| --------- | --------------------------------------------------- |
| created   | MVC中的M的用户数据的获取后执行                      |
| mounted   | MVC中的v中的DOM插入  render()的其中一个功能, 后执行 |
| update    | MVC中的v中的render()方法调用后执行                  |
| destroyed | 销毁Vue的实例                                       |

vm.$destroy()销毁实例 , 触发destroyed钩子



## **实例方法和生命周期:**

| 方法                                           | 生命周期            |
| ---------------------------------------------- | ------------------- |
| $mount(selector)  手动挂载                     | 触发mounted生命周期 |
| $destroy()  手动销毁                           | 触发destroy生命周期 |
| $forceUpdate() 强制更新ui                      | 触发update生命周期  |
| $nextTick( [callback\] ) 下次更新时 , 触发回调 |                     |









# 指令

> 一些常用功能的封装

1. 点亮自己 , 熄灭别人

```js
$xx.on('click','li',(e)=>{
    var index = $(e.target).index()
    $xx.find('li').eq(index).addClass('active')
        .siblings().removeClass('active')
})
```

这个代码逻辑的关键是:

1. 事件委托 :  这样可以少写事件绑定
2. 获取当前点击元素的下标  
3. 通过下标获取元素 , 添加class , 移除兄弟class



Vue的实现:

v-for + v-on + v-bind

1. v-for : 直接在li上使用 . 遍历生成元素 , 元素在数组的下标**index也可以轻易得到**
2. v-on由于绑定在元素上 , 可以跟着li复制 , 不存在重复写的情况 . 所以**Vue中事件委托消失了**
3. 添加class和移除class操作 , 逻辑上就是  布尔操作







2. 通过修改display:none/block  , 或者切换dislay属性 , 决定元素是否在页面上显示

v-show : boolean   决定css的display属性

v-html : 更新innerHtml

v-text :  更新textContent  , 文本节点



3. 动态决定html结构是否渲染到文档中

v-if + v-else + v-else-if





小结: 第二点和第三点 是我们做弹出层 , 对话框常用的两个思路

1. display <==>  v-show
2. 动态插入渲染  <==>v-if

## 指令辨析

v-if: 模板动态渲染 -- 用时插入到DOM

v-show : display:none/block;



v-if 和 template 配合使用

tempate 上的指令解析过后 , 会被动态替换 , 不会影响html的结构





> 指令对应webapi的对照表

| v-指令 | webApi    |
| ------ | --------- |
| v-text | innerText |
| v-html | innerHtml |
|        |           |



## 自定义指令

1. 全局函数 directive() 创建自定义指令

```js
Vue.directive('指令名',{
    // 选项对象
})
```

2. 三个钩子函数

| 指令名           | 特点       | 执行时机                 |
| ---------------- | ---------- | ------------------------ |
| bind             | 只调用一次 | 在绑定到元素上时调用     |
| inserted         | 可多次调用 | 元素在插入父节点时调用   |
| update           |            | 更新的时候调用(没怎么懂) |
| componentUpdated |            | 组件更新完毕后调用       |
| unbind           | 只调用一次 | 指令与元素解绑时调用     |



3. 钩子函数参数



# 版本问题

### [运行时 + 编译器 vs. 只包含运行时](https://cn.vuejs.org/v2/guide/installation.html#运行时-编译器-vs-只包含运行时)



不完整版本  , 不能直接把在js中使用视图

1. 无法读取 , template中的数据
2. 不支持 template 选项



那么template选项该如何代替呢?

一个神奇的函数代替!

render(h){	

​	return h()

}

这个h是一个函数 ,  功能类似于webApi的createElement()



这种形式 , 要手动创建DOM结构 , 不像innerHtml()的智能分析



所以:

我们写代码的时候 , 用完整版本

部署的时候 , 用不完整版本

那么如何做到呢?

webpack 打包的时候转译啊 

vue - loader 啊



# 过渡

DOM的改变: 插入 , 更新 , 移除时 , 提供过渡效果



## transition 组件

> 描述

用transition 组件 包裹 的元素 , 在改变时 , 就会自动添加过渡效果



> 过渡动画的两种实现方式

1. 纯css --> animation 属性
2. js和class  transition tansform 睡醒  , js改变状态 , 提供动力



> 处理机制

transition组件:

1. 自动检测元素是否用了css的过渡 , 如果有则自动添加类名
2. 自动检索是否有js钩子函数 , 在恰当的时候自动被执行

> 过渡的类名

默认有6个class 切换

1. enter
   1. v-enter-active 进入的整个过程
      1. v-enter 开始
      2. v-enter-to 进入了
2. leave 同理





# 可复用性

## 混入

mixins : 不使用类 , 创造子类 , 并混入原型新的方法



