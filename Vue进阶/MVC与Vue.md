# V -- 视图对象的功能

- 定义模板字符串
- 模板字符串替换
- 模板字符串渲染
- 插入文档

```js
const V = {
    el: document.querySelector("#app"),
    html:`<p>{{xx}}<p>`,
    render(data,elementX|el) {
        // 模板字符串替换
        newHtml = this.html.replace("{{xx}}",data.xx)
     //下面两行就是渲染代码   
        elementX.innerHTML = ``
        // 这里elementX就是原本就有的DOM元素,下面的代码就相当于文档插入了
        elementX.innerHTML = newHtml
    }
}
```



现在问题的关键就是`elementX`的获取

1. 传统的MVC , 这个`elementX` 就是事先在html里面准备好的 ,是固定死的
2. Vue的解决方式 : 
   1. 虚拟DOM
   2. js对象模拟DOM树 , 数据变化 , 重新渲染js对象 , 对比新旧js对象 , 找到差异
   3. 查到上层节点 , 赋值为 `elementX`

**虚拟DOM保证了这个文档钩节点是可以变化的 , 这就是局部更新的基础**





**Vue的响应式保证了render() 的自动调用**

