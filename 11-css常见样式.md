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
  - em :  2em --> 相对于父亲字体大小的倍数（如果是非font-size属性对应的值，则是相对于当前元素的font-size)
  - rem：2rem -->  根元素(html或者:root)字体的倍数
  - 百分比 : 同em
- 注意
  - rem是相对于html的font-size，不是相对于body
  - 浏览器「默认字体大小**16px**」
  - Chrome有最小字体限制(11px or 12px)



