# 起步

## `npm`使用

```bash
npm install vue-router
```

如果在一个模块化工程中使用它，必须要通过 `Vue.use()` 明确地安装路由功能：

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)
```



## 导出和导入

- 需要的东西 -- 视图组件
  - 是组件 
  -  未加载时 , 在js原生内存中 , 加载后 , 在jsDOM内存  「这就是实现的原理」
  - 所以称之为视图组件 -- 就是用来切换的  很像是 v-if
- 把路由实例 , 导出到 根实例中

```js
import Vue from "vue";
import App from "./App.vue"; //组件根
import router from "./router/index" // 路由实例
Vue.config.productionTip = false;

new Vue({
  router,
  render: h => h(App)
}).$mount("#app");

```

- 根组件 和 根实例 的作用
  - 根实例是整个js代码的入口
  - 根组件是整个视图的入口

- 规则
  - 如果我写的是js文件 , 那就应该最终导入到根实例
  - 如果我写的是.vue文件 , 视图 , 那就应该导入到App.vue
  - App.vue会导入到实例中 , 最终实现统一的代码入口



## 视图组件注册

- 正常的组件是需要在父组件注册的
- 而视图组件实在 路由实例中注册的
  - 这就是一开始感觉到奇怪的地方
- [代码示例](https://codesandbox.io/s/trusting-tdd-324qe)





# 动态路由

- 多个路径映射到一个视图组件  -- 就i叫做动态路由 -- 通过动态参数实现
- 视图组件的**父组件**  ,  可以用`this.$router` 访问
- 每次点击都会切换一次路径 , 路径的动态参数可以通过$router.params.xx获取
  - `:xx`



## 小结 : 

- 路由表 :  路径 到  视图组件的一个映射

- 常见的映射关系

  - 一对一  : 本质上都是一对一

  - 多对一 : 动态路由 or 404兜底路由 

    - 想要兜底 , 必然是多对一的关系
    - 只有这样才可以匹配所有 , 处理难处理的事

  - 一对多 : ??? 那就是后面要学的嵌套路由

    - 嵌套路由
    - 命名视图

    



# 嵌套路由

格式: 

```js
routes:[
    {
        path:'/user/:id', component: User,
        children:[
            {
            	path: 'profile',
          		component: UserProfile
        	},
            {
                path: 'profile',
          		component: UserProfile
            }          
     	 ]
    }
]
```



核心 : 

1. 如何表示嵌套关系?

当然是对象结构 , 最可以表示这种树的层次东西了   js那边解决了!!!

2. 视图的嵌套如何表示 , 自然是模板的嵌套啦 , 和html逻辑差不多
   1. 视图展示只能有一个入口  ,  换句话说在根组件里面只能有一个<router-view>
   2. 嵌套路由的<router-view> , 写在父视图的`template` 里面





# history对象与编程路由

## history对象

### 用api操作浏览器地址栏的按钮



- 按钮一 :  → 下一个  back()
- 按钮二 : ←  上一个 forward
- 批量处理 : go(n)





### 修改地址栏

- 重命名 -- 不会触发跳转事件  pushState() 

  - 新建一个记录 , 替代地址栏

- ```js
  replaceState()   // 修改记录 , 新建
  ```





## 编程路由

用方法修改地址 , 代替<router-link>









# 命令路由

把路径用对象语法去写

```js
{
    name:xx,
    params:{
        
    }
}
```



# 命名视图

 name属性



# 路由组件传参

配置路径的时候 , 设置props选项

触发时 , 自动传参







# 导航守卫

路由改变之前的一个过滤器

核心 : 

1. 全局 -- index.js  Each
2. routes 数组里面调用  Enter
3. 组件里面调用 `RouterEnter` `RouterXXX`





# 数据获取

页面跳转后 , 从服务器获取数据





# 懒加载

