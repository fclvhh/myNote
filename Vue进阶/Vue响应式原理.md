# Vue的响应式原理

## 概述

> Vue实例创建 , 会将data选项中的模板数据 , 加入到Vue的响应式系统中
>
> 一旦 , 数据发生改变 , 视图也会跟着改变

所谓响应式 : 数据驱动视图的单向流动



## 问题

1. data里面的数据改变 , Vue内部是如何监听数据改变的
2. Vue监听到数据改变 , 又是如何知道要通知哪些元素 , 页面发生刷新



## 监听对象属性

> Object.defineProperty() api 监听数据改变

```js
var app = new Vue({
    el:"#app",
    data:{
        msg:"hah"
    }
})
// Object.keys: 返回一个数组 ,value为对象的属性名
Object.keys(app).forEach(key=>{
    let value = app[key] // obj[属性名] 拿到属性值
    // Object.defineProperty  定义属性的api
    // 可以重写对象 , 完成对属性get/set 重写
    Object.defineProperty(app,key,{
        set(){
            // 这里可以监听数值的改变
          console.log('数值变了')
        },
        get(){
            
        }
    })
})
```



## 通知订阅元素

```js
 Object.defineProperty(app,key,{
        set(){
            // 这里可以监听数值的改变
            // 根据解析html代码 , 获取到哪些人在使用当前属性
          console.log('数值变了')
        },
        get(){
            // 谁使用过这个属性 , 就会在这里调用过get
            // 这里就记录了谁使用过当前元素
        }
    })
```





# 关于对象的getter/setter

## 起步

```js
let obj0 = {
    姓:'高',
    名:'圆圆',
    age:18
}
```



## 需求一:得到姓名

```js
let obj1 = {
  姓: "高",
  名: "圆圆",
  姓名() {
    return this.姓 + this.名;
  },
  age: 18
}
console.log("需求一：" + obj1.姓名());
```



利用了一个函数, 进行字符串拼接 , 返回姓名



## 需求二 : 去掉括号?

```js
let obj2 = {
  姓: "高",
  名: "圆圆",
  get 姓名() {
    return this.姓 + this.名;
  },
  age: 18
};

console.log("需求二：" + obj2.姓名);
```



## getter/setter的真相

> 他们相当于关键字 ,  作为属性名的前缀 ,  说明这个属性并不是真是的对象属性
>
> 而是虚拟存在的!

语法:

```js
var obj = {
    get xxx() {
        
    },
    set xxx() {
        
    }
}
```

1. 给obj对象 , 添加了一个"假" 属性xxx
2. obj.xxx  读属性 , 就会调用 get() 
3. obj.xxx = value  写属性 , 就会调用set(value)
4. 但是调用者是不知道的 , 还天真的以为xxx 真的是obj的属性



> **为什么要这么设计?**

就以 `姓+名 ` 为例

1. 内存里明明都有数据了 , 难道还要单独开辟一块空间存储重复数据??

   **明明拿出来拼接一下就好了** 

2. 我又写了一个函数作为属性 , 不是两全其美吗?  

   函数就不占内寸吗? 函数是一个对象啊 , 也是真实的属性啊!

3. js发明语法 get/set  搞一个假属性 ,  不久节省内存 , 提升效率了嘛!!!





## 需求三:写属性

```js
let obj3 = {
  姓: "高",
  名: "圆圆",
  get 姓名() {
    return this.姓 + this.名;
  },
  set 姓名(xxx){
    this.姓 = xxx[0]
    this.名 = xxx.slice(1)
  },
  age: 18
};
```







# 关于Object.defineProperty

## 介绍

> 可以给对象添加属性

对 , 这个属性是用getter/setter 添加的假的属性



## 需求一:定义属性n

```js
let data1 = {}

Object.defineProperty(data1, 'n', {
  value: 0
})
```



## 需求二：n 不能小于 0

```js
let data2 = {}

data2._n = 0 // _n 用来偷偷存储 n 的值

Object.defineProperty(data2, 'n', {
  get(){
    return this._n
  },
  set(value){
    if(value < 0) return
    this._n = value
  }
})
```



## 为什么要有一个_n?

> 结合前面getter/setter  和 姓+名

假属性**姓名**的前提 , 是内存中有真实的属性 姓+名

所以这个_n就是真属性 , 是假属性存在的前提啊 , 提供真实的内存



# 使用代理

```js
let data3 = proxy({ data:{n:0} }) // 括号里是匿名对象，无法访问

function proxy({data}/* 解构赋值，别TM老问 */){
  const obj = {}
  // 这里的 'n' 写死了，理论上应该遍历 data 的所有 key，这里做了简化
  // 因为我怕你们看不懂
  Object.defineProperty(obj, 'n', { 
    get(){
      return data.n
    },
    set(value){
      if(value<0)return
      data.n = value
    }
  })
  return obj // obj 就是代理
}
```



