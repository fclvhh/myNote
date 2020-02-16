# Vue事件回调的参数问题

## 函数不需要传参时

> 绑定回调时 , 回调的 ( ) 可省去

```js
@click = "changeColor"  // changeColor() 的()省了
```



## 函数需要参数

1. 不知道要传

```js
@click = "sum"  // 本来是要传参数的 如 sum(a,b)
```

Vue会默认把event对象 传入

2. 知道要传 , 但是忘记了

```js
@click = "sum()"  //忘记传 a, b
```

Vue会默认传入 undefinded







# Vue的事件修饰符

## 事件修饰符

| 修饰符   | 作用                                   |
| -------- | -------------------------------------- |
| .stop    | 阻止冒泡 ==>event.stopPropagation()    |
| .prevent | 阻止默认事件 ==>event.preventDefault() |
| .once    | 事件只会触发一次                       |



## 按键修饰符

格式: 

```js
@keyup.按键名 = "changeX"
```

常用按键名：
.enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right



