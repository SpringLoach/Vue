

#### 基本使用  
> 需要注意的是，vue3创建实例与挂载的方式**与之前不同**。  

```
<head>
  <script src="https://unpkg.com/vue@next"></script>
</head>

<script>

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






















