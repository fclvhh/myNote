# 盒子模型

## 块级盒子和行级盒子

### 块级盒子

- 特征
  - 盒子会在水平方向上扩展并占据父容器在该方向上的所有可用空间
  - 每个盒子都会换到新行
  - width和height属性有效
  - 内边距（padding）,外边距（margin）和边框（border）会将其他元素从当前盒子周围**推开**
  - 可通过display属性改变显示类型
- 常见的块级别标签
  - 标题   h1~h6
  - 列表相关   「ul ol dl  」|  「li  dd」
  - 章节,内容结构
    - header  footer  main  aside
    - section



### 行级盒子

- 特征
  - 盒子不会产生换行。
  - width和height属性将不起作用。
  - 内边距、外边距以及边框会被应用但是**不会**把其他处于inline状态的盒子**推开**。
  - 可通过display属性改变显示类型
- 常见的行级标签
  - `<a>、<span>、<em>、<strong>、<img>、<input>`



## 小结

- 区别
  - 可以换行吗?
  - w 和h 属性?
  - 是否推开?

# css盒模型

> 完整的CSS盒模型应用于块级盒子，内联盒子只使用部分内容模型定义了盒的每个部分——margin,border,padding,and content



## 盒子模型的各个部分

- 盒模型组成
  - Content:这个区域是用来显示内容，大小可以通过设置width和height.
  - Padding:包围在内容区域外部的空白区域；大小通过padding相关属性设置。
  - Border:边框盒包裹内容和内边距。大小通过border相关属性设置。
  - Margin:这是最外面的区域，是盒子和其他元素之间的空白区域。大小通过margin相关属性设置



## 标准盒模型

- 设置的width和height指的是Content
  - 宽度=350+25+25+5+5，高度=150+25+25+5+5
  - margin不计入实际大小，但会影响盒子实际占用空间

![](assets\盒子模型.jpg)



## IE盒模型

- 设置的width和height包含内边距和边框
  - 浏览器默认使用标准盒模型，如想使用IE盒模型可以设置box-sizing
    - border-box
    - content-box

![](assets\盒子模型2.jpg)



## 究竟用那种盒模型好?

- 正常人的思维
  - 我们在放东西的时候 , 只会考虑"物品"大小 , 很少会考虑包装的厚度
    - 因为对于我们来说「物品就是自带包装啊!」
    - 这是很自然的 想法吧!
  - 换句话说 , 「IE盒子就很符合我们正常的思维」
- 因此 , 我们用标准盒模型很容易产生
  - "物品+包装" 之后就放不下的问题
- 推荐就用IE盒模型算了



## 让页面全部元素使用IE盒子模型

```css
/*方法*/
html {
	box-sizing:border-box;
}
*,*::before,*::after {
    box-sizing:inherit;
}

```

这个方法的优点

- 但我有 "弹窗" 需求时 , 我想让弹窗 div 为 content 盒子模型
- 这样写 , 它的 子元素 也会继承 , 为content盒模型
- 效果更好





# 盒子模型属性

- margin
  - 四边是顺时针赋值
  - 也可以单独设置
  - ![](assets\margin1.jpg)
  - ![](assets\margin2.jpg)

- 盒模型居中
  - `margin:0 auto;`
- padding

```css
.box {
  	padding:10px;
  	padding:10px20px;/*10px20px10px20px*/
    padding:10px20px30px40px;/*上右下左*/
}
.box {
    padding-top:10px;
    padding-bottom:20px;
    padding-left:30px;
    padding-right:40px;
}
        
```

- border

```css
.box {
    border:1px solid black;/*solid、dashed、dotted*/
    border-radius:4px;
    border-radius:50%;
}
.box {
    border-top:10px solid #ccc;
    border-bottom:5px dashed yellow;
    border-left:10px solid transparent;
    border-right:1px dotted black;
}
```

