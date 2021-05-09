
#### 最初的栗子  
```
/* HTML */
<div id="demo">
    <p>{{text}}</p>
</div>

/* Vue */
const demo = new Vue({    // 新建 Vue 对象实例
    el: '#demo',          // 用于挂载被管理的元素，可以是string（CSS 选择器）或 Element
    data: {               // 定义数据
        text: 'Hey, man!'  
    }
})
```
:snowflake: 所有的挂载元素会被 Vue 生成的 DOM 替换。因此不推荐挂载 root 实例到 <html\> 或者 <body\> 上。   
:herb: Vue 能管理挂载元素及其 DOM 结构上其子树的内容。

#### Vue列表显示
```
/* HTML */
<div id="demo">
    <p>{{text}}</p>
    <ul>
        <li v-for="item of colors">{{item}}</li>  // 迭代数组，并将每一个元素解析到对应位置
    </ul>
</div>

/* Vue */
const demo = new Vue({
    el: '#demo',
    data: {
        text: 'Hey, man!',
        colors: ['red','yellow','blue','slategrey']
    }

})
```
:snowflake: Vue 实例内部的 option 之间以 `,` 分隔，其中的 data 内部同样以 `,` 分隔。  
:snowflake: 最强大的地方在于它可以通过在控制台对 `demo.colors` 使用数组方法来控制列表。  
:snowflake: Vue 实例也代理了 data 对象上所有的 property，因此访问 `vm.a` 等价于访问 `vm.$data.a`。

#### 一次性计数器  
```
/* HTML */
<div id="demo">
    <h3>当前计数:{{counter}}</h3>
    <button v-on:click="add">+</button>
    <button @click="counter--">-</button>    // 直接对 data 中的数据进行操作
</div>

/* Vue */
const demo = new Vue({
    el: '#demo',
    data: {
      counter: 0
    },
    methods: {            // 用于在 Vue 对象中定义方法；为对象类型
      add: function() {
        this.counter++;
      }
    } 
})
```
:snowflake: `v-on:` ，用于监听某个元素的点击事件，可以简写为 `@`。    
:snowflake: data 中的数据用到 Vue 对象的其他选项中需要加前缀 `this.` ，加到 HTML 元素中，则不需要。    
:bug: data 内部的最后不要加上 `;`，会导致一些问题。  

#### Vue中的MVVM  

![MVVM-1](./img/MVVM-1.jpg)  

- View层：视图层  
- Model层：数据层  
- VueModel层：视图模型层
  + 实现了数据绑定，将 Model 的改变**实时**的反应到 View 中    
  + 实现了 DOM 的事件监听。当 DOM 中发生一些事件时，会被它监听到，并在需要的情况下改变对应的 Data  

![MVVM-2](./img/MVVM-2.jpg)  

#### 生命周期钩子  
> 每个 Vue 实例在被创建时都要经过一系列的初始化过程，在这些过程当中也会运行一些叫做生命周期钩子的函数，可以给这些钩子**添加函数**，它们将会被用作回调。  

```
/* Vue */
const demo = new Vue({
    el: '#demo',
    created: function() {
        console.log('created');
    }
})
```

----

### 插值  

#### 文本   
> 即 Mustache 语法 (双大括号)，会将数据解释为普通文本。    
```
<div id="demo">
    <h3>{{type + ' ' + name}}</h3>
    <h3>明年{{counter + 1 + '岁'}}</h3>
</div>

/* Vue */
const demo = new Vue({
    el: '#demo',
    data: {
        type: 'dog',
        name: 'sunny',
        counter: 17
    }
})
```

v-once指令  
> 能执行首次的插值，但当数据改变时，插值处的内容不会更新。
```
<div id="demo">
    <h3 v-once>{{message}}</h3>
</div>
```

#### 原始HTML  
> 可以输出真正的 HTML。  

```
<p v-html="url"></p>

/* Vue */
const demo = new Vue({
    ...
    data: {
        url: '<a href="www.bilibili.com">bilibili</a>'
    }
})

```

:bug: **绝不要**对用户提供的内容使用插值，很容易导致 XSS 攻击。  

#### v-text  
> 更新（替换）元素的 textContent。  

```
<p v-text="lalala">被替代了</p>
```

#### v-pre  
> 跳过这个元素和它的子元素的编译过程。可以用来显示原始 Mustache 标签，就像是取消了数据绑定。  

```
<span v-pre>{{ this will not be compiled }}</span>
```
#### v-cloak  
> 保持在元素上直到关联实例结束编译（即消失）。    

这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。

```
/* CSS */
[v-cloak] {
  display: none;
}


/* HTML */
<div v-cloak>
  {{ message }}
</div>
```
:snowflake: 届时，当网页卡顿时，就不会将未编译的 Mustache 标签显示出来。



















