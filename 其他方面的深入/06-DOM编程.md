# 入门

## 什么是DOM?

> DOM : Document  Object  Module
>
> ​			文档对象模型
>
> 网页是一颗树 , 每个元素都是树上的节点

核心思路:

1. 将html文档结构抽象成一个树结构 
2. 树在内存如何表示呢? -- js对象
3. DOM的意思就是:

> 把html document 抽象成树结构去理解 。 树结构在内存中以对象为模型表示 ， 也就是DOM名字的由来



## 程序员的核心工作

> 操作内存!!!

所谓操作 ， 就是增删改查





## DOM标准

![](assets\DOM编程-1.jpg)

### 从数据结构角度



> DOM模型是一个树结构
>
> 树的名字 -- 文档树
>
> 树的节点 -- node节点

js对象有原型链 , 那么同样的  DOM对象也有原型链



### 从代码角度看内存

文档树 由 document对象模拟而成了  , html就是document对象

element节点是 element对象模拟的 , 作为document对象的属性而存在的

text节点是  textElement对象模拟的 , 作为element对象的属性而存在的

```js
{
   childElemnt：[
        {
        type：'node'
        tag：'head'
        }，
    	{
        	type："node"，
        	tag:"body"，
        	childNode：{
       			type:"node"，
       			tag:"div"，
       			childText：{
       				type:"text",
       				value：'msg'
             		}
          		}
    	
	 		}
   ]
}   
```



从内存角度就是这样模拟dom文档树的 

伪代码





### 从原型链角度去看内存

![](assets\DOM编程-2.jpg)

> 类比：

- js对象的原型是Object对象   =>  node节点的原型就是node对象

- js对象分为普通对象 , 数组 , 函数   =>node节点分为 element节点 , text节点 , document节点
- 所以 原型链的内存结构式一样的 , 脑海中必须记住的



> 看原型  console.dir(div1)

![](assets\DOM编程-3.jpg)



**代码结构**

![](assets\DOM编程-4.jpg)



## DOM API



> DOM API

将页面中的节点  = 「构造函数」=> js对象

这就是DOM api的作用



## js如何操作这棵树

> 浏览器往window上加一个 document 即可

js用document操作网页  , 这就是 DOM  ---- 文档对象模型



# 节点操作

## 获取节点

> querySelector / querySelectorAll

```js
var sampleSingleNode = element.querySelector('#className');
var sampleAllNodes = element.querySelectorAll('#className');
```

IE9以下不支持





## 获取特定的元素

| 获取元素       | api                      |
| -------------- | ------------------------ |
| 获取html       | document.documentElement |
| 获取head       | document.head            |
| 获取body       | document.body            |
| 获取窗口       | window                   |
| 获取所有的元素 | document.all             |



# 节点的增删改查

## 增加节点

### 总览

> 在DOM树种增加节点：

1. 在js线程中创建好节点
2. 把js线程的节点 ， 添加到DOM对象模型的内存中

### 细节

> 创建 element 元素节点

```js
let div = document.createElement("div")
```

> 创建文本节点

```js
let text1 = document.createTextNode("你好")
```

> 元素对象嵌套文本对象

1. appendChild()   设计出来是为了实现元素对象的嵌套 , 而不是专门的

```js
elementX.append(elementY)   //末尾添加
```

2. innerText()









### 小结appendChild()

创造的元素对象无论有多少层的嵌套 , 无论结构多么负责 , 都是在js的内存中的 , 并没有放到document的内存中 , 最后总是需要放到document的内存中的

```js
document.body.appendChild(divPlus)
```

或者 , 找到一个已经在document中的元素节点 

```js
let div = document.querySelect("#app")
div.appendChild(divPlus)
```

如果同一个元素节点被添加到两个元素中 , 请问这个元素节点的爸爸是谁?

```js
app.appendChild(divX)
documnet.body.appendChild(divX)
// 最终会被插入到body 中 , 因为他是后执行的
```

所以 : 一个元素不能出现在两个地方   

除非你复制一份

```js
let divY = divX.cloneNode(ture)
```



## 删除节点

### 总览

1. 在dom对象模型的内存中删除对应的元素对象索引
2. 元素对象的具体内容依然存在于js线程中

### 细节

> 两种方法

