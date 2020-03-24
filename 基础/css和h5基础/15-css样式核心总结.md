# css是什么?

> css:层叠样式表

## 层叠 : 

1. position定位和float 的重叠 
2. 高优先级对低优先级选择器样式的覆盖
3. 背景的重叠
   1. 背景图片
   2. 内层或者外层的背景颜色透明度设置
   3. 叠加出的效果

## 样式 :  核心是  描述事务

**如何描述一个事物?**

⑴描述事物的属性

1. 实体外观
   1. 大小
   2. 形状
2. 色彩外观
   1. 颜色
   2. 背景
   3. 透明度

⑵描述事物之间的社交关系?

1. 两者之间的是位置关系
   1. margin的微调

2. 多者之间的位置关系
   1. 对齐
   2. 两端对齐
   3. 中心对齐



# css背景

㈠img 标签

img标签作为盒子的背景

缺点: 做不了内层透明度遮罩效果   因此用的很少

优点; 可以手动设置尺寸,让图片的尺寸和盒子的尺寸完美的匹配



㈡ background 属性

背景: 作为一个事物有自己的描述规则

- 颜色
- 图片
- 对齐  position
- 重复  repeat

缺点:**图片尺寸不能手动设置!**

- 解决  background-size
- 图片尺寸和盒子尺寸不匹配时,图片按比例缩放    「图片是否完全作为背景展示的问题」
  - 牺牲背景:图片完全按比例缩放,可以完全放到盒子里面   contain
  - 牺牲图片:图片等比例缩到能覆盖盒子就行,不能完全展示   cover

优点:可以实现内衬遮罩效果

[比较案例](http://js.jirengu.com/weval/1/edit?html,css,output)

⑶总结:

因为background属性完美弥补了backround的缺点,所以涉及到背景一律用background属性

# 文本的形状

## 文本字体的原理

- 文本作为0/1代码,是需要对应的码表却解码的,之后浏览器需要根据字体设计(.ttf文件),**画出**字体
- @fant-face   自定义字体属性
  - 这个东西  iconfont会自动生成  不用自己写,能看懂就行
  - ⒈自定义字体的名字,方便font-family引用
  - ⒉字体设计文件的资源url

- 文本占位问题
  - 特殊文本需要 0/1代码占位 , 占位之后才能添加class设置font-family
  - 会混入16进制数字,可读性降低
  - font-class方式解决问题
  - symbol方式解决问题    「目前最优,照做就好」

## 字体图标

> iconfont网站制作小的图标,减少img/background的url的请求,提高网站效率





# 居中

## 水平居中

1) 若是行内元素, 给其父元素设置 text-align:center,即可实现行内元素水平居中.

2) 若是块级元素, 该元素设置 margin:0 auto即可.

3) 若子元素包含 float:left 属性, 为了让子元素水平居中, 则可让父元素宽度设置为fit-content,并且配合margin, 作如下设置:

```css
.parent{
    width: -moz-fit-content;
    width: -webkit-fit-content;
    width:fit-content;
    margin:0 auto;
}
```

fit-content是CSS3中给width属性新加的一个属性值,它配合margin可以轻松实现水平居中, 目前只支持Chrome 和 Firefox浏览器.

[浮动盒的案例](http://js.jirengu.com/buqom/1/edit?html,css,output)

4) 使用flex 2012年版本布局, 可以轻松的实现水平居中, 子元素设置如下:

```css
.son{
    display: flex;
    justify-content: center;
}
```

5) 使用CSS3中新增的transform属性, 子元素设置如下:

```css
.son{
    position:absolute;
    left:50%;
    transform:translate(-50%,0);
}
```

transform知识的基本补充:

1. 可以**平移**给定元素  除此之外还有很多
2. 坐标点一般选为给定元素的左上角
3. 相对它进行位移

[案例](http://js.jirengu.com/wimuc/1/edit)





## 垂直居中

### 单行文本

1) 若元素是单行文本, 则可设置 line-height 等于父元素高度

### 行内块级元素

2) 若元素是行内块级元素, 基本思想是使用display: inline-block, vertical-align: middle和一个伪元素让内容块处于容器中央.

```css
.parent::after, .son{
    display:inline-block;
    vertical-align:middle;
}
.parent::after{
    content:'';
    height:100%;
}
```

这是一种很流行的方法, 也适应IE7.

### 元素高度不定

3) 可用 **vertical-align** 属性, 而vertical-align只有在父层为 td 或者 th 时, 才会生效, 对于其他块级元素, 例如 div、p 等, 默认情况是不支持的. 为了使用vertical-align, 我们需要设置父元素display:table, 子元素 display:table-cell;vertical-align:middle;

**优点**

元素高度可以动态改变, 不需再CSS中定义, 如果父元素没有足够空间时, 该元素内容也不会被截断.

**缺点**

IE6~7, 甚至IE8 beta中无效.

4)flex布局

```css
.parent {
  display: flex;
  align-items: center;
}
```

5)决定定位实现居中,transform:translate() 解决百分比数值带来的问题

1. 百分比问题 : 百分比是根据父元素的content计算的
2. top:50%;left:50%;  指的是离视口左上角 的距离为父元素content的一半
   1. 但是子元素本身也在父元素里面,自身也有尺寸
   2. 所以需要两个方向上个各平移一般的尺寸

[案例](http://js.jirengu.com/yufaq/1/edit)

```css
.dad .son {
  border:1px solid pink;
  height:80px;
  width:80px;
  position:absolute;
  left:50%;
  top:50%;
  transform:translate(-50%,-50%)
}
```



### 元素高度不定

6)设置父元素相对定位(position:relative), 子元素如下css样式:

```css
.son{
    position:absolute;
    top:50%;
    height:固定;
    margin-top:-0.5高度;
}
```

ps: 水平居中

```css
.son{
    position:absolute;
    left:50%;
    width:固定;
    margin-left:-0.5宽度;
}
```



## 总结

水平居中常用

- text-align
- margin:0 auto;
- flex

垂直居中

- 百分比相对定位+transform平移回位
- flex

