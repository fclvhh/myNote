# 深刻理解Vue

> 在学习一个东西之前 ， 我们首先应该搞清楚 ， 这个东西是个是什么！！
>
> 那么Vue究竟是什么呢？
>
> - 如果你有写过Vue的经验 「Hello World 经验就好了」, 你会发现基本没有什么DOM操作!我们不需要很繁琐的操作DOM!	所以这是它的第一个特点 : **取代的了jQuery 的DOM 操作！**
>
> - 关于Vue我们常提到的一个东西就是 , 「Vue是MVVM框架」!!! 天呐我们连MVC框架都没弄明白,就要去搞清楚MVVM框架 , 这不是神经病嘛!!!  「天啊撸 , 还是放弃写代码了吧!」  我觉得最大的问题就是 , 网上的很多回答 , 都是「用概念去解释概念!」, 看了表面上是懂了「有过项目操作经验的人可能会懂!」,  但是小白根本就不明白啊「比如我」!所以我觉得 , 学不好的问题不在于自己 , 而是那些「无良心 , 低水平的培训班搞得!」, 看上去降低了门槛 , 其实不过尔尔!  所以我看了「阮一峰」
> - 说了那么多, 究竟什么是MVVM ?   「我认为 , 目前没有必要去搞清楚框架是什么 , 由什么组成的! 那不就是又进入了一个用概念去解释概念的循环中去了嘛 」所以我们要打破这个死循环 , 直接说有什么用 : **「控制数据流向 !  」**
> - Vue 还是 **数据驱动 !**  很容易联想到 , 是不是和MVVM 框架有关呢?  这个问题本身就是一体两面的
> - Vue框架 , 操作DOM的性能提升 , 就是由于**「虚拟DOM」** 



结论: 想要比较深入的学习Vue , 至少要理解如下概念:

1. 什么是MVVM?
2. 什么是 虚拟DOM?
3. 数据驱动是什么意思?





# 什么是MVVM?

## 什么是MVC?

> MVC(Model-View-Controller）是最常见的软件架构之一，业界有着广泛应用!

- mvc模式的意思是 : 软件可以分为三个部分
  - 视图（View）：用户界面。
  - 控制器（Controller）：业务逻辑
  - 模型（Model）：数据保存
  - ![](assets\MVC-1.jpg)
- MVC 各部分的通信「数据流动」
  - View 传送指令到 Controller  (从View触发)
  - Controller 完成业务逻辑后，要求 Model 改变状态
  - Model 将新的数据发送到 View，用户得到反馈
  - 所有的通信都是单向的 ,  故而是 顺时针的
  - ![](assets\MVC-2.jpg)

## 互动模式

> 是不是用户发出的指令 , 必须要由 View模块接收呢?



- 通过 View 接受指令，传递给 Controller。
  - ![](assets\MVC-3.jpg)

- 直接通过controller接受指令。
  - ![](assets\MVC-4.jpg)

- 无论如何 「数据流动都是单向的」 顺时针的!



## 数据不再是单向流动会怎么样?

- 实际开发中 , 数据的流动会更加灵活
  - 用户可以向 View 发送指令（DOM 事件），再由 View 直接要求 Model 改变状态
  - 用户也可以直接向 Controller 发送指令（改变 URL 触发 hashChange 事件），再由 Controller 发送给 View。
  -  Controller **非常薄**，只起到路由的作用，而 **View 非常厚**，业务逻辑都部署在 View。所以，Backbone 索性取消了 Controller，只保留一个 Router（路由器） 。
  - ![](assets\mvc-5.jpg)

- 小结:
  - 业务转移
    - 业务不再是集中在Controller层, 而是大量的转移到View层了
    - Controller层只保留 简单的「路由功能 : 表现上看 类似于超链接 , 点击就会跳转! 」
  - 数据不再单向
    - View多了两个「 回指逆向」



## MVP

> MVP 模式将 Controller 改名为 Presenter「主讲人」，同时改变了通信方向

- 各部分之间的通信，都是双向的。
- View 与 Model 不发生联系，都通过 Presenter 「主讲人」传递。
- View 非常薄，不部署任何业务逻辑，称为"**被动视图"（Passive View）**，即没有任何主动性，而 Presenter非常厚，所有逻辑都部署在那里。
- ![](assets\MVC-6.jpg)



## MVVM

> MVVM 模式将 Presenter 改名为 ViewModel，基本上与 MVP 模式完全一致

- 它采用双向绑定（data-binding）：View的变动，自动反映在 ViewModel，反之亦然
  - ![](assets\MVC-7.jpg)

## 参考资料

- [阮一峰-MVVM](https://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html)
- [掘金面试题](https://juejin.im/post/5d59f2a451882549be53b170#heading-20)





# 数据驱动之双向绑定

> Vue的核心数据的双向绑定的实现所用到的原理:
>
> 1. 发布订阅者模式 (又称观察者模式)
> 2. 数据劫持  「ES6新增API – Object.defineProperty()」



## 什么是发布订阅者模式?

- 简单理解
  - 有人发布
  - 有人订阅
  - 一旦有人发布,订阅者就可以收到消息
  - 主动权在发布者手里
- 简单例子
  - 订阅报纸
  - 报社有新报纸发行了 , 会送一份给你
- 