- width 、height

  - 属性值: 100%   代表着什么?
    - 让自己的content大小等于父元素content的100%
  - 带来的问题
    - 如果自己本身还有 padding 等其他的盒子模型相关的属性
    - 就会造成 
      - 自身的大小 超过父容器盒模型 的 content 的大小
      - 出现很奇怪的布局
  - 父元素宽度 一般都是要设置的
    - 但是高度却不一定 , 因为让子元素撑开嘛
    - 出现了需求的 , 再添加高度

  

- display



# 内联盒子

- [示例](http://js.jirengu.com/rirur/1)
  - span的span的宽度和高度设置无效；
  - 左右外边距和左右内边距生效 「占据空间」；
  - 上下外边距、上下内边距 和 **边框**「样式生效」，但不占用空间



# display属性值inline-block详解

- inline-block
  - 会具有块盒子、内联盒子的 双重特性
  - 设置width和height属性会生效
  - padding,margin的「上下生效」以及 border会推开其他元素
  - 它不会跳转到新行 (不换行)
  - [没有inline-block的导航条](http://js.jirengu.com/joqec/2)
  - [有的](http://js.jirengu.com/joqec/1)
- inline-block面试题之 缝隙问题
  - [存在缝隙问题](http://js.jirengu.com/joqec/1)
  - 缝隙产生的原因
    - TAB、LF、FF、CR、SPACE都是空白字符
    - 多个连续的空白字符会合并成一个空格，而空格也占据一个字符的空间
      - 这就让不同的a链接直接产生了「一个空格的缝隙」
    - white-space属性可以修改合并规则
  - 解决方法
    - 设置font-size 解决
      - [示例](http://js.jirengu.com/fubuj/1)
    - 通过更改布局方式来解决，比如使用float、flex、grid布局
- CR、LF  「都代表换行的意思 , 老式打字机换行的两个操作」
  - CR(/r)（Carriage Return）回车。老的打字机打印头回到纸张最开头
  - LF(/n)（Line Feed）换行。老的打字机打印头向下移动一行
  - 对于Windows，CRLF代表换行
  - 对于老Mac，CR代表换行
  - 对于新Mac、Linux，LF代表换行
  - FF - -代表换页
- HTML实体
  - 在HTML中，`&lt;` `&#60;`<都表示小于
  - 在CSS里，需要转换成16进制。如span::before{content:'\003C';}
- [unicode 码表](https://unicode-table.com/en/#myanmar)



# display属性值

- none
- block
- inline-block
- inline
- flex
- inline-flex
- grid
- inline-grid
- table
- table-cell



# 外边距合并

- 定义

  - 块级元素的上外边距和下外边距有时会合并（或折叠）为一个外边距，其大小取其中的最大者

- 浮动元素和绝对定位元素的「外边距」不会折叠

- 发生场景

  - 相邻元素
    - 相邻的两个块级元素之间的(上下)外边距会折叠
  - 父子元素
    - 如果在父元素与其第一个子元素之间不存在「边框、内边距、行内内容」
    - 也没有创建「块格式化上下文」、或者「清除浮动」将两者的**margin-top分开**
    - 父子外边距会折叠
  - 空的块级元素
    - 如果一个块级元素中不包含任何内容
    - 并且在其「margin-top」与「margin-bottom」之间没有边框、内边距、行内内容、height、min-height 将 **两者分开**
    - 该元素的上下外边距会折叠

  

- 注意
  - 如果==参与折叠==的**外边距中包含负值**，折叠后的外边距的值为最大的正边距与最小的负边距（即「绝对值最大」的负边距）的和。
  - 如果所有参与折叠的**外边距都为负**，折叠后的外边距的值为最小的负边距的值。这一规则适用于相邻元素和嵌套元素

## 阻止外边距合并

- 核心
  - **将外边距隔开**
  - 外边距之间有的东西是
    - border
    - paddding

方法:

1. 加border

2. 加padding
3. 创建块级格式化上下文
4. 改变盒子特性（如浮动、绝对定位、改变display)





