[tabber小项目](#tabber)  
[promise简单用法](#promise)  
[Vuex](#Vuex概念)    

#### tabber

**#模块化思路**
1. 建立一个 TabBar 组件，并以 TabBarItem 组件作为它的子组件。  
2. 在 TabBar 组件中使用插槽，使外界可以动态插入任意数量的 TabBarItem 组件。  
3. 在 TabBarItem 组件中，使用图片插槽和文字插槽。  
  
:palm_tree: 组件自身相关的 HTML 和样式写到自己内部，然后引入根组件注册并使用。  
:palm_tree: 添加插槽是为了是让外界动态决定插入的内容，有利于组件的复用。    
:palm_tree: 注意 TabBarItem 中根容器即为弹性项目，即`class="tab-bar-item"`。

- src
  + assets
    - css
      + base.css
    - img
      + tabbar
  + components
    - tabbar
      + TabBar.vue
      + TabBarItem.vue
  + views
    - cart
    - category
      + Category.vue
    - home
      + Home.vue
    - profile

文件 | 说明 
 :-: | :-: 
 components | 放公共、功能性组件的文件
 views | 放独立组件的文件
 home | 文件夹用小写命名
 Home.vue | vue 件用大写命名


**#样式**

1. 初始化的样式很多（这里初始化内外边距），可以写到 `base.css` 中，然后引入 `App.vue`  
```
/* App.vue */

<style>
  @import "./assets/css/base.css"; 
</style>
```

2. 水平方向平均分布，固定底部，阴影效果，控制弹性项目中图片和文字的布局。

  - 容器框类型改为 `flex`；背景#f6f6f6；定位为 `fixed`，且左右底为0；`box-shadow: 0 -1px 1px rgba(100,100,100,.1)`
  - 弹性项目添加样式 `flex: 1` ；居中；高度49px

:palm_tree: `tab-bar-item` 的高度为 49px，是最佳实践。  
:palm_tree: `fixed` 定位时左右为 0 ，能保证占据全部宽度。  
:palm_tree: `<img>` 底部默认有 3 像素，`vertical-align: middle` 可以将其去除。  
:palm_tree: 命名类名时，与组件标签一致，都是用 `-` 连接。  

----

#### 传入active图片  

在 TabBarItem 组件中新增一个具名插槽 `name="item-icon-active"` 用于放入活跃状态的图片。   

然后结合[条件渲染](https://github.com/SpringLoach/Vue/blob/main/learning/section1.md#条件渲染)决定渲染哪张图片。  

:palm_tree: 插槽被替换时，属性也会被替换掉。可以将插槽包含在一个 `<div>` 内部，根据需要对 `<div>` 添加属性和类。  

----

#### 结合路由进行页面跳转

- 在创建好组件后，进行[路由映射配置](https://github.com/SpringLoach/Vue/blob/main/learning/section2.md#路由映射配置)以及[路由的默认值和模式修改](https://github.com/SpringLoach/Vue/blob/main/learning/section2.md#路由的默认值和模式修改)，而且使用的是[路由懒加载](https://github.com/SpringLoach/Vue/blob/main/learning/section2.md#路由懒加载)的方式。
- 在 `TabBarItem` 组件中进行[父传子](https://github.com/SpringLoach/Vue/blob/main/learning/section1.md#父子组件通信)来获取自定义属性 `path`，最后[通过代码跳转路由](https://github.com/SpringLoach/Vue/blob/main/learning/section2.md#通过代码跳转路由)。  
- 在 `App.vue` 中插入 `<router-view>`，以确定映射的组件显示的位置。

:bug: 通过代码跳转路由步骤，在 `this.$router.replace(this.path)` 后添加 `.catch(err => {})` 可以解决报错问题，但是否真的解决了呢？

----

#### 颜色动态控制  

1. 通过判断活跃路由是否为当前路由，选择对变量的赋值，从而进行条件渲染。  
2. 对于字体颜色，我们希望可以让外界动态决定，故需要设置自定义属性（用于获取颜色）并动态绑定样式。同时我们希望给它一个默认值，并添加到自定义属性中。  
3. 添加计算属性，通过判断活跃路由是否为当前路由，决定添加到[动态样式](https://github.com/SpringLoach/Vue/blob/main/learning/section1.md#绑定style)的内容。

----

#### 进一步抽离 

- src
  + components
    - MainTabBar.vue

1. 将 `App.vue` 当中 `TabBar` 模板和 `TabBarItem` 模板的内容转移至 `MainTabBar`。  
2. 在 `MainTabBar` 中注册 `TabBar` 组件和 `TabBarItem` 组件。  
3. 在 `App.vue` 中注册并使用 `MainTabBar` 组件。
4. 改写一下图片的路径。  

----

#### 起别名  

#cli-2

`项目文件/build/webpack.base.conf.js` 中的有一个 `resolve` 对象，其中的 `alas` 属性为别名。  
```
resolve: {
    ...,
    alias: {
        '@': resolve('src'),
        'asserts': resolve('@/asserts'),
        'components': resolve('@/components'),
        'views': resolve('@/views'),
    }
}
```

使用别名
```
/* src中使用需要前缀~ */
src="~assets/img/tabber/home.svg"

/* import则不用 */
import TabBar from 'components/tabbar/TabBar'
```
:bug: 实际测试时，除了默认配置的 `@`，其它都不能用。  

----

#### promise
> 一般情况下有连续的异步操作时，使用 promise 对其进行封装，这样写更加优雅。  

1. 实例化 `promise` 类时，需要传入一个函数，这个函数有两个可选的参数 `resolve` 和 `reject`，它们是第二步的前提。  
```
new Promise((resolve,reject) => {
  
)
```


2. 在这个函数中调用 `resolve()` 或 `reject()` 表示期约的完成（解决或拒绝）。不调用的话，就是等待状态了。  
3. 这里用 `setTimeout()` 模仿完成期约所需要的些许时间。  
4. `then()` 方法将在期约完成时被调用，该方法最多可接受两个函数作为参数。  
```
new Promise((resolve,reject) => {
  setTimeout(() => {
    console.log('Hello World!');
    resolve();
    }, 2000);
  }
).then(()=>{
    setTimeout(() => {
    console.log('Hello Vue!');
    }, 2000);
})
```
4. 可以将获取的数据作为 `resolve()` 的参数，它将作为参数传递给 `then()`方法。  
```
new Promise((resolve,reject) => {
  setTimeout(() => {
    console.log('Hello World!');
    resolve('Hello Vue!');
    }, 2000);
  }
).then((data)=>{
    setTimeout(() => {
    console.log(data);
    }, 2000);
})
```
5. 链式传递，在 `Promise`的方法中获取数据；在 `then()` 中处理数据，有需要则再调用一次期约。  
6. 之所以能对 `then()` 调用 `then()`，是因为它自身返回的也是一个期约的完成。  
```
new Promise((resolve, reject) => {
  console.log('Hello World!');
  setTimeout(() => {
    resolve(1);
    }, 2000);

}).then((data)=>{
  console.log(data);
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(data);
    }, 2000);
  })
  
}).then((data)=> {
  console.log(data+1)
})
```
7. `then()` 方法中使用 `return` 时，某些返回值相当于解决了期约。  
```
new Promise((resolve, reject) => {
  console.log('Hello World!');
  setTimeout(() => {
    resolve(1);
    }, 2000);

}).then((data)=>{
  console.log(data);
  return data
  
}).then((data)=> {
  console.log(data+1)
})
```
8. 设置解决和拒绝处理程序的一个方法，`then()` 会将接受到的拒绝期约返回出去。  
```
new Promise((resolve, reject) => {
  console.log('Hello World!');
  reject('problem');
  
}).then((data)=>{
  console.log(data);

}).catch((err) => {
    console.log(err);
})
```
**#取得两个请求的结果后进行一些操作**
> `Promise.all()`方法接受一个可迭代对象，其中可包括多个期约实例，待所有期约解决后，返回一个新期约，并将可迭代对象中期约的解决值作为数组参数传过去。  

```
Promise.all([
    new Promise((solve) => {
      solve(2);
    }),
    new Promise((solve) => {
      solve(1);
    })
]).then((solves) => {
    console.log(solves[0]+solves[1]);
})
```

----

#### Vuex概念

- Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**，采用**集中式存储管理**应用的所有组件的状态。   

- 当多个组件、或嵌套很深的组件需要读写某些状态（用变量保存）时，就可以使用 Vuex 来管理这些状态（响应式）。

:herb: 将属性添加到对象原型上，实际上是共享的，但因为一些原因，无法做到响应式。  

单界面的用户管理

![单界面的用户管理](./img/单界面的用户管理.jpg)  

多界面的用户管理

![多界面的用户管理](./img/多界面的用户管理.png)  

:palm_tree: Dispatch：派遣  
:palm_tree: Actions：执行一些异步操作（如发送网络请求）时，需要这一步    
:palm_tree: Backend：后端    
:palm_tree: Devtools：Vue 开发的一个浏览器插件（需安装）。记录（同步的）修改，方便跟踪  
:palm_tree: Mutate：改变    

**安装并配置Vuex**
```
npm install vuex --save
```

- src
  + store
    - index.js 

```
/* store 下的 index.js */
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  
})

export default store
```
:palm_tree: 单一状态树：将状态信息全部保存在一个 Store 对象中，便于后期的管理和维护。

```
/* src 下的 main.js */
import store from './store'

new Vue({
  ..,
  store
})
```
:palm_tree: 在 Vue 的原型上添加了 `$store`。

```
/* store 下的 index.js */
const store = new Vuex.Store({
  state: {},                // 保存的状态其它界面能共享
  mutations: {},           
  actions: {},              // 异步操作
  getters: {},              // 类似于计算属性
  modules: {}
})
```

**state属性**
```
/* store 下的 index.js */
state: {counter: 101}, 
...

/* App.vue */
<h2>{{$store.state.counter}}</h2>
```
:herb: 任何组件都可以读取该状态，但是不建议直接 `$store.state.counter++` 来修改状态，这样的修改没有记录。  

**mutations属性**
> 用于同步操作中更新 `state` 的状态，默认参数为 state 对象。

```
/* store 下的 index.js */
mutations: {
  increment(state){
    state.counter++
  },
  addCount(state, count){
    state.counter += count
  }
},
...

/* App.vue */
<button @click="addition">+</button>
<button @click="addCount(5)">+5</button>

methods: {
  addition(){
    this.$store.commit('increment');
  },
  addCount(count){
    this.$store.commit('addCount', count);
  }
}
```
:snowflake: 一般认为 `mutations` 中包括事件类型和回调函数两部分。  
:snowflake: 在 `this.$store.commit()`中，第一个参数为事件类型，第二个参数为载荷。  
:palm_tree: Payload：载荷，此处为传递的参数，当参数不止一个时，可以将 Payload 以对象的形式传递。  

**mutations提交风格**  
> 实际上，`this.$store.commit()` 还有其它的提交方式。  

```
mutations: {
  addCount(state, count){
    state.counter += count
  },
  addCount2(state, playload){
    state.counter += playload.count
  }
},
...

/* App.vue */

<button @click="addCount(5)">+5</button>
<button @click="addCount2(5)">+5</button>

methods: {
  addCount(count){
    this.$store.commit('addCount', count);
  },
  addCount2(count){
    this.$store.commit({
      type: 'addCount2',
      count
    })
  }
}
```
:snowflake: 以第二种风格提交的参数始终是一个对象。

**getters属性**  
> 多个页面需要获取处理后的状态时使用，第一个参数为 state 对象，第二个参数为 getters 对象。

```
/* store 下的 index.js */
getters: {
  powerCounter(state){
    return state.counter * state.counter
  },
  powerCounteradd(state, getters){
    return getters.powerCounter + 1
  },
  powerCounterAnyadd(state, getters){
    return num => {
      return getters.powerCounter + num
    }
  }
},
...

/* App.vue */
<h2>{{$store.getters.powerCounter}}</h2>
<h2>{{$store.getters.powerCounteradd}}</h2>
<h2>{{$store.getters.powerCounterAnyadd(666)}}</h2>
```
:snowflake: 当某些处理需要传参才能进行时，需要在定义的方法中返回一个函数，用这个函数去接受参数并进行处理。  

#### Mutation相应规则   
> 需要提前在 store 中初始化所需的属性。  

```
state: {
  info: {
    name: 'Kaerx',
    age: 400
  }
},
mutations: {
  updateInfo(state){
    /①/  state.info.age = 14
    /②/ state.info['sex'] = 'girl'
    /③/ Vue.set(state.info, 'sex', 'girl')
    /④/ delete state.info.age
    /⑤/ Vue.delete(state.info, 'age')
  }
}

/* App.vue */
<button @click="updateInfo">更新</button>

methods: {
  updateInfo() {
    this.$store.commit('updateInfo');
  }
}
```
:snowflake: `state` 中定义的属性会被添加到响应式系统中，并对其进行监听，当属性变化时，会通知所有用到该属性的地方，让界面刷新。  
:snowflake: `Vue.set()` 用于向对象（数组）添加或更改属性，接收三个参数：对象，索引值或键，修改后的值。  
:snowflake: 若通过方法②和④来添加或删除对象的属性，是做不到**响应式**的。  





