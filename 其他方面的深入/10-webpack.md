# 安装

> 官网安装

```shell
npm install --save-dev webpack@xx
npm install --save-dev webpack-cli@xx
```

> 学习时

```shell
npm install -S -D webpack@4
npm install webpack-dev-serve
```



# 是什么

- 转译代码
- 构建build
- 代码压缩
- 代码分析



# 使用

## 如何运行本地的webpack

```shell
npx webpack
```

or

```shell
webpack文件所在目录  webpack
```



## 转译js

可以把index.js 及其引入的变量 , 都**编译生成**一个main.js

这个main.js 是对index.js 做了**识别替换 和 浏览器兼容**



## 警告解决

> WARNING in configuration
> The 'mode' option has not been set

配置文件报错 , mode选项没有设置

去官方文档 , 找到configuration





# 配置文件的options

[照抄](https://webpack.js.org/configuration/#options)

| options | 作用                     |
| ------- | ------------------------ |
| mode    | 开发or产品               |
| entry   | src的入口js              |
| output  | 编译生成的文件和文件地址 |



## 关于缓存

filename:'[name].[contenthash]'

当文件改变时 ,  再次打包时 , 会自动生成新的文件名

这个文件名的改变 , 会触发浏览器去请求新的资源 , 而不会去访问缓存



配合Cache-control 这个响应头 需求 , 而特意设计的一个功能



##  问题解决

> 打包会生成新的文件 

1. 先删除以前的文件
2. 然后再重新打包

```shell
rm -rf dist ; npx webpack
```



为了避免忘记 , 封装一个script 脚本命令

```json
{
    "scripts":{
        "build":"rm -rf dist && npx webpack"
    }
}
```



# 第一个插件html

1. 搜索 webpack htmlplugin
2. 参看配置文档





# 第一个css load

引入css到文件中

> 模块化的一个问题

只可以引用.js文件  , 不可以引入.css文件

解决思路:

1. 在 1.js 文件导入 1.css

2. 在index.js 中引入1.js 文件 
3. 间接的完成导入.css资源的目标



先安装css-loader , 把css代码变成string

style.loader 把string  变成stylev标签 , 插入到DOM 中



# css plugin





# 配置文件



开发一个配置文件 , 生产一个配置文件