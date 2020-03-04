# 前后交互

三种方式

1. form
2. ajax
3. websock



## form

- 仅支持GET和POST
- 缺点 : 页面会刷新 , 导致用户体验不好



## ajax

- async  javaScript and xml
  - 使用内置的XMLHttpRequest 和 fetch 对象
  - 实现前后交互
- 优点 ; 交互时页面不刷新



## websock

可以由服务器端主动发起





# XMLHttpRequest的使用

## 发送get请求

1. 创建对象
2. 配置参数
3. 绑定事件
4. 发送请求

```js
let xhr = new XMLHttpRequset()
xhr.open('GET',url,true)
xhr.onload = function() {
    
}
xhr.send()
```



## 新版写法

![](assets\ajax-demo.jpg)





## 发送POST请求

![](assets\ajax-demo2.jpg)



分析 : 

post请求 , 是向服务器发送数据

1. 告诉服务器 string的格式

```js
xhr.setRequsetHeader("Content-type","xxx")
```

2. 设置超时时间 , 用来判断数据是否发送成给了

```js
xhr.timeout = 3000
xhr.ontimerout = ()=>{ }
```

3. 设置请求体

```js
xhr.send("请求体的内容")
```





# xhr对象的重要细节

## 同步 vs 异步

- open(method,url,Boolean)

