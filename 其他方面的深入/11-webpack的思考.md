# webpack是什么？

> 是解决写代码到代码上线 ， 这个生产过程中的一系列问题的工具
>
> 有自己的基本功能 ， 更多的功能需要loader+plugins 补充

开发时的问题：

1. 模块化

> ES6的import/exports , 浏览器兼容性不好，需要转译

2. 打包

> 需要把.js文件整合

3. ES6

> ES6语法浏览器的兼容性有问题 , 也需要转译

4. 图片资源的压缩

> 提高性能

5. css资源的处理

> 1. css作为style 由js放到DOM
> 2. css作为html的link , 加入DOM

6. html资源的

> 作为渲染hook
>
> 打包后应该自动生成

7. dist目录

> - dist
>   - index.js
>   - main.css
>   - index.html
>
> 我们希望的 , 打包后的dist目录结构

8. 开发时, 热加载

> 修改代码 , 浏览器实时预览效果

9. 打包前, 自动清空dist文件夹

> 这样dist目录 , 就不会出现多次的打包 , 不需要的代码

10. 配置文件

> 1. 基础公共配置
> 2. 开发时的配置
> 3. 打包时的配置







webpack的基础功能:

- 支持.js文件的导入导出,但是.css文件不支持
- 自带打包功能 
  - 把多层依赖的.js文件 , 打包成一个完整版的js文件
  - 当然只支持js , 不支持html和css以及图片
- 自带打包的代码丑化
- 自带打包文件名的hash







# 工程化

从小作坊 ==> 流水线/工程化



工程化:

1. 自动化    自动完成 , 减少工作人员
2. 模块化    职责分明 , 提高效率
3. 性能优化   ...





## sass 自动化

**步骤 :** 

1. main.css  对应一个 main.sass
2. main.sass 可以写 sass语法
3. 运行命令行 , 转译 , 把sass语法的main.sass文件 , 转译成main.css文件



**常用sass语法:**

1. 嵌套语法

css语法

```css
body>p>span {
    color:red;
}
body {
    font-size:4em;
}
body>p {
    border:1px solid;
}
```

sass语法

```sass
body {
	 font-size:4em;
	>p {
		border:1px solid;
		>span {
		color:red
		}
	}
}
```

2. 自动化

```shell
node-sass src/style.scss dest/style.css -w
```



## Babel 自动化

自动把ES6语法 , 转译成ES3

和sass同样的步骤

1. 转译
2. 自动化





## 为什么前端有大量的自动化工具?

1. 前端东西多

html/css/js 三门语言

img/svg ….等资源

每一种资源都需要处理 , 所以需要很多自动化工具





那么问题来了? 这么多自动化工具如何管理呢?

至此webpack横空出世了!!!



