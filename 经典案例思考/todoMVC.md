# 说明

这是学习web技术的经典案例 , 因为项目虽小 , 但是五脏俱全

是一个综合性很高的入手项目!!!



# 基本样式切换

从功能上去分析 : 

1. 展示用户指定的计划
2. 计划 就理应有两种状态
   1. 未完成
   2. 已完成

这两种状态完全就可以用一个字段来标识  「completed」

同理 , 也可以用一个class 来切换样式   「.completed」



# 基本功能设计

不变的逻辑 : 

获取ui数据 == > ui数据持久化 ==> 读取数据 ==>展示数据



## 用户输入功能

> 用一个input 标签让用户输入 , 核心 : 获取到ui数据

初始 : 

​	没有数据 , 因为没有用户输入 , 那么就没有展示数据的必要 , 因此要把展示数据结构隐	藏

用户输入后 : 

​	获取用户ui数据 , 此时展示数据功能就应该被激活 , 因此结构应该显示

用户数据可能不止一个 : 

​	因此需要暂存一下数据 , 方便访问



输入功能就有**两个状态 :** 

1. 一个都没有的 --- 空状态
2. 不少于一个的状态  ---不空状态



**如何标识这两种状态呢?**

通过 arr.length 来标识

- arr.length === 0  ==>说明没有获取 ui数据 , 状态自然为空
- else  ==>说明获取到了ui数据 , 状态自然非空



**如何选择激活/隐藏数据展示功能?**

这两种状态是跟着 ==空与非空==  走的 , 所以标识一样 , 

增强功能就好





## ui数据持久化功能

套路 :  利用localStorage对象

原理 : 是一个非关系数据库 , 一个树结构

localStorage的一个属性 key 对应一个数据库 , key对应的值就是一张表

应用场景 :  对页面少量ui数据的记录 , 减少对用户的干扰



写代码思路 : 

1. 创建一个数据库api对象
2.  向外暴露两个api : fetch() and save()
3. watch  deep 深度监听 , 数据改变了 就调用save ,进行持久化存储



## 数据读取功能

从数据库读数据到Vue的data缓存 , 同时加入响应式

读取后 , 交给Vue托管



## 数据展示功能

根据数据的字段和结构 : 

​	用js写html , 模板字符串替换将数据展示到页面





# 伪代码的简单实现

## 获取ui数据

实现用户输入功能:

```html
<input type="text" id="input">
```

```js
$("#input").on("input",function(e){
    // 拿到用户数据
    e.target.value 
    // 把数据暂存 到一个数组里面
    data.arr.push()
})
```



空与非空的状态切换

```js
if(data.arr.length===0) {
    // 说明空状态
    隐藏展示结构 :  display:none;
}else{
    显示结果 : display:block;
}
```



## ui数据持久化

前面我们仅仅把ui数据 , 暂时存储在**内存**中 , 接下来应该把数据存储到**文件**中

套路 : 

1. 创建数据库

```js
const uiDataDb_KEY = "uiData"
const uiDataDb = {
    fetch(uiDataDb_KEY) {
        return JSON.parse(localStorage.getItem(uiDataDb_KEY)||"[]")
    },
    save(val) {
        localStorage.setItem(uiDataDb_KEY,JSON.stringify(val))
    }
}
```

2. 监听数据是否变化

```js
watch:{
    data.arr:{
        deep:true,
        handle:function(val,odd) {
            uiDataDb.save(val)
        }
    }
}
```

3. 缓存中的数据应该从uiDataDb中读取 , 因为ui数据已经持久化在数据了

```js
data.arr = uiDataDb.fetch()
```





## 缓存对象的结构设计

现在数据拿到了 , 并且缓存好了 , 也持久化存储好了

接下来就是展示数据了



展**示数据 , 第一个要考虑的就是数据的状态 , 根据数据状态的不同 , 选择不同的展示效果**



前面分析了 , 数据应该有两种状态: , 应该用一个字段来标识 



因此数据的结构设计也出来了



```js
{
    id: x; //数据的唯一标识字段
    content: //数据的内容
    conpleted://标识数据的状态
}
```





## 展示功能的实现

### 完成和未完成 , 切换class

伪代码实现

```js
if(data.arr[index].completed) {
    return
}else{
    $(".ul").find(".li").eq(index).
    	addClass(completed).siblings().removeClass(completed)
}
```

关键在于获取index

1. 事件委托方案 , 获取index

```js
$(".ul").on('keyup-enter",function(e){
        $(".ul").children().forEach((item,index)=>{
    		if(item = e.target){
                return index
            }
		})  
   })
```

2. Vue实现 v-for

```html
v-for = "(item,index) in arr"
```

```js
arr[index].completed //直接拿到值
```



### 增加`ui`数据

1. 用户输入加工成设计好的数据结构

```js
e.target.value
arr.push({
    id:arr.length++,
    content:value,
    conpleted:false
})
```



### 修改`ui`数据

1. 用户双击 , 切换成编辑状态 , 添加.edit class
2. 获取index , arr[index].content = e.target.value
3. 监听keyup-enter, 退出编辑状态

伪代码实现

这里采用 "上课逻辑 " ,代码写了太多遍!!!

```html
<input type="text" v-model="arr" v-for="(item,index) in arr">
```



### 删除ui数据

都到这里了 , 实现起来很简单 , 不用写了





# 用户体验优化

## 用户输入优化

1. 去除空格  trim()
2. 用户输入完毕
   1. keyup-enter : 常见的
   2. 失去焦点事件 :  也应该实现



## 用户修改优化

当用户想修改事件 , 光标应该自动获取且对焦



## 用户提示优化

应该提示用户 , 计划的完成情况

1. 已完成统计
2. 未完成统计



## 批量删除优化

1. 提供全选按钮

   1. 这个功能挺复杂的
   2. 当点击按钮的时候 , 要修改所有数据的状态
   3. 当检测到所有的数据状态都改变时 ,  自己的状态也应该跟着改变

   换句话说 : 全局状态 和 局部状态 , 存在一个双向绑定的关系

2. 提供清除已完成的按钮

