# 代码风格指南

- 组件名为多个单词:

  - 这样做可以避免跟现有的以及未来的 HTML 元素[相冲突](https://w3c.github.io/webcomponents/spec/custom/#valid-custom-element-name)，因为所有的 HTML 元素名称都是单个单词的。
  - 如:   todo-my 或者`TodoMy`

- 组件的 data 必须是一个函数:

  - ```js
    Vue.component('some-comp', {
      data: function () {
        return {
          foo: 'bar'
        }
      }
    })
    ```

  

