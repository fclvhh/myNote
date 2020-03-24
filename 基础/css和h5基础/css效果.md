# 概述

这篇文档的主要作用,是记录平时开发中,常用的css效果实现, 没有什么条理

大概内容如下:

1. 简单的css效果(换句话说就是要手写,没有什么库/插件可以用的)
2. css好用的库
3. 使用库的技巧和经验

# 简单的`css`效果

## 用背景图片方式实现banner

[在线代码](http://js.jirengu.com/bifis/1)

重点知识归纳:

核心知识:

- css是层叠的样式表
- mask层是==背景色半透明==,就好像一层==面纱==
- mask是banner的孩子,会层叠banner,表现出来的效果就是==给banner蒙上了一层面纱==
- 这就是遮罩的原理

详细了解:

- css深入浅出



结论:

​	所有的遮罩层都是上述的原理,有的使用js动态实现的罢了,但是原理不变

**css层叠**



## `css`形状

所有的css形状都是用border画出来的

[box-radius属性辅助工具](https://border-radius.com/)



## `css`阴影

[box-shadow阴影在线工具](https://www.w3cschool.cn/tools/index?name=css3_boxshadow)



## sticky 

- `position:fixed`
- flex实现
  - [无bug实现](http://js.jirengu.com/xaxuh/1)
  - [有bug实现](http://js.jirengu.com/fubur/1)
- bug解决
  - bug:main overflow:auto 之后，head和foot的高度就垮掉了
  - 解决方法：在head和foot里面套一层 div ，hold住高度





# 遮罩层小结

原理: 图层层叠    css深入浅出里面有

用一个div做为遮罩层,想办法把这个div的图层提高,就可以达到遮罩效果了





# `css`动画

- [hover.css](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2FIanLunn%2FHover)
  - 非常好用的hover效果库
  - 不需要js





 