1. 旧的方法 : parentNode.childChild(childNode)
2. new metnd : 元素节点.remove()   //兼容性不好 

> 思考



## 改属性

```js
div.classList.add（）
```

> 修改style

```js
div.style.xxx = "yy"
```

注意 "-" 换成 小驼峰

```js
div.style.backgroundColor = "red"
```



> 读取clss

```js
div.classList
```





关于[classList   api](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/classList)

toggle(string, 布尔)

当只有一个参数时：切换类值；也就是说，即如果类值存在，则删除它并返回 `false`，如果不存在，则添加它并返回 `true`。

当存在第二个参数时：若第二个参数的执行结果为 `true`，则添加指定的类值，若执行结果为 `false`，则删除它。





# DOM事件

## 事件流

DOM事件分为三阶段：

1. 捕获过程：它会从window节点一路跑下去直到触发事件元素的父节点为止，去捕获触发事件的元素
2. 触发过程 : 当事件被捕获之后就开始执行事件绑定的代码
3. 冒泡过程 : 当事件代码执行完毕后，浏览器会从触发事件元素的父节点开始一直冒泡到window元素（**即元素的祖先元素也会触发这个元素所触发的事件**）





## 事件操作

注册:

```js
eventTarget.addEventListener(type, listener[,useCapture])
```

取消:

```js
eventTarget.removeEventListener(type, listener[,useCapture]);
```

触发 (这里指代码触发):

```js
eventTarget.dispatchEvent(type);
```



## 事件对象

> 调用事件处理函数时传入的信息对象，这个对象中含有关于这个事件的详细状态和信息，它就是事件对象 `event`。其中可能包含鼠标的位置，键盘信息等。

**是事件回调函数 , 浏览器传入的默认参数 , 里面包含着各种信息**

如 : 位置信息 、绑定的事件 等等

[Event对象](http://js.jirengu.com/maqef/1)

```js
box.addEventListener("click",function(event){
	console.log(event)
})
```



### 属性和方法

##### 通用属性和方法

**属性**

- type 事件类型
- target(srcElement IE 低版本) 事件触发节点
- currentTarget 处理事件的节点

**方法**

- stopPropagation 阻止事件冒泡传播
- preventDefault 阻止默认行为
- stopImmediatePropagation 阻止冒泡传播

###### 阻止事件传播

`event.stopPropagation()`（W3C规范方法），如果在当前节点已经处理了事件，则可以阻止事件被冒泡传播至 DOM 树最顶端即 `window` 对象。

`event.stopImmediatePropagation()` 此方法同上面的方法类似，除了阻止将事件冒泡传播值最高的 DOM 元素外，还会阻止在此事件后的事件的触发。

`event.cancelBubble=true` 为 IE 低版本中中对于阻止冒泡传播的实现。

###### 阻止默认行为

默认行为是指浏览器定义的默认行为（点击一个链接的时候，链接默认就会打开。当我们双击文字的时候，文字就会被选中），比如单击链接可以打开新窗口。

`Event.preventDefault()` 为 W3C 规范方法，在 IE 中的实现方法为 `Event.returnValue=false`。



## 获取元素的位置相关信息

var pos = box.getBoundingClientRect()

console.log (pos)



# 事件分类



# 事件代理





# DOM操作跨线程

## 浏览器功能划分

> 浏览器分为渲染引擎和js引擎

这两个引擎分属不同的线程 , 他们互不相干



## 跨线程操作

- js引擎不能操作页面 , 只能操作js
- 渲染引擎不能操作js , 只能操作页面



**那么DOM api 是 如何在jsz中 在页面上添加节点的呢?**



## 跨线程通信

![](assets\DOM编程-5-1577355017996.jpg)



图示:

![](assets\DOM编程-6-1577355058705.jpg)



## 插入新标签过程

![](assets\DOM编程-7.jpg)



## 属性同步

![](assets\DOM编程-8.jpg)



结论:

> 只要在js线程使用DOM api 修改的了 常见的属性 ,  那么就会触发渲染线程重绘

自定义属性不会 , 如果想强化功能 , 就使用data- 前缀



图示

![](assets\DOM编程-9.jpg)



# 属性辨析

![](assets\DOM编程-10.jpg)



> property 属性 , 是js引擎对属性的称呼
>
> attribute 属性 , 是渲染引擎对属性的称呼

