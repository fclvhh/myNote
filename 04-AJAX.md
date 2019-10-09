# 什么是AJAX!!!

`Async`  JavaScript   And  XML (被JSON取代了)

说人话:

​	异步的javascript 和 XML

还是不懂?

​	就是一种用js发送请求的技术!

# 历史过程

## 如何发请求？

用 form 可以发请求，但是会刷新页面或新开页面
 用 a 可以发 get 请求，但是也会刷新页面或新开页面
 用 img 可以发 get 请求，但是只能以图片的形式展示
 用 link 可以发 get 请求，但是只能以 CSS、favicon 的形式展示
 用 script 可以发 get 请求，但是只能以脚本的形式运行

有没有什么方式可以实现

1. get、post、put、delete 请求都行
2. 想以什么形式展示就以什么形式展示

## 微软的突破

IE 5 率先在 JS 中引入 ActiveX 对象（API），使得 JS 可以直接发起 HTTP 请求。
 随后 Mozilla、 Safari、 Opera 也跟进（抄袭）了，取名 XMLHttpRequest，并被纳入 W3C 规范

## 为什么不用XML了?

JSON 好用啊!为什么还要用API 不好用的XML?





# 页面刷新和局部刷新

## 页面刷新

> “客户端发出请求 -> 服务端接收请求并返回相应HTML文档 -> 页面刷新，客户端加载新的HTML文档”。

客户端下载服务端数据并且在加载过程中,页面是保持刷新的,也就是小圆圈在不停的转! 这就是页面刷新!

## 局部刷新

我们来试想一个例子:

我们用js给页面上的按钮绑定一个事件,浏览器负责监听这个事件! 这个对于js来说异步的代码!

当事件触发时,浏览器通知js调用回调函数.    如果这个回调函数把input元素里面的数据改成了100 ,  那么页面是不是也相当于改变了?

答案 :  是的   

所用到的原理就是   ==使用js对数据进行更新是不需要整个页面更新的。==

所以AJAX就利用了这个原理:

1. 用js向服务器发送请求
2. 接收服务器的数据,然后用js完成对数据的更新

AJAX 的原理就和上述里面的例子是类似的,它里面的内部实现我现在没有能力,也不需要知道啊!





# 如何使用AJAX?

## 原生的AJAX 

1. 发送get请求

```javascript
// 用按钮触发事件的方式 发送AJAX请求
myButton.addEventListener('click', (e)=>{
    // 创建一个XMLHttpRequest的实例
  let request = new XMLHttpRequest()
  	// 创建一个请求 open()-->创建了一个get请求,请求路径为/xxx  请求参数也可以跟在后面
  request.open('get', '/xxx') 
    // 请求的第四部分,因为get请求的第四部分为空,所以这句也可以不写
  request.send()
    // 对onreadystatechange这个事件进行监听 目的:接收服务的响应
  request.onreadystatechange = ()=>{
      // readyState用来判断响应是否完毕了
    if(request.readyState === 4){
      console.log('请求响应都完毕了')
      console.log(request.status)
       // 响应完毕之后,就对响应进行解析
        	// 响应信息1: 请求是否成功?
      if(request.status >= 200 && request.status < 300){
        console.log('说明请求成功')
          	// 如果成功了,那么自然可以获娶到响应的数据
        console.log(typeof request.responseText)
        console.log(request.responseText)
          	// responseText属性获取响应信息
        let string = request.responseText
        // 把符合 JSON 语法的字符串
        // 转换成 JS 对应的值
        let object = window.JSON.parse(string) 
        // JSON.parse 是浏览器提供的
        console.log(typeof object)
        console.log(object)
        console.log('object.note')
        console.log(object.note)
          
      }else if(request.status >= 400){
        console.log('说明请求失败') 
      }

    }
  }
})
```

2. 发送post 请求

```javascript
// 用按钮触发事件的方式 发送AJAX请求
myButton.addEventListener('click', (e)=>{
    // 创建一个XMLHttpRequest的实例
  let xhr = new XMLHttpRequest()
  	// 创建一个请求 open()-->创建了一个pst请求,请求路径为/xxx
 	 xhr.open('get', '/xxx') 
    // 如果想要使用post提交数据,必须添加此行 设置请求头
    xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
    // 请求的第四部分,带上post请求参数
 	 xhr.send('name=fox&age=18')
    // 接下来的代码和get一致了
    // 对onreadystatechange这个事件进行监听 目的:接收服务的响应
 	 xhr.onreadystatechange = ()=>{
      // readyState用来判断响应是否完毕了
    if(xhr.readyState === 4){
      console.log('请求响应都完毕了')
      console.log(request.status)
       // 响应完毕之后,就对响应进行解析
        	// 响应信息1: 请求是否成功?
      if(request.status >= 200 && request.status < 300){
        console.log('说明请求成功')
          	// 如果成功了,那么自然可以获娶到响应的数据
        console.log(typeof request.responseText)
        console.log(request.responseText)
          	// responseText属性获取响应信息
        let string = request.responseText
        // 把符合 JSON 语法的字符串
        // 转换成 JS 对应的值
        let object = window.JSON.parse(string) 
        // JSON.parse 是浏览器提供的
        console.log(typeof object)
        console.log(object)
        console.log('object.note')
        console.log(object.note)
          
      }else if(request.status >= 400){
        console.log('说明请求失败') 
      }

    }
  }
})
```



## `axios`

jQuery的ajax早就没人用咯! 哈哈!!!!

现在都是用axios和vue代替了jQuery!!!

# 如何手写ajax?(面试常考)

我总结为四步骤:

1. 创建xhr对象 (异步对象)
2. 创建请求   
   1. post 请求 需要设置请求 ,所以需要第三步
   2. get请求  不需要设置请求 , 可以跳过第三步
3. 设置请求
   1. 设置请求头
   2. 设置请求参数
4. 绑定 onReadyStateChange 这个事件
   1. readyState   判断请求与响应的整个流程是否成功了   4
   2. stiatus 表示状态码    判断请求是否成功   200-300
   3. 获取 responseText
   4. 把responseText JSON格式的字符串解析成 js对象     JSON.parse

```javascript
var xhr = new XMLHttpRequst()
xhr.open('post','url')
xhr.setRequstHeader('content-type','application/...')
xhr.send('name=sun&age=18')
xhr.onreadyStateChange = function(){
    if(xhr.readyState === 4){
        if(xhr.status>=200&&xhr.status<=300){
           var string = xhr.responseText
           var object = window.JSON.parse(string)
        }
    }
}
```



# AJAX的同源策略

只有 协议+端口+域名 **一模一样**才允许发 AJAX 请求

1. [http://baidu.com](http://baidu.com/) 可以向 [http://www.baidu.com](http://www.baidu.com/) 发 AJAX 请求吗 no
2. [http://baidu.com:80](http://baidu.com/) 可以向 [http://baidu.com:81](http://baidu.com:81/) 发 AJAX 请求吗 no



浏览器必须保证:只有 协议+端口+域名 一模一样才允许发 AJAX 请求

处于保护隐私的角度考虑,也是互联网的基石



原因分析:

举个例子:

​	只有你本人可以看你的日记,如果别人发的AJAX请求,你的日记本服务器响应了,别人就可以看你的日记了!!!



# CORS 跨域

就一句代码

```javascript
res.setHeader('Access-Control-Allow-Origin','地址')
```

后端在写处理逻辑的时候,加上这句话,相当于为"地址"所i在的地方提供了通行证,从而可以实现跨域访问了

当然,这是后端在写逻辑的时候指定   我们控制不了哦!



