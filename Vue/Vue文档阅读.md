# 介绍

## 声明式渲染

**本质: 模板字符串替换** 只不过标识符号固定成 `{{}}`

```js
var　html = `
	<P>{{xx}}</p>
`
html.replace({{xx}},data.xx) 
```

内部做了这些事



## 条件与循环

> 结构切换 , 决定结构是否展示

思路:

1. 用js调整display的属性值
2. 用js控制DOM结构



v-show : 将数据绑定到DOM属性 display 上  **可以算是v-bind的语法糖**

v-if : 将数据绑定到DOM的结构上 , 决定结构是否存在



小结:

​	==**Vue可以把数据绑定到文本上 , 属性上 , DOM结构上**==



> 将数组数据展示到列表里

不用数组的forEach方法 , 采用v-for一键生成

**本质:**  还是使用forEach(()=>{})

这就是为什么v-for里面的三个参数正好时 forEach()里面回调的三个参数

item , i  , this

 

## 处理用户输入



事件监听

1. 创新一 : 把事件与元素的绑定写到了对应的元素上 , 省去了获取元素在绑定事件的过程
2. 创新二 : 事件回调 统一写在methods对象 ,完成**自动化绑定**



## 组件化应用创建

- 任意类型的组件都可以抽象成一个组件树
- 组件的本质 : 一个拥有**预定义选项**的一个Vue实例
- 常见组件树模板

```shell
根实例
└─ TodoList
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```

 		有一个根实例 , 根实例下面有一个根组件 , 根组件下面按照树的规则展开

- 组件类似于自定义标签 , 但又不完全相同
  - 跨组件数据流 「这里主要是父传子的通信方式」
  - 自定义事件通信「主要是子传父 , 也是单向绑定的核心思路」
  - 构建工具集成 「.vue三段式文件」







> 举例说明 : 组件传值

```html
<div id="app">
     <ol>
    <!-- 创建一个 todo-item 组件的实例 -->
      <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
 </div>
```

要点说明 : 

1. 组件传值格式

```html
<todo-item :todo="item"></todo-item>
```

​	todo是子组件的Props选项的参数名 , 绑定了父组件传下来的值item

​	在子组件的属性上传值



```js
Vue.component('todo-item', {
  // todo-item 组件现在接受一个
  // "prop"，类似于一个自定义 attribute。
  // 这个 prop 名为 todo。
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var vm = new Vue({
  el:"#app",
  data:{
    groceryList: [
      { id: 0, text: '蔬菜' },
      { id: 1, text: '奶酪' },
      { id: 2, text: '随便其它什么人吃的东西' }
    ]
  }
})
```





# Vue实例

- Vue的实例 , 常命名为vm (ViewModel 的缩写) , 也就是MVC中的View视图层的缩写



## 数据与方法

- data对象中的所有属性加入Vue的响应式系统
- 提前设计初始值的目的就是为了加入响应式



## 阻止响应式系统的追踪

`Object.freeze()`



## 生命周期钩子函数





# 模板语法

## 插值

- 胡子语法会将数据解释成普通的文
  - 那么想插入html 结构怎么办?
  - v-html
- **插值的方式 , 将决定数据的解释方式!!!**
- 插值 : 就是数据绑定

### 数据作为文本

​	**一次性插值** v-once指令



### 数据作为html结构

​	数据 

```js
let rawHtml = `<span style="color:red">哈哈</span>`
```

​	插入

```html
<p>只有我一个人想笑吗 {{rawHtml}}</p> //这是错的
<p>只有我一个人想笑吗 <span v-html="rawHtml"></span></p>
```

v-html执行: 

- 会替换掉 依附的span标签
- 在span标签占位处 , 换上rawHtml代码



### 数据作为属性



- 属性的分类

  - 值属性  :  属性的值是非布尔类型的数据
  - 布尔属性 : 属性的值是布尔类型的数据

  



### 数据作为js表达式

