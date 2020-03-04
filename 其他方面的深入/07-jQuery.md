# jQuery的基本知识

## 是什么?

是window对象的一个属性 , 本质上是一个函数

这个函数会返回一个对象 , 这个对象有很多很多的方法 , 把这个对象称呼为 "api对象"



## 代码风格

> 链式风格

jQuery(选择器) 获取对应的元素 , 但是不返回这些元素

返回的是一个对象 , 称为jQuery构造出来的对象

这个对象可以操作对应的元素



# 自己实现一个jQuery

## 初步实现

```js
window.jQuery = function(selector) {
  const elements = document.querySelectorAll(selector)
  const api = {
      // 闭包 ,函数维持着elements数组 , 保证在使用的时候不会删除
    addClass(className) {
      for(let i in elements) {
        elements[i].classList.add(className)
      }
      //return api  addClass的调用者就是api这个对象 , 返回this优化
      return this
    }
  }
  return api
}
jQuery(".test").addClass("haha")

```



## 直接返回对象

```js
window.jQuery = function(selector) {
  const elements = document.querySelectorAll(selector)
  return {
    addClass(className) {
      for(let i in elements) {
        elements[i].classList.add(className)
      }
      //return api  addClass的调用者就是api这个对象 , 返回this优化
      return this
    }
  }
}
jQuery(".test").addClass("haha")

```





## 小结

核心 : 闭包 + 链式操作「返回对象」





## 实现find函数

```js
window.jQuery = function(selector) {
  const elements = document.querySelectorAll(selector)
  return {
    addClass(className) {
      for(let i in elements) {
        elements[i].classList.add(className)
      }
      //return api  addClass的调用者就是api这个对象 , 返回this优化
      return this
    },
    find(selector) {
      let arr = []
      for(let i in elements) {
        const tempEle = Array.from(elements[i].querySelectAll(selector))
        arr = arr.concat(tempEle)
      }
      // 这里return 什么呢? 
      // 如果是this , 会出现什么问题呢? 链式调用出错!!!
      return this
    }
  }

}
jQuery(".test").addClass("haha").find(".child").addClass("xixi")
// 这里第二次调用的addClass , 但是操作的依然是.test元素 ,而不是我们
// find()的.child元素  这就是链式调用处问题了

```



## 改变操作的元素

1. 我们不能直接返回一个api 对象  , 因为这个api对象是最开始创建的 , 会出现链式调用bug
2. 因此 : 我们每实现一个功能函数 , 都在函数末尾 , 返回一个新的 api对象 , 如api2…
   1. 这个api对象的 是jQuery() 函数构建的
   2. 所以要求jQuery()不能只接受selector这一个参数
   3. 这样的话 , elements 变量 就会相应的变化
3. jQuery 函数 需要进行参数重载
   1. 所谓重载 : 就是对不同的形参类型进行不同的操作

```js
window.jQuery = function(selectorOrArray) {
    // 1. 参数重载
  let elements 
  if(typeof selectorOrArray ==="string") {
     elements = document.querySelectorAll(selector)
  }else if(selectorOrArray instanceof Array) {
      elements = selectorOrArray
  }
  return {
    addClass(className) {
      for(let i in elements) {
        elements[i].classList.add(className)
      }
      //return api  addClass的调用者就是api这个对象 , 返回this优化
      return this
    },
    find(selector) {
      let arr = []
      for(let i in elements) {
        const tempEle = Array.from(elements[i].querySelectAll(selector))
        arr = arr.concat(tempEle)
      }
      // 2. 一个操作对象 , 对应一个api对象 , 这样链式调用就不会出错
      let newApi = jQuery(arr)
      return newApi
    }
  }

}
jQuery(".test").addClass("haha").find(".child").addClass("xixi")
```





核心 :  

- 让一个操作对象 , 有一个自己的`api`对象 , 这样就不会链式调用出错
- 为了实现这个需求 , 就需要对jQuery 函数进行参数重载



## 实现end函数

> 现在希望可以 , 直接通过一个函数把操作的元素切换成之前的元素



这就是end() 函数的功能切换api对象

