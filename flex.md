[参考资料](https://juejin.im/post/58e3a5a0a0bb9f0069fc16bb#heading-0)



# 概览

![](C:\Users\lenovo\Desktop\assets\flex布局.png)

- 什么是flex?

> 每个items就像是一个弹簧,可以压缩或是拉伸,从而灵活的满足布局需求

- 为什么不用left/right,而用start/end

> 因为轴是有方向的



# 基本概念

1. 父容器+子容器
2. 轴

需要知道的是:

1. 父容器可以控制子容器的排列,子容器自身也可以控制自己的排列.当两者冲突时,子容器优先
2. 主轴就是父容器的=='水平’'==二分线,方向是可以控制的,顺时针旋转90°并且与主轴交叉的轴就是交叉轴(==父容器的垂直二分线==)
3. 主轴和交叉轴就是排列的基准,选那条轴作为标准是可以控制的



# 实现细节

## 确定容器

```css
.father{
    display:flex;
}
.father这个元素所在区域就是父容器
.father这个元素的子元素就是子容器
```

## 确定轴

1. 轴是有方向的
   - 右: row
   - 左: row-reverse
   - 下: column
   - 上:column-reverse
2. 主轴方向确定了,那么交叉轴方向也就确定了
3. `flex-direction: row`



## 可以开始排列了

### 选择一个基准进行排列

![](C:\Users\lenovo\Desktop\assets\flex-2.png)



### 选择对齐方式

设置子容器按照「**主轴排列**」

①内容对齐方式

- 两端对齐
  - 确定两端
  - 每行均匀分布
    - 两端是否留空
    - between
    - around
  - 摆放顺序
- 居中对齐
  - 确定行center
  - 确定内容center
  - 行center和内容center的对齐

![](C:\Users\lenovo\Desktop\assets\flex-3.png)

以上五种对齐方式是常用的



设置子容器按照「**交叉轴排列**」

![](C:\Users\lenovo\Desktop\assets\flex-4.png)

以上五种对齐常用



要点:

> **baseline**：基线对齐，这里的 `baseline` 默认是指首行文字，即 `first baseline`，所有子容器向基线对齐，交叉轴起点到元素基线距离最大的子容器将会与交叉轴起始端相切以确定基线。



![img](C:\Users\lenovo\Desktop\assets\f78e9f42be9a3f165f8f.png)

> **stretch**：子容器沿交叉轴方向的尺寸拉伸至与父容器一致。



![img](C:\Users\lenovo\Desktop\assets\160170b3d2022800ffea.png)



# 初步反思

上面的内容,可以很顺利的实现各种布局,但是有一个问题值得思考?我们这样就可以做到弹性布局了吗?

弹性体现在什么地方呢?我们的小弹簧呢?



# 小弹簧出场咯

> 每个items都是一个小弹簧,他的伸缩是可被控制的!



## 实现细节

### 选择基准吧

- 在主轴上如何伸缩：**flex**

![](C:\Users\lenovo\Desktop\assets\flex-5.png)



# 完善功能

1. 当父容器的一行==''装不下''==子容器的时候,可以换行吗

   > ​	设置换行方式：**flex-wrap**

![img](C:\Users\lenovo\Desktop\assets\19fb0f3a31fa497191b8.png)

2. 让html结构在后面的先展示–-圣杯布局

   ​	flex的 子容器的 一个  order属性解决

