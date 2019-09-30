# www的历史

## 1990

> 万维网（World Wide Web）出生的年份。
>
> 这一年 Tim Berners-Lee 发明了用网址就能访问网页的办法，他发明了第一个网页、第一个浏览器和第一个服务器。



## 1990之前

> 人类在 1950 年 ~ 1960 年就已经发明了互联网的雏形，不过那时只有科研机构、军方和政府能进入这个网络。
>
> 后来逐步扩大到所有人都能进入这个网络。
>
> 不过 1990 年之前，没有网页。
>
> 那么人们在电脑面前做啥呢？游戏和办公软件软件不需要联网，当时也没有浏览器，所以大家是怎么上网的？
>
> 当时我还没出生，所以也不太清楚，于是我查了一下[维基百科](https://en.wikipedia.org/wiki/History_of_the_Internet#Use_and_culture)。以下是我了解到的内容。
>
> **当时人们对互联网的使用主要集中在 Email**。
>
> - 1965 年，Email 被发明出来，成为互联网的「杀手级」应用，因为你可以瞬间发一封信给远方的人，不需要信纸、邮票和邮递员。
> - 1971 年，用 @ 符号来表示 Email 的方法被发明出来。我挺好奇之前的邮箱地址是怎么表示的。
> - 1979 年，邮件讨论组被发明出来，人们可以在一个话题下公开地互发邮件。
> - 人们通过 FTP 来下载文件附件
> - 1980 年至 1990 年间，人们迫切需要一种更好的上网方式，很多方案被提出，如 HTTP 和 Gopher。后面的事情大家都知道了，HTTP 因为其易用性胜出。
>
> 当时的邮件内容全都是普通文本，或者是类 Markdown 形式的文本，人们需要一种超级文本用来做页面跳转，也就是我们现在见到的 <a> 标签，不过那时的人还没想到这一点，当时的超集文本方案有很多，HTML 只是其中之一，而且当时的 HTML 也非常简陋，只有 11 个标签。

## www的发明

> 在这种背景之下， Tim Berners-Lee（下文中称为李爵士） 在 1989 年至 1992 年间，发明了 WWW（World Wide Web），一种适用于全世界的网络。

主要包含三个概念

1. URI，俗称网址

2. HTTP，两个电脑之间传输内容的协议

3. HTML，超级文本，主要用来做页面跳转

**URL 的作用是能让你访问一个页面，HTTP 的作用是让你能下载这个页面，HTML 的作用是让你能看懂这个页面。**

这是一个简单而完美的系统。



# URI是什么

> **统一资源标识符**（英语：**U**niform **R**esource **I**dentifier，缩写：**URI**）在[计算机](https://zh.wikipedia.org/wiki/電腦)术语中是一个用于[标识](https://zh.wikipedia.org/wiki/标识)某一[互联网](https://zh.wikipedia.org/wiki/互联网)[资源](https://zh.wikipedia.org/wiki/资源)名称的[字符串](https://zh.wikipedia.org/wiki/字符串)。
>
> [URI维基百科](https://zh.wikipedia.org/wiki/%E7%BB%9F%E4%B8%80%E8%B5%84%E6%BA%90%E6%A0%87%E5%BF%97%E7%AC%A6)

URI分为

- URL : L->loaction  位置    如`https://www.baidu.com`
- URN : N ->name  名称    如:`ISBN:123321314141`

一般用URL作为网址!

![](C:\Users\lenovo\Desktop\前端博客MarkDown\assets\http-1.png)



# HTTP

## 概览

- server   服务器
- client  客户端
- HTTP

![](C:\Users\lenovo\Desktop\前端博客MarkDown\assets\http-2.png)

![](C:\Users\lenovo\Desktop\前端博客MarkDown\assets\http-3.png)



## http请求

请求格式

```http
1 动词 路径 协议/版本
2 Key1: value1
2 Key2: value2
2 Key3: value3
2 Content-Type: application/x-www-form-urlencoded
2 Host: www.baidu.com
2 User-Agent: curl/7.54.0
3 
4 要上传的数据

```

1. 请求最多包含四部分，最少包含三部分。（也就是说第四部分可以为空）
2. 第三部分永远都是一个回车（\n）
3. 动词有 GET POST PUT PATCH DELETE HEAD OPTIONS 等
4. 这里的路径包括「查询参数」，但不包括「锚点」
5. 如果你没有写路径，那么路径默认为 
6. ==第 2 部分中的 Content-Type 标注了第 4 部分的格式==



**需要注意的是:**

- put 和 patch 的作用

  - put  全部更新
  - patch 局部更新

- delete

  - 删除

- 路径和URL里面的路径是不一样的

  - url -> /s
  - 这里的话  ->带上查询参数

  

## http响应

响应格式

```http
 协议/版本号 状态码 状态解释
2 Key1: value1
2 Key2: value2
2 Content-Length: 17931
2 Content-Type: text/html
3
4 要下载的内容

```



常用的状态码

状态码要背，是服务器对浏览器说的话

- 1xx 不常用
- 2xx 表示成功  
- 3xx 表示滚吧
- 4xx 表示你丫错了
- 5xx 表示好吧，我错了

常用的状态码:

`200->成功啦  204->登录成功,常用于POST请求   `

`301->永久没有了  302->我还会回来的  304->你就用你上次下载的东西`    