# 说明

Gird概览

> Grid 布局则是将容器划分成"行"和"列"，产生单元格，然后指定"项目所在"的单元格，可以看作是**二维布局**



区别于flex

> Flex 布局是轴线布局，只能指定"item"针对轴线「主轴」的位置，可以看作是**一维布局**
>
> Grid布局二维的, 交叉重叠产生 **单元格**  , 剩下的就是 将"item" 放入相应的单元格就好

[阮一峰Grid](https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

# 核心概念

**思维core : 二维**



1. 行和列  row and column



交叉区域   cell



行和列产生

和线条的关系 ;  n=>n+1



# 相关属性

## 划分容器

grid-template-columns 属性

> 划分有几行
>
> 格式:
>
> ​	后面几个数据代表有几行   
>
> ​	数值代表行height

grid-template-rows 属性

> 同理



### **行或列过多带来书写问题**  repeat

解决方案: 

​	repeat (重复次数,...行高)

```css
grid-template-columns: repeat(3, 33.33%);
```

**和行数的逻辑理解:**

还是根据`...行高` 的==个数== 决定有多少行 , 根据第一个参数决定重复次数

如: repeat(2,100px 100px  50px)

…行高: 三个数据==>三行

2 : 重复2次

总共行数 : 2*3 = 6



  

关键词**auto-fil**

> 作为repeat( ) 的第一个参数 ,表示尽可能重复的意思



**fr**关键字（fraction 的缩写，意为"片段"）

>  为了表示比例关系 
>
> 和flex-grow  一样 : 多个列 , 决定所在行占据剩余空间的比例





### minmax()

> `minmax()`函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。



auto`关键字表示由浏览器自己决定长度





## 间距

grid-row-gap 属性， grid-column-gap 属性， grid-gap 属性



## 区域

area



## 排序

flow

子元素排序

先行后列  or  先列后行



`row dense`，表示"先行后列"，并且尽可能紧密填满，尽量不出现空格



# 子元素和cell 

子元素在cell中的布局

```css
.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
```



# 内容与容器

justify-content 属性， align-content 属性， place-content 属性



```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```



**容器 = 内容 + 空余**



# item指定--> 浏览器自适应处理

item自己制定摆放位置 , 超出原本设计的布局

浏览器自适应处理的机制

 grid-auto-columns 属性， grid-auto-rows 属性

自动增加必要的行数 , 需要设置行高





# 综合属性

grid-template 属性， grid 属性

> `grid-template`属性是`grid-template-columns`、`grid-template-rows`和`grid-template-areas`这三个属性的合并简写形式。
>
> `grid`属性是`grid-template-rows`、`grid-template-columns`、`grid-template-areas`、 `grid-auto-rows`、`grid-auto-columns`、`grid-auto-flow`这六个属性的合并简写形式。



# 🎈item 属性

根据 假象线条 决定 item :

1. 从哪里开始摆放
2. 占据几个cell





## 单个item的自己调整

justify-self 属性， align-self 属性， place-self 属性

和 flex align-items 和align-self 的性质一样

