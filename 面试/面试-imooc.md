# 自我陈述:

​	遇到不会的 , 先猜测 , 后询问

​	询问 学习资料

​	知识点补充好 , 是不是还可以过来面试?



# 页面布局

## 三栏布局

实现方案:

1. float
2. 绝对定位
3. flex
4. grid



## float实现三栏布局

[float实现](http://js.jirengu.com/veriy/2)

技巧:

​	1. float元素应该写在前面

2. center写在最后 , 调一下外边距  , 好看一点



## 父相子绝布局模型

[absuolute实现](http://js.jirengu.com/vajas/1)



## flex实现

[flex实现](http://js.jirengu.com/wexiy/1)



## grid实现

简单





# 布局面试扩展



- 上述几种布局的优缺点
- 如果去掉高度已知的条件 , 还有哪几种可以使用? 为什么?
- 在实际业务中 , 兼容性如何?那个是最实用的?



优缺点总结

| 布局类型 | 优点           | 缺点                     |
| -------- | -------------- | ------------------------ |
| flaot    | 兼容性较好     | 清除浮动影响             |
| absolute | 使用便捷       | 容器和其子元素脱离文档流 |
| flex     | 移动端完美适配 | 几乎没有                 |
| grid     | 方便快捷       | 兼容性不好               |

 

**去掉高度那个能用?**

> flex可以用

**为什么其他的方案不可以使用了**

> float : 高度撑开之后 , 没有浮盒遮挡 , 会width铺满
>
> 解决思路:  给center开启BFC
>
> absolute: 类似,没有遮盖 
>
> 解决思路: 不适合写三栏布局 , 
>
> grid也不行

**实际开发中:**

> flex和float



![](assets\面试-布局.jpg)







# css盒模型

基本概念:

​	标准盒模型+IE盒模型

区别:

css是如何设置的?

js如何设置获取盒模型对应的宽和高?

实例题

![](assets\面试-盒模型.jpg)



**最终: BFC解决margin 合并**







流的问题 : 

​		文章排版=>垂直margin合并

margin合并:

1. 父子
2. 兄弟
3. 空元素

阻止的关键 , 中间有东西挡着



float: 

​	排版:图片围绕文字

问题 : 高度塌陷 + 行盒收缩

BFC : 解决上述排版问题

1. 保持流的基本特性 : 铺满容器 , 上下排列
2. 解决流的margin合并的排版问题
   1. BFC容器内为了保证文章排版 , 因此保持margin合并
   2. 提供了一种解决思路 : 给子元素套上一层BFC容器  解决margin合并问题
3. 解决float 图片围绕文字的排版
   1. BFC开启独立空间 , 里面的元素出不去 , 外面的元素进不来
   2. BFC计算高度时 , 会计算float盒子的height
   3. BFC和float不会重叠 : 解决了行盒收缩的问题



记忆核心:

两个排版问题的解决!



# DOM事件

![](assets\面试-DOM事件.jpg)

​	 

## DOM事件级别

![](assets\DOM事件级别.jpg)



## DOM事件流程

![](assets\DOM事件流.jpg)



## DOM事件流的具体流程

![](assets\DOM事件流的具体流程.jpg)



## event对象的常见应用

event.preventDefault()  =>阻止默认事件

event.stopPropagation() =>阻止事件的冒泡

event.stopImmediatePropagation() =>当一个元素的click事件绑定了多个回调时 , 会阻止其中一个回调



event.currentTarget() 和 event.target()

=>如 ul>li*5 时 , 需要给每一个li分别绑定事件 , 可以做事件代理 , 也就是把事件绑定到ul 上

event.currentTarget() =>事件代理的父元素

event.target() =>触发事件的那个li



## 自定义事件

```js
var eve = new Event('事件名',obj);
ev.addEventListener('事件xx',function(){
    
})
ev.dispatchEvent(eve) // 这里是事件对象

// 一般是配合 延时器 实现功能的
```





# http协议类

## http的主要特点

> 简单快捷 无连接无状态



## http报文的组成部分

![](assets\面试-http-1.jpg)

四个部分



## http request  动词类型

![](assets\面试-http-2.jpg)







## get和post的区别

![](assets\面试-http-3.jpg)



要点:

​	缓存 和 安全

get : 

​	有缓存  , 回退无害

​	参数跟在 资源路径后面的请求参数 , 所以没有第四部分  请求参数长度也被限制

post : 

​		反之





## http状态码

![](assets\面试-http-4.jpg)

​	

常见的状态码

![](assets\面试-http-5.jpg)

![](assets\面试-http-6.jpg)



## http持久连接

![](assets\面试-http-7.jpg)



http是可以持久连接的 , 版本1.1才支持





## 持久连接带来的管线化

![](assets\面试-http-8.jpg)

了解 , 但是没有深入的学过 

请问有什么资料可以推荐学习一下吗?



![](assets\面试-http-9.jpg)

