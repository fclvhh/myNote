# 选择网络模块

选择一:

传统的Ajax是基于XMLHttpRequest

真实开发使用 jQuery-ajax

隐患: 只需要用到jQuery-ajax的功能 , 但是jQuery代码太大了

选择二:

ａｘｉｏｓ框架



## axios的功能特点

- 在浏览器中发送XMLHttpRequests请求
- 在node.js中发送http请求
- 支持PromiseApi
- 拦截请求和响应
- 转换请求和响应数据



## 安装

```shell
npm install axios --save
```



安装以后,**直接导入使用**

import …



# axios的基本使用

**axios(config)**

config是一个配置对象

常用配置属性

![](assets\axios-2.jpg)

method : 表示请求的方式

url : 表示路径

params : 用于 get请求的? 后面的参数拼接

![](assets\axios-1.jpg)



# 深入

## axios发送并发请求

格式

```javascript
axios.all([axios(config),axios(config)])
```

案例    打印的结果是一个数组

![](assets\axios-3.jpg)

数组分割  spread

![](assets\axios-4.jpg)



## 全局配置

axios.defaluts 是一个全局配置对象

常用全局配置项

baseURL

![](assets\axios-6.jpg)

timeout : 请求超时限制

其他的配置选项

![](assets\axios-5.jpg)



## 常用配置信息

![](assets\axios-7.jpg)





# axios实例

axios.create(config)  创建实例

![](assets\axios-8.jpg)



# axios的封装

## 为什么要封装?

不要把让组件和第三方框架直接有直接的依赖关系

两者之间 搞一层中转 

一旦第三方框架出问题,并且不再维护, 需要换框架了

这个时候不用一个一个组件的修改, 只要把中转层次的逻辑写清楚就好了



## 封装步骤

①新建一个network文件目录

![](C:\Users\lenovo\Desktop\前端博客MarkDown\assets\axios-9.jpg)

②使用封装好的axios模块

![](assets\axios-10.jpg)

③对于参数的另一种封装   参数三合一

![](assets\axios-11.jpg)

![](assets\axios-12.jpg)



## 用Promise机制完美封装

前面封装的点在于   成功回调和失败回调,需要自己手动定义箭头函数

从这个方向进行简化:

Promise有原装的成功和失败回调,  只要用Promise包裹axios异步操作就好

![](assets\axios-14.jpg)

![](assets\axios-15.jpg)

对上这两张图, 会发现在封装axios时,  instance(config)后面紧接着then()/catch()  

这是没有必要的,因为 main.js在使用axios 时 , 还是接了then()   重复了

补充一个小知识:  axios(config) => 返回的结果式一个Promise , instance(config)后面可以直接then()和 catch()的原因

正常情况下需要 return Promise(()=>{}) 的  , 这一句 应该axios帮我们写了



## 最终使用版本

![](assets\axios-16.jpg)

![](assets\axios-76.jpg)



## 框架失效

![](assets\axios-18.jpg)



# axios拦截器的使用

作用;每次在发送请求获得响应后,进行对应的处理



使用:

四种情况

- 请求成功
- 请求失败
- 响应成功
- 响应失败

分别可以对者四种情况进行拦截



格式

```javascript
axios.intercepters.request.use( config =>{return config},err =>{return err})
```



理解:

axios的拦截器在把ajax请求发送成功后,去服务器之前,  拦截住, 获取请求的参数config对象,

对请求的功能进行增强 , 之后返回config对象, 放行 ajax请求

类推; axios拦截器 会在服务器的响应回来后,拦截住,对响应的信息进行增强, 然后传给 别人

处理信息