```js
window.jQuery = function(selectorOrArray) {
  let elements 
  if(typeof selectorOrArray ==="string") {
     elements = document.querySelectorAll(selector)
  }else if(selectorOrArray instanceof Array) {
      elements = selectorOrArray
  }
  return {
    oldApi = selectorOrArray.oldApi
    addClass(className) {
      for(let i in elements) {
        elements[i].classList.add(className)
      }
      //return api  addClass的调用者就是api这个对象 , 返回this优化
      return this
    },
    find(selector) {
      let arr = []
      for(let i in elements) {
        const tempEle = Array.from(elements[i].querySelectAll(selector))
        arr = arr.concat(tempEle)
      }
      arr.oldApi = this // 这个this是切换操作元素钱的api对象
      return jQuery(arr)
    },
    end() {
      return this.oldApi // 完成操作元素的切换
    }
  }

}
jQuery(".test").addClass("haha").find(".child").addClass("xixi").end()
.addClass("yellow")

```







# 使用jQuery

## 前置

学习DOMapi时 , 是按照 `增删改查` 的顺序的

但是学习jQuery 则是按照 先学查 

这是因为 : jQuery(selector) 的设计



## 查

| 含义 | 英文     |
| ---- | -------- |
| 兄弟 | siblings |
| 排行 | index    |
| 弟弟 | next     |
| 哥哥 | prev     |
| 后代 | find     |
| 儿子 | children |
| 遍历 | each     |



## 增

> 增: 

1. js线程创建节点
2. 插入到DOM对象模型中

jQuery() 自带innerHTML 特性 , 直接创建好节点



> why? 

因为jQuery内部参数重载了



> 插入的api

appendTo()   // 把新增的元素放到另一个元素中  `小儿子`   

这样设计很爽啊!!!

| 插入api    |                    |
| ---------- | ------------------ |
| appendTo() | 创建元素后链式插入 |
| append()   | 尾插               |
| prepend()  | 前插               |
| after()    | 添加弟弟           |
| before()   | 添加哥哥           |

**after()和before（） 又叫做外部插入**

##  **想知道细节**

通过api对象查看细节

```js
const $div = $('#test')
```





## 删除

```js
$('div').remove（）
$（'div'）.empty（）
```





## 改

| api               | 作用         |
| ----------------- | ------------ |
| text()            | 读写文本内容 |
| html()            | 读写html内容 |
| attr()            | 读写属性     |
| css()/style       |              |
| addClass()        | 添加属性     |
| removeClass（）   |              |
| hasClass()        | 查看属性     |
| on(“事件名”，fn)  | 添加事件     |
| off(“事件名”，fn) |              |





# jQuery的设计模式

1. 不用new的构造函数
2. $(支持多种参数) , 这个模式叫做重载
3. 用闭包隐藏细节
4. `$div.text()` 可读可写 , getter/setter模式
5. jQuuery 阵对不同的浏览器使用不同的代码 , 这叫做适配器





# jQury的经典逻辑

## jQuery对象 -yes

- 使用jQuery对象 , 远比使用 DOM对象好用

```js
$(DOM对象)  //会返回一个含有api 的jQuery对象
```

## 选项卡逻辑

- tabbar选项卡的逻辑

  1. 事件委托

  ```js
  $("#tabbar").on('click',"li",()=>{
      const $li = $(e.currentTarget)
  })
  ```

  2. jQuery的on()

  ```js
  on('事件','委托者的selector',回调)
  ```

  3. 永远不要在js中写css 「行为样分离的原则」

  ```js
  $xx.css()  / show() / hide() 这样的api不要用
  ```

  ```js
  // 采用添加class的思路
  $("#tabbar").on('click',"li",()=>{
      const $li = $(e.currentTarget)
      const index = $li.index() // index() 这个jq api,可以知道这是第几个li
      $("#tabbar").children() //返回$xx对象数组
      .eq(index)  //其实是数组遍历语法糖 , 找到对应的li
      .addClass("active")  //点亮自己 , 熄灭别人
      .siblings() //找到li的所有兄弟
      removeClass("active")
  })
  ```

  4. 点亮自己 , 熄灭别人 **是好用的思维**
  5. 设置选项卡的初始状态
  
  ```js
  $("#tabbar").children().index(0).trigger("click")  //trigger()自己触发事件
  ```





## 点亮自己 , 熄灭别人 

实现步骤:

1. 事件委托
   1. 爸爸绑定事件 
   2. 回调时 , 用`currentTarget` 找到真正的触发者
   3. 找到爸爸所有的孩子 , 遍历孩子数组 , 找出 触发者的index   「jq index()语法糖」

2. 找到真的触发者 , 为它addClass()  实现效果
3. 找到其他候选人 , removeClass()  真的实现效果
4. 赢家通吃的思维



## "左右横跳"逻辑

1. 默认状态1 , 第一次点击 , 进入状态2
2. 第二次点击 , 进入状态1

```js
$("#div").toggleClass("live2")
```

