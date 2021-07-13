- 起步
  + [基本使用](#基本使用)
  + [官方模块化写法](#官方模块化写法)
- 新特性
  + [组合式API](#组合式API)
    - [setup组件选项](#setup组件选项)
      + [响应式变量](#响应式变量)
      + [初始情况](#初始情况)
    - [使用 ref 函数创建响应式引用](#使用 ref 函数创建响应式引用)
    - [在setup内注册生命周期钩子](#在setup内注册生命周期钩子)
    - [watch响应式更改](#watch响应式更改)
    - [独立的computed属性](#独立的computed属性)
    - [功能模块的结合](#功能模块的结合)


#### 基本使用  
> 需要注意的是，vue3创建实例与挂载的方式**与之前不同**。  

```
<head>
  <script src="https://unpkg.com/vue@next"></script>
</head>

<script>

// 该方法暴露在全局 Vue 上  
Vue.createApp({
  data(){
    return{
      anyt: [4, 2]
    }
  }
}).mount("#app")

</script>
```

另一种展现  
```
const htmls={
  data(){
    return{
      anyt: [4, 2]
    }
  }
}
Vue.createApp(htmls).mount("#vue")
```

#### 官方模块化写法
> 将组件作为参数传入。  
```
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```

----

### 组合式API  
> 使用 data、computed、methods、watch 等组件选项来组织逻辑通常都很有效。然而，当我们的组件开始变得更大时，逻辑关注点的列表也会增长，导致组件难以阅读和理解。
> 
> 组合式 API 能够将同一个逻辑关注点相关代码收集在一起。  

#### setup组件选项  
> 该选项会在 props 被解析后，组件创建**之前**执行。    
> 
> 因此，避免在选项中使用 `this`，反而它返回的结果可以用于模板及组件的其余部分。  

```
props: {
  user: {
    type: Number,
    default: 3
  }
},
// 首参为自定义属性选项
setup(props) {
  console.log(props) // { user: 3 }
  
  return {}
}
```

----

#### 响应式变量

**初始情况**   
> 该选项内的变量是非响应的，即使在其它地方改变了它，也不会体现到视图上。  
```
setup(props) {
  const abc = 2;
  return {abc} 
}
```

**使用 `ref` 函数创建响应式引用**    
> 该函数返回一个对象，包含一个值为参数的 `value` 属性。  
```
import { ref } from 'vue'

setup(props) {
  const abc = ref(2);  
  abc.value++;
  
  console.log(abc);         // { value: 3 }
  console.log(abc.value);   // 3

  // 内部需要操作它的 value 属性，导出时则不需要考虑
  return {abc} 
}
```

----

#### 在setup内注册生命周期钩子
> 这些新函数的名字与生命周期钩子类似，如 `onMounted`。  
> 
> 接受一个回调，执行时机同组件生命周期。  

```
import { ref, onMounted } from 'vue'

setup(props) {
  const abc = ref(2);  

  const mF = () => {console.log(abc.value)}
 
  // 在 `mounted` 时调用 `mF`
  onMounted(mF)

  return {abc} 
}
```

#### watch响应式更改  
> 首参为需要侦听的**响应式引用**或 getter 函数  
> 
> 第二个参数为回调。  

```
import { ref, watch, toRefs } from 'vue'

props: {
  user: {
    type: String,
    default: 'abc'
  } 
},
setup(props) {
  // // 使用 `toRefs` 创建对自定义属性 `user` 的响应式引用
  const { user } = toRefs(props)
  
  const abc = ref([])
  
  // 更新值的方法
  const mF = () => {abc.value.push(user.value)}
  
  // 在 user prop 的响应式引用上设置一个侦听器
  watch(user, mF)
  
  return {abc}
}
```

#### 独立的computed属性  
> 该方法接收类似 getter 的回调函数，输出**只读**的响应式引用。  

```
import { toRefs, computed } from 'vue'

props: {
  user: {
    type: Number,
    default: 2
  } 
},
setup(props) {
  // // 使用 `toRefs` 创建对自定义属性 `user` 的响应式引用
  const { user } = toRefs(props)
  
  const abc = computed(() => {
    return user.value + 1;
  })
  
  return {abc}
}
```

#### 功能模块的结合  
> 可以将不同功能的模块拆分成单独的文件，将其作为函数导出，并使用解构获取返回值。  
> 
> 之后的函数可以使用先前函数的返回值作为参数。最终再从 `setup` 导出需要的变量。  

```
setup(props) {
  const { user } = toRefs(props)

  const { data1, data2 } = mF1(user)

  const { data3, data4 } = mF2(data1)

  return {
    data1,
    data2,
    data3,
    otherName: data4
  }
}
```

----

### 全局API
> 在 Vue2 中，只有全局的 Vue 接口而没有**实例接口**，因此全局配置很容易意外地污染其他测试用例。  

#### 接口转移  
> 任何全局改变 Vue 行为的 API 现在都会移动到应用实例上。  

2.x 全局 API | 3.x 实例 API (以app为栗) | 说明
:-: | :-: | :-: 
Vue.config | app.config | /
Vue.config.productionTip | 移除 | “使用生产版本” 提示
Vue.config.ignoredElements | app.config.isCustomElement  | 支持原生自定义元素
Vue.component | app.component | /
Vue.directive | app.directive | /
Vue.mixin | app.mixin | /
**Vue.use** | app.use  | /
**Vue.prototype** | app.config.globalProperties | 用于添加所有组件都能访问的属性

#### 插件使用须知  
> 之前的 `Vue.use()` 被废除，需要在实例上使用插件（挂载前）。  

```
/* 之前 */
Vue.use(VueRouter)

/* 现在 */
const app = createApp(`组件`)
app.use(VueRouter)
```

#### 挂载App实例  
> 使用 createApp(/\* options \*/) 初始化后，将它挂载到元素上。  

```
import { createApp } from 'vue'
import MyApp from './MyApp.vue'

const app = createApp(MyApp)

// 可以通过 app 添加指令、插件依赖性等

app.mount('#app')
```

#### 依赖注入  
> 与 Vue2 的 [provide](https://github.com/SpringLoach/Vue/blob/main/learning/其它官方补充.md#依赖注入) 选项类似。  

```
// 在入口
app.provide('guide', 'Vue 3 Guide')

// 在子组件以 book 取得相应值、方法等
export default {
  inject: {
    book: {
      from: 'guide'
    }
  }
}
```

#### 在应用之间共享配置  
> 创建工厂函数。即在函数内部通过参数创建 Vue 实例并添加配置，最终将实例返回。  























