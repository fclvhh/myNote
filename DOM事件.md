# 语法

```javascript
element.addEventListener('click', function () {

 }, false);
```

- `addEventListener()`来绑定事件
- 第一个参数  :   ''事件名"
- 第二个参数  :  ''事件响应函数''
- 第三个参数  :  表示从哪里开始触发,默认是false
  - 表示冒泡阶段执行
  - true表示捕获阶段执行



# 理解

- 阻止默认事件
  - `e.preventDefault()`
- 阻止冒泡
  - `e.stopPropagation（）`
- jQuery的话
  - `el.on（'事件名',false）`
  - false作用就相当于阻止默认事件和阻止冒泡





# 深入了解

## DOM事件触发的完整流程

![](C:\Users\lenovo\Desktop\前端博客MarkDown\assets\事件冒泡.png)





问题:

- 为什么事件的发生要分为两个阶段?

  历史原因:  微软和其他人打架

- 事件是怎么触发的?

  事件可以在捕获阶段触发,也可以在冒泡阶段触发,我们通常会选择在冒泡阶段触发

- 事件触发的效果理解

  [事件触发的效果](<http://js.jirengu.com/zayipuvafo/1/edit?html,js,output>)



## 实现点击隐藏图层的案例

[bug1-click事件的冲突](http://js.jirengu.com/lagiyocuzu/3/edit?html,css,js,output)

我们先点击button的时候,只触发fn1,可是冒泡原理让fn2 也触发了

![](C:\Users\lenovo\Desktop\前端博客MarkDown\assets\DOM事件-bug1.jpg)

[解决方案1](http://js.jirengu.com/lagiyocuzu/1/edit?html,js,output)

在div.wrap 层(button的上一层),阻止冒泡,这样document的fn2就不会执行

我们点击wrap意外的区域时,自然会触发fn2, 隐藏掉样式

**问题:**

1. 如何做到再次点击button的时候,也隐藏掉样式呢? 这样让用户体验更好啊!
2. 绑定的事件太多了? 为了节省内存,应该要优化? 如何优化呢?



**优化方案1**

为了优化内存,选择把document的事件只绑定一次,事件执行结束,就立马解绑! 

[解决方案一问题引发的bug2](http://js.jirengu.com/lagiyocuzu/4/edit?html,js,output)

![](C:\Users\lenovo\Desktop\前端博客MarkDown\assets\DOM事件-bug2.jpg)



**解决方案**

1. 在wrap层阻止冒泡,从而让fn2不执行,下次点击wrap以外的地方时,再触发就好了吗,触发一次,这个事件就自动解绑了
   1. 反思:
      1. 虽然节省了内存,但是用户体验的问题依然存在,所以不是最好方案啊
2. 设置一个定时器,从而让代码异步
   1. 冒泡阶段执行完成,定时器里面的代码才执行,这样就不会发生click冲突事件了
   2. [初步方案](http://js.jirengu.com/lagiyocuzu/2/edit?html,js,output)
      1. 这个代码仍然有问题
      2. 点击两次点我的时候,第三次就没反应了
      3. 出现bug3的原因,就是每次点击都会添加一个fn2到下一次冒泡
      4. 点击按钮 进去下一次冒泡就会导致点击事件冲突

