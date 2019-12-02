# css基本语法

```html
<<html>
	<head>
        <style>
            p {
                font-size:20px;
            }
            @mediaprint {
                font-size:60px;
            }
        </style>
	</head>
	<body>
        <p>饥人谷</p>
	</body>
</html>

```



两种语法:

选择器 {

​	key:value;

}

@关键字 {

​	其他

}



# css引入

-   `<link>` 标签

  - `<link rel="stylesheet" href="xxx.css">`

- 通过CSS语法@import引入

  - `inport ”xxx.css“;`

- 内部样式  `<style> ` 标签

  - `

    <style>
        body {
          font-size:22px;
        }
      </style>

    `

- 内联样式 style

  - `<body style="font-size:22px；"></body>`





# link方式和@import引入CSS有什么区别？

- `<link>`是HTML的==标签==，在页面上代表一个元素，==可以被JS选中修改==；@import是CSS的语法，JS无法操控
- 两个link标签，文件会并行加载。一个link的CSS里包含一个@import，文件==串行加载会更慢==。因此不推荐用



# css选择器

## 常见选择器

### 元素选择器

- 标签选择器
  - `p { color:red }`
  - 选中 p 标签 , 上红色

### ID选择器

- id选择器
  - 精确度最高 , 不容易出粗 , 但是用来很麻烦 
  - 一般情况下 , 使用 class类选择器

### class类选择器

- 用的对多
- `class="xx yy zz qq ..."`

### 通用选择器

- 选中文档中的所有元素
  - `* { margin:0px; padding:0px; }`
  - 用法二 `.box * { color:red;}`
    - 这是选中.box的所有后代
    - 注意 box和* 之间 有一个 空格

## 属性选择器

- 如 [id=“xx”] 
  - 这是符合 elemt语法的 , 很容易理解
- [attr=val]   
  - 仅选择attr属性被赋值为val的所有元素
- 属性选择器用的比较少



**不很常用的属性选择器**:

- [attr~=val]
  - 仅选择attr属性的值（以空格间隔出多个值）中有包含val值的所有元素，比如位于被空格分隔的多个类（class）中的一个类
  - 如 [class~="password"]
    - 只要class类型中有password就选择
      - 如 class=“xx yy zz  password” 都可以选
    -  和[class=“password”]  不同
      - 这个只能选择  class=“password”的元素
- [attr*=val]
  - 选择attr属性的值中包含字符串val的元素
- [attr^=val]
  - 选择attr属性的值以val开头（包括val）的元素
- [attr$=val]
  - 选择attr属性的值以val结尾（包括val）的元素
- [attr|=val]
  - 选择attr属性的值是val或以val-开头的元素（-用来处理语言编码）



## 组合选择器

- A,B
  - 一口气选择两个
- A  B 「中间有空格」
  - A后代中的B
- AB
  - 交集 , 满足条件A的情况下,又满足条件B的元素
  - `p.para1{
        color: red;
      }`
  - class属性为paral,并且元素类型是 p 的元素
  - [效果](http://js.jirengu.com/quval/1)
- A>B
  - A直接子元素B
- A+B
  - A的**下一个相邻**的B
- A~B
  - A后面的 全部相邻元素



# 伪元素和伪类

## 伪类

- :pseudo-class-name  「伪装-类-名」
  - 伪类是一个选择器
  - 类似于给元素加了一个class
    - 选中元素 , 添加class
- 作为孩子 childeren
  - first-child
  - last-child
  - nth-child(n)  
    - 第n个(nth) 个孩子
    - even  | odd  等价于下面
    - 2n  |  2n+1

- 作为自己父亲**当前标签类型**的第x个孩子
  - [示例](http://js.jirengu.com/nupes/1)
  - `.box :first-of-type{
      color:red;
    }`
  - div下的 每种类型的的第一个元素  
    - 也就是 h2 类型
    - p类型的第一个

 

## a链接相关伪类

- :link
  - 选中为访问的
- :hover
  - 选中鼠标放置的
- :active
  - 选中鼠标按下未松开的
- :visited
  - 选中访问过的链接
- 顺序
  - 爱恨原则
  - LoVe  HAte
  - :link-:visited-:hover-:active
- 开发中
  - 常用的 
  -  a {color:xxx }
  - a:hover {color:yyy}

## 更多伪类

- :checked
  - 选中被勾选的表单元素
  - `input:checked { color:xxx }`
- :disabled
  - 选中被禁用的表单元素
- :focus
  - 选中被激活的表单元素
- :root
  - 选中根节点 `<html>`



## 不常用的伪类

- :target
  - 选中页面上id为当前hash的元素
  - `#tab1`就是hash
  - [css实现tab栏1](http://js.jirengu.com/dozit/1)
- [css实现tab2](http://js.jirengu.com/qiquv/1)
  - 利用 radio 和 :checked 实现 
- :target 实现弹窗功能
  - [弹窗](http://js.jirengu.com/neyul)

- :not(xx)
  - `.box:not(.active) { color:red;}`





# 伪元素

- ::pseudo-element-name 「伪造-元素-名」
  - 其作用相当于**给元素加了个标签** , 而不是和伪类一样加了一个class
  - 注意: 四个点

## 伪元素类型

- ::before 、::after
  - 在元素内插入一段内容 , 作为元素的「第一个/最后一个」孩子
  - 必须有content 属性
  - 常用来替代 图标、无实际意义的标签
- ::first-line
  - 选中段落的第一行
- ::first-letter
  - 选中段落的第一个字符
  - 通常是第一个字符巨大
- ::selection
  - 匹配被鼠标选中的文字内容
  - 一般让鼠标选中的内容变色 , 反正就是设置样式 , 突出出来





