
#### 最初的栗子  
```
/* HTML */
<div id="demo">
    <p>{{text}}</p>
</div>

/* Vue */
const demo = new Vue({    // 新建 Vue 对象实例
    el: '#demo',          // 用于挂载被管理的元素，填入对应 id
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
        <li v-for="item of colors">{{item}}</li>  // 迭代数组，并将成员解析为 li 的文本节点
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