语法:

```js
let p = new Proxy(target, handler);
```

参数:

1. traget 目标对象 (被代理对象)
2. handle  代理的操作「会被反射到被代理对象」

返回值 : 

- 返回一个代理对象 





## 绕过代理

```js
let myData = {n:0}
let data4 = proxy({ data:myData }) // 括号里是匿名对象，无法访问

// data3 就是 obj
console.log(`杠精：${data4.n}`)
myData.n = -1
console.log(`杠精：${data4.n}，设置为 -1 失败了吗！？`)
```



匿名对象虽然访问不到,但是可以是声明一个匿名对象的引用



说人话: 就是房东是谁 , 中介不告诉我 , 但是我可以知道 , 房东的电话号码 , 私下联系



## 代理与被代理者

```js
let myData5 = {n:0}
let data5 = proxy2({ data:myData5 }) // 括号里是匿名对象，无法访问

function proxy2({data}/* 解构赋值，别TM老问 */){
  // 这里的 'n' 写死了，理论上应该遍历 data 的所有 key，这里做了简化
  // 因为我怕你们看不懂
  let value = data.n
  delete data.n  //删除原始的n
  Object.defineProperty(data, 'n', {
    get(){
      return value
    },
    set(newValue){
      if(newValue<0)return
      value = newValue
    }
  })
  // 就加了上面几句，这几句话会监听 data

  const obj = {}
  Object.defineProperty(obj, 'n', {
    get(){
      return data.n
    },
    set(value){
      if(value<0)return//这句话多余了
      data.n = value
    }
  })
  
  return obj // obj 就是代理
}
```



基本思路:

1. 在代理的handle 对对象的属性进行劫持 

```js
let value = data.n //n属性被劫持了
delete data.n  //删除原始的属性n
```

2. 后面对代理对象的任何属性操作 , 都操作的是劫持的value 

```js
 Object.defineProperty(data,"n",{get:xxx,set:xxx})
// 之前删除属性n , 现在重新新增 假属性n
// 操作value , 就很好的进行了模拟
```

3. proxy2的最后返回一个obj

```js
return obj  //这个obj就是代理对象
```

步骤

> 1. 把被代理对象的属性值记录一下
> 2. 删除被代理对象的属性 , 然后defineProperty()重新安装一个新的属性
> 3. 完成对被代理对象的监听后 , 再返回代理对象



## Vue的代码

```js
const vm = new Vue({data:{n:xx}})
```

现在来看:

- vm:代理对象
- new Vue() 就是代理函数
- {n:xx} 就是被代理对象



现在思路就明确了





# 代理的好处

[案例](http://js.jirengu.com/nonar/2)

发现:

没有放在data模板数据里的东西 , 是无法做到响应式的

需要给age初始化

```js
data:null
```

[代码](http://js.jirengu.com/nonar/2)



proxy()是比Object.defineProperty还要强的api





proxy()api的好处:

- 根据调用自动创建内存的代理结构

```js
let obj = proxy({},{
    xxx
})
obj.a.b.c.d.f = "哈哈"

// Proxy ,会自动创建代理的嵌套 , 模拟出代码的内存结构 , 不报错
```

- 这个可以让proxy在不知到属性名key的情况下 ,也可以代理
- 而对象的api必须知道属性名  , 才可以代理





# vue的bug

Vue的响应式bug

其实根源还是Obeject.defineProperty(obj,‘x’,{})

要求必须传一个属性名



解决bug:

1. data模板初始化的时候 , 忘记传key , 后来添加无法响应式
2. Vue提供了一个api– vue.set()

```js
Vue.set(this.obj,'key',值)
```

```js
this.$set(this.obj,'key',值)
```



Vue.set和this.$set的作用:

1. 新增key
2. 自动创建代理和监听(之前没有创建过)
3. 触发ui更新(但不会立即更新)



# Vue数组bug

> 数组对象的怎么添加key呢？



## 数组变异

[参考](https://cn.vuejs.org/v2/guide/list.html#%E5%8F%98%E5%BC%82%E6%96%B9%E6%B3%95-mutation-method)

思想：

1. 给Array原型外面包一层代理VueArray
2. 在VueArray，内部监听数组变化 ， 调用Vue.set()

伪代码

```js
class VueArray extend Array {
    push(...args) {
        const oldLenth = this.length
        super.push(...args) //调用父类Array上的push方法
        const newLength = this.length
        for(let i = oldLength;i<newLength;i++) {
            Vue.set(this,i,this[i]) //实现数据响应式
        }
    }
}
```







# 伪代码来实现一下

