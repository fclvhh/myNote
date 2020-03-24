# 核心概念

## state

- 单一状态数
- 所有的状态 「共享数据」 , 都放在一个state属性里面



### 避免多次注入

- 把store.js实例 , 注入到根实例
- 各个组件通过`this.$store.state.xxx` 访问



### 访问状态

- 组件常采用计算属性来访问state



### `mapState` 辅助函数

当一个组件需要获取多个状态时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性，让你少按几次键

- 一个组件使用多个state

- 严格来说就是语法糖

常见的用法

1. 对象语法

```js
computed: mapState({
    // 让 this.count 代替 this.$store.state.count
    count(state) {
      console.log(state)
      return state.count
    },
    // 把上面的函数用这个 别名
    countAlias: 'count',
    // 真正的计算属性
    andCounter() {
       return this.localCount+this.count
    }
  })
}
```



2. 数组语法

```js
computed: {
  ...mapState([
      'a', 
      'b'
  ]),
}
```

等价于

```js
computed: {
  a() {
    return this.$store.state.a
  },
  b() {
    return this.$store.state.b
  },
}
```



3. 复合语法

```js
computed : {
    //[]语法自动转化  this.xx ==>this.$store.state.xx
    ...mapState({
        // alias : "xx"  其别名
        leftFocus:'leftFocus',
        loadingNum:'loadingNum',
        hasBg:'hasBg'
    }),
     // 真正的计算属性
    fn1(){ return ......},
}
```

对象语法三步走一部也没少

只是第一步换了 数组语法实现



## Getter

- 多个组件 , 共用一个state 「或这个state相关的计算属性」
- `this.$store.getter.xxx`  来调用计算属性



余下的内容和state思路相似 , 自己看代码



## Mutation

- Mutation的机制 , 就类似于事件的机制
- Mutation 不支持异步调用

eg1

```txt
触发一个"click"的事件
触发一个 "increment"de mutation
```

这些说法是不是很类似!!!



eg2 : mutation的机制

```txt
事件-->mutation-->commit
```



eg3 : commit() 就是 emit()  , 效果一毛一样

```js
increment () {
      this.$store.commit('increment')
      
},
    
addOne() {
    this.$emit("add",e)
}
```





### mutation的流程

1. 监听mutation,绑定`mutation`的回调

```js
mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state) {
      // mutation回调代码
      	 // 修改state
    }
  }
```

上述代码: 监听了一个叫做SOME_MUTATION的mutation

2. 触发mutation

首先绑定事件和事件回调

```html
 <button @click="increment">+</button>

```

在事件回调中 , 触发mutation 

```js
 methods: {
     //
    increment () {
       //
      this.$store.commit('increment')
      
    }
 }
```

这个流程和自定义事件一毛一样



### mutation的语法糖

- 每次commit前都要写`this.$store​.commit("xxx")` 很烦人
- 语法糖 : `this.increment()`

mapMutation

```js
 methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
```



## Action

- Acion 很类似于异步
- 可以包含任意异步操作
  - action可以暂存异步操作
  - 异步返回后结果 , 提交到Mutation
  - 不是直接修改状态



### 机制

和Matution机制类似 

```js 
事件--action--dispatch--(mutation--commit)--修改state
```

- 事件的回调提供动力 , 用来触发自定义action/mutation
  - 触发action的函数为 dispatch()
  - 触发mutation的函数为 commit()
- 逻辑 : 
  - 在回调前 , 获取动力 
  - 回调时 , 将动力传给下个流程

### 流程分析

代码片段1: 绑定事件 

```html
 <button @click="increment">+</button>
```

代码片段2 : 触发increment action

```js
methods:{
    increment() {
        this.$store.dispatch("increment")
    }
}
```

代码片段3 : 监听increment的action , 并且绑定回调,在回调中触发 mutation

```js
actions: {
    increment (context) {
        //触发mutation
      context.commit('increment')
    }
  }
```

1. 在actions监听了一个 increment action
2. 这个action的回调中 , 触发了 increment mutation

代码片段4: 修改state

```js
 mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
 }
```

实际修改了state     



### 语法糖

- 参数解构

```js
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

简化前

```js
actions: {
    increment (context) {
      context.commit('increment')
    }
  }
```

1. context是一个对象 , 但是不是store实例本身



- 分发简化

前

```js
 this.$store.dispatch("increment")
```

后

```js
this.increment()  自动分发
```





## Module

- 防止$store对象 , 由于项目复杂 , 造成属性过多 , 显得臃肿 , 难以维护
- module : 模块化思想 , 分割store

```js
const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```



