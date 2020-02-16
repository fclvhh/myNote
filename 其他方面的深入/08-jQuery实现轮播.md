# 轮播的思路

1. 把图片用一个div 容器包裹
2. 图片在div里横向排列
3. 在div容器外层设置一个 wrapper 作为轮播的视口



# 轮播视口实现

1. 视口的遮挡功能

```css
overflow:hidden;
width : 图片的width
```

2. 定宽



## 轮播动画

方案一:

transform + transition



方案二 : 

margin-left + transition



方案三  :  纯css方案

animation



# 细节

## img是可替换元素

> 可替换元素 , 需要在元素上写好width 和 height 尺寸
>
> 方便占位 , 避免浏览器的重排 , 提高性能



## DOM之事件委托

> 将多个按钮事件的绑定都委托给外层封装好的代理层

1. 给代理层绑定回调
2. `div.currentTarget`  获取触发事件的按钮



> 如何知道当前按钮的排行呢?

原生的DOM

```js
var buttons = document.querySellectAll("#bottom .son")
for (let index in buttons) {
    if(e.currentTarget === buttons[index]) {
        return index
    }
}
```

jQuery

```js
var index = $(e.currentTarget).index()
```



jQuery 就是对原生api 的一个封装





# 自动播放

## 步骤

1. 给相应的按钮绑定好事件

```js
var buttons = $('#buttons>button')
for(let i in buttons) {
    $(buttons[i]).on('click',function() {
        $('.images').css({
            transform:translateX('i*-300'+'px')
        })
    })
}
```

2. 声明好计时器

```js
var size = buttons.length
// js触发事件
setInterval(()=>{
    n = n+1
    $(buttons[n%size]).trigger("click")
},1000)
```

3. 关键api

```js
trigger()
```





## 按钮高亮

> 点亮自己 ， 吹灭别人！！！

```js
var size = buttons.length
// js触发事件
setInterval(()=>{
    n = n+1
    $(buttons[n%size]).trigger("click")
    .addClass('red')
    .sibling('.red').removeClass('red')
},1000)
```

思路: 

1. 给轮播按钮添加lclassName
2. 找到他的兄弟姐妹 , 除去相应的className
3. 点亮自己 , 同时熄灭别人





# hover 时 , 取消自动轮播

> 鼠标移入 : mouseenter
>
> 鼠标移除 : mouseleaver

思路: 

1. 鼠标移入 砸掉计时器
2. 鼠标移出  新开一个计时器



setInterval()



# 无缝轮播



[swiper实现](http://js.jirengu.com/zadus/1)

