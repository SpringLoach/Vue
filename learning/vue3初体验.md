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

----

### Treeshaking  
> 过去有许多直接暴露在单个 Vue 对象上的全局 API，尽管没有使用它们，仍会被包含在最终的打包产物中。  
> 
> `tree-shaking` 是表达 “死代码消除” 的一个花哨术语。有了全局 tree-shake 后，用户只需为他们实际使用的功能 “买单” 。

```
// 要使用全局 API，现在必须导出
import { nextTick } from 'vue'

nextTick(() => {
  // 一些和DOM有关的东西
})
```

#### 受影响的 API  

Vue 2.x | 说明
:-: | :-:
Vue.nextTick | 常用于操作 DOM
Vue.observable | 用 Vue.reactive 替换
Vue.version | /
Vue.compile | 仅完整构建版本
Vue.set | 仅兼容构建版本
Vue.delete | 仅兼容构建版本

#### 防止模块打包工具打包Vue  
> 如果使用 webpack 这样的模块打包工具，这可能会导致 Vue 的源代码输出打包到插件中。  
>
> 可以告诉 webpack 将 Vue 模块视为一个外部库。  

```
// webpack.config.js
module.exports = {
  /*...*/
  externals: {
    vue: 'Vue'
  }
}
```

### v-model  

1\. 用于自定义组件时，`v-model` prop 和事件默认名称已更改

v-model | 从前 | 现在
:-: | :-: | :-:
prop | value | modelValue
event | input | update:modelValue

2\. 移除了 `v-bind` 的 `.sync` 修饰符和组件的 `model` 选项，可用 v-model 作为代替。  

3\. 允许在同一个组件上使用多个 `v-model` 进行双向绑定。  

4\. 允许[自定义](https://v3.cn.vuejs.org/guide/component-custom-events.html#处理-v-model-修饰符) `v-model` 修饰符，例如解决 `number` 无法输入小数点的问题。  

#### 默认情况  
```
<ChildComponent v-model="pageTitle" />

<!-- 相当于 2.X -->
<ChildComponent :value="pageTitle" @input="pageTitle = $event" />

<!-- 相当于 3.X -->
<ChildComponent :modelValue="pageTitle" @update:modelValue="pageTitle = $event" />
```

#### 更改model名称  
```
<ChildComponent v-model:title="pageTitle" />

<!-- 是以下的简写: -->

<ChildComponent :title="pageTitle" @update:title="pageTitle = $event" />
```

#### 使用多个v-model
```
<ChildComponent v-model:title="pageTitle" v-model:content="pageContent" />

<!-- 是以下的简写： -->

<ChildComponent
  :title="pageTitle" @update:title="pageTitle = $event"
  :content="pageContent" @update:content="pageContent = $event"
/>
```

----

#### Key

索引 | 说明 | 补充
:-: | :- | :-:
Ⅰ | 对于条件渲染，key 可以不提供 | Vue 会自动生成唯一的 key
Ⅱ | 对于条件渲染，如果手动提供 key，必须保证唯一性 | /
Ⅲ | 对于 `<template v-for>`, key 应该设置在 <template\> 标签上 | Vue2 则需要设置到它的子节点

----

#### v-if与v-for的优先级  
> 与 Vue2 相反，两者作用于同一个元素上时，现在 `v-if` 优先级更高。
>
> 建议避免在同一元素上同时使用两者。

----

#### v-bind敏感排序  
> 过去，如果一个元素同时定义了 `v-bind="{}"` 和一个相同的属性，会取单独的属性。  
> 
> 现在以位于标签靠后的优先。  

```
<!-- template -->
<div id="red" v-bind="{ id: 'blue' }"></div>
<!-- result -->
<div id="blue"></div>
```

----  

#### 移除.native修饰符  
> 过去，需要填加修饰符监听子组件根元素的原生事件。  
> 
> 现在默认子组件中的**所有事件**都被当作原生事件添加到子组件的根元素中。  
> 
> 可以添加 `emits` 选项记录仅在当前组件触发的事件。    

```
// 父组件
<my-component @close="mF" @mousedown="mF2" />

// 子组件选项
emits: ['close']

/* 父组件将不能监听该组件的 close */
```

----

#### v-for中的Ref数组  
> 过去，在带有 `v-for` 结构的标签里使用 ref 会用 ref 数组填充相应的 `$refs`，但被遗弃了。  
> 
> 现在，要从单个绑定获取多个 ref，需要将 `ref` 定义为[回调函数](https://v3.cn.vuejs.org/guide/migration/array-refs.html)，显式地[传递](https://v3.cn.vuejs.org/api/special-attributes.html#ref)元素或组件实例。  


