```js
{{n++}}
```

n : 是数据

n++ : 是js表达式

js支持所有的表达式 , 但不支持语句

 如 : 

```js
var n = n++   
```



## 指令

- Directives  
- 指令是特殊的自定义属性
- 指令的值 : 单个js表达式
- 指令的作用 : js表达式的值改变时 , 将其产生的影响 , 响应式的作用于DOM



### 参数



- 一些指令可以接收参数
- v-bind:href = “value”
  - 指令将 href属性 和  value 扳道工

### 动态参数



v-bind:[xxx] = “yyy”

xxx: 可以是一个js表达式



### 修饰符

v-on.xxx

事件绑定修饰符





# 计算属性和侦听器



## 起因

{{js表达式}} 的应用在写代码的过程中逐渐多了起来

这种写法每次视图更新的时候 , 不管js表达式中用到的data里面的属性值是否改变

都需要重新计算 

换句话说 : 没有缓存 , 效率不高



所以就为类似的表达式 , 添加了缓存 , 用computed选项来管理



## vs 方法

计算属性本质上 : 是js表达式 , 是一个值

被set/get 模仿成一个属性 , 所以称为计算属性. 又结合ES6的语法 , 以及set()的省略

因此 , 直观上和方法很像   其实是不同的两个东西 , **方法是事件的回调**

**计算属性本质上是一个js表达式**



## vs 侦听属性

本质 : 响应式的区别



计算属性 : 

​	js表达式 中的算子是响应式系统的 , 因而表达式的值也自然是响应式的

watch：　

​	自定义的响应式，原理和Vue的一样

​	只是在可以自己写,notisfy方法



小结 : 计算属性好用点

1. 不是命令式
2. 不重复





## 计算属性的setter

> 经典案例

计算属性的getter+setter  配合 v-model   实现全选的功能





## 侦听器watcher

自定义响应式





# Class 和 Style的绑定

改 : 

1. Class
2. Style
3. 结构



## 对象语法

> 决定值是否存在

{x:y,}

x是否存在 , 由y的真假决定



## 用在组件上

在父组件看来 , 子组件只是一个自定义标签而已





# 条件渲染



## v-if 配合template标签

- 指令作为属性 , 必须写在标签上
- template标签作为 , 占位符
- 指令解析后  , template标签会自动销毁 , 渲染结果不包括template标签



## v-else



## v-else-if



## 用key管理可复用的元素

- Vue会尽可能的复用已经存在的元素 「就地复用策略」
- 但不想Vue复用时 , 需要给相应的元素添加上 key属性



# 列表渲染

## 维护状态

v-for渲染列表时 , 默认采用"就地更新"策略

1. 不会移动DOM元素
2. 当数据顺序发生改变时 , 就地更新视图 , 重新渲染



这是Vue的默认处理方式 , 但是不适用于所有的情况

- 依赖子组件的数据
- 依赖临时DOM的数据

这些情况下 , 需要增加key属性



## 数组更新检测

> 响应式的bug

`Object.defineProperty`(obj,“key”,{})

需要三个参数 : 

1. obj
2. 对象属性的key名
3. 属性重写的功能增强配置 Option



其中第二个 , 必须要属性的key值 !!! 是bug的根源



1. obj ==> data 对象
2. key ==>data里面的名字

所以,如果开始的时候没有在data里面初始化好key , 是无法响应式的



解决: set() or $set



> 数组没有key 咋办???

用数组变异解决, 使用的是Vue自己写的数组API

用这些api操作 数组 , 数组也会支持响应式



### 替换数组

数组的高级api , 可以无障碍的使用



### 数组常见bug

- 你利用索引直接设置一个数组项, 无法响应式
- 你修改数组的长度 , 也无法响应式

解决思路 : 

Vue.set()

splice()

[数组注意事项](https://cn.vuejs.org/v2/guide/list.html#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)



​	

# 事件处理

# 表单输入绑定

