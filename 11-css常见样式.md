# 文本样式



## 文本颜色 color

- 设置字体颜色

  - 单词表示法
    - red grey blue pink
  - 「十六进制」表示
    - 格式:`#xxxxxx`   前面两个ff代表red  中间的ff代表green  最后的ff代表blue
    - [示例](http://js.jirengu.com/vatig/1)
    - 共6位 16进制的数 , 每两位代表一个颜色   
      - 三原色 : 「红  第一个ff  绿 : 第二个ff   蓝 : 第三个ff」
      - 需要记忆的颜色
        - red : #ff0000
        - green : #00ff00
        - blue : #0000ff
        - 黑色 : #000000
        - 白色 : #ffffff
        - 特别的 :  如果每位上的值都相等 , 则表示「三原色的比例」相等 , 那么就应该是黑色 , 只不过由于「三原色本身很淡」,所以混出的黑色也会变淡, 也就是「灰度级」

  - rgb(x,y,z)  表示
    - 如: rgb(255.0,0) <==> #ff0000
    - 存在这种等价关系
  - rgba(x,y,z,.w)
    - rgb的一种增强 
    - 带 透明度的 颜色表示
  - 颜色关键字
    - transparent  <==> rgba(0,0,0,0)
      - 作用 :  div + border 制作形状 , 如三角形
      - [制作三角形](http://js.jirengu.com/gecex/1)
    - currentColor 
      - 作用 : 让边框的颜色和文字的颜色一样 , 当改变文本颜色时 , 就不用再去修改边框的额颜色了

  

## 文本大小font-size

- 文本大小的单位
  - px : 像素
  - em :  2em --> 相对于父亲字体大小的倍数（**如果是非font-size属性对应的值，则是相对于当前元素的font-size**)
    - 什么意思? 
    - `p{font-size:2em;padding:1em;}`字体大小和内边距分别是多少像素
      - 字体大小是2em,而padding则是以该元素font-size的值来计算的
  - rem：2rem -->  根元素(html或者:root)字体的倍数
  - 百分比 : 同em
- 注意
  - rem是相对于html的font-size，不是相对于body
  - 浏览器「默认字体大小**16px**」
  - Chrome有最小字体限制(11px or 12px)
- 使用经验
  - em的使用案例
    - 「等比数列」般的效果
    - [示例](http://js.jirengu.com/nanef/1)



## 文本字体font-family 「包含字体图标比较重要」

- 设置字体
  - font-family:'微软雅黑','MicrosoftYaHei','SourceHanSans',Helvetica,Arial,sans-serif;
  - 先找第一个字体，找不到再第二个，依次往后...
  - 字体里包含空白字符最好要加引号（可以不加，但如果字体包含数字和其他特殊字符，一定要加引号)

- 设置自定义字体
  - 英文用多个很多,中文用的少 , 因为中文字体库太大了
  - 少量的文字可以定义自定义字体
  - @font-face 用来设置自定义的字体

```javascript
@font-face {
font-family:"BitstreamVeraSerifBold";
src:url("https://mdn.mozillademos.org/files/2468/VeraSeBd.ttf");
}
body {
    ffont-family:"BitstreamVeraSerifBold",serif
}
```



- 字体图标
  - 文字是被画出来的
  - 「原理」
    - 写代码 的时候 , 用unicode写 如 1102这样的编码
    - 浏览器解析的根据url去找对应的.ttf文件
    - 根据文件 , 由1102(unicode编码) 去找对应的字体/字体图标
    - 浏览器在页面中画出来
  - 使用iconfont网站
    - [unicode使用](https://www.iconfont.cn/help/detail?spm=a313x.7781069.1998910419.d8cf4382a&helptype=code)
    - [unicode案例](http://js.jirengu.com/duwac/1)
    - unicode的优化
      - 文本时unicode  导致可读性不强
      - font-class 方案,提高可读性!
      - [font-class使用](http://js.jirengu.com/jekox/1)
  - 优缺点
    - 优点：矢量放大不失真，可设置颜色，体积小加载快，使用方便
    - 缺点：单色或者CSS3渐变色
  - svg图标
    - [svg用法3](https://www.iconfont.cn/help/detail?spm=a313x.7781069.1998910419.d8cf4382a&helptype=code)



## 文字的精细修饰

### font-style

- 用于设置文字是否以斜体显示
  - normal正常
  - italic斜体
  - oblique模拟斜体

### font-weight

- 用于设置文字粗细
  - normal正常übold粗体
  - lighter比父元素字体「细」一级
  - bolder比父元素字体「粗」一级
  - 100-900用于微调字体粗细
- 注意
  - 设置字体的粗细还取决于字体库是否存在该粗细尺寸的字体的图形描述



### text-decoration

- 设置文字划线样式
  - none取消下划线
  - underline设置下划线
  - overline上划线
  - line-through中划



### text-transform

- 用于改变字母的大小写
  - none取消转换效果
  - uppercase转为大写
  - lowercase转为小写
  - capitalize转为首字母大

### text-shadow

- 设置文字阴影
  - text-shadow: 水平偏移  垂直偏移   模糊半径  颜色;

```html
<p>欢迎来到饥人谷</p>
<h1>多重阴影</h1>
<style>
    p { 
        text-shadow:5px 5px 3px green;
    }
    h1 {
        text-shadow:5px 5px 3px green,
            		2px -3px 4px red,
            		-3px 3px 3px blue;
    }
</style>
```

- ![](assets\text-shadow.jpg)



# vw、vh 与移动端适配

- 长度单位
  - 1vw=视窗宽度的1%，100vw表示水平满屏
  - 1vh=视窗高度的1%，100vh表示垂直满屏
- 兼容性
  - iE9以上可以用
  - caniuse 网站
- 应用
  - 使用vw做移动端的「适配」
    - 适配 : 等比例缩小 



# 块级容器设置行内内容「布局」

- text-align
  - 块级容器设置行内内容（例如文字）的对齐方式
  - left左对齐
  - right右对齐
  - center居中
  - justify文字向两侧对齐，对最后一行无效



## white-space

- 设置如何处理元素中的空白和换行
  - normal连续的空白符会合并，换行符不换行；边界换行。
  - **nowrap**连续的空白符会合并；换行符不换行；保持一行遇到边界不换行。
  - pre连续空白符会保留；换行符会换行；边界不换行。
  - pre-wrap连续空白符会保留；换行符换行；边界换行。
  - pre-line空白符会被合并；换行符换行；边界换行



## 溢出和隐藏

- text-overflow
  - 应用到块级元素上，设置内部文本溢出后的展示样式
  - 不设置，默认溢出会展示
  - `text-overflow:clip;`溢出隐藏切断
  - `text-overflow:ellipsis;`溢出最后展示   「... 」的符号

- overflow
  - 应用到块级元素上，设置如何处理内容太大的场景
  - overflow:visible;默认值。内容不修剪，呈现在元素框之外
  - overflow:hidden;内容被修剪，不出现滚动条
  - overflow:scroll;出现滚动条
  - overflow:auto;不超出时无滚动条，超出出现滚动条

- 注意
  - overflow可以单独设置水平和垂直方向，如overflow-x:scroll;overflow-y:hidden;
  - 使overflow有效果，块级容器必须有一个指定的高度（height或者max-height）或者将white-space设置为nowrap「水平方向上不可以换行,就需要手机使用overflaot」
- [示例:单行文本溢出](http://js.jirengu.com/pugoc/1)