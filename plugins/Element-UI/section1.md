### CDN引用  
> 注意要**先**引入vue才能正常使用，而且需要使用到vue挂载的元素上。  

```
<head>
  <!-- 引入样式 -->
  <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
  <!-- 引入组件库 -->
  <script src="https://cdn.staticfile.org/vue/2.6.11/vue.min.js"></script>
  <script src="https://unpkg.com/element-ui/lib/index.js"></script>
</head>
    
<script>
new Vue({
    el: '#app',
    data: function() {
    return { visible: false }
    }
})
</script>
```

----

### 快速上手  
> 针对CDN引用外的其它引入方式，可以完整引入或按需引入。    

#### 按需引入  
> 按需引入时，通常先导入，然后安装/添加到原型。  

```
import {Dialog, Message} from 'element-ui';

/* 安装 */
Vue.use(Dialog);

/* 添加到原型 */
Vue.prototype.$message = Message;
```

#### 全局配置  
> 在导入组件后，安装组件前进行配置。  

```
// 拥有 size 属性的组件的默认尺寸均为 'small'，弹框的初始 z-index 为 3000。
Vue.prototype.$ELEMENT = { size: 'small', zIndex: 3000 };
```

----

### 内置过渡动画  
> 可以[按需引入](https://element.faas.ele.me/#/zh-CN/component/transition#an-xu-yin-ru)。  

#### 淡入淡出  

类型 | name
:-: | :-:
淡入淡出-线性 | el-fade-in-linear
淡入淡出 | el-fade-in
缩放-沿中间 | el-zoom-in-center
缩放-沿顶 | el-zoom-in-top
缩放-沿底 | el-zoom-in-bottom

```
// 控制按钮，需要初始化 show
<el-button @click="show = !show">Click Me</el-button>

<transition name="el-zoom-in-top">
  <div v-show="show">abc</div>
</transition>
```

#### 展开折叠  

```
<el-button @click="show = !show">Click Me</el-button>

<el-collapse-transition>
  <div v-show="show">abc</div>
</el-collapse-transition>
```

----

### Layout布局  
> 在行标签内镶嵌列标签，列标签占据的**最大**列数总合为24。  

```
<el-row>
  <el-col :span="3"><div>abc</div></el-col>
  <el-col :span="2"><div>def</div></el-col>
</el-row>
```

Row属性 | 说明 | 类型 | 默认值
:-: | :-: | :-: | :-:
:gutter | 栅格间隔 | num | 0
type | 布局模式，可选 flex | str | /
justify | flex-[水平排列方式](https://element.eleme.cn/#/zh-CN/component/layout) | str | start
align | flex-垂直排列方式 | str | /

Col属性 | 说明 | 类型 | 默认值
:-: | :-: | :-: | :-:
:span | 栅格占据的列数 | num | 24
:offset | 栅格左侧的间隔格数 | num | 0
:push | 栅格向右移动格数（相对定位） | num | 0

### Container布局容器  
> 采取了 flex 布局。  
> 
> 当存在顶栏或低栏容器时，子元素垂直上下排列，否则水平排列。  

标签 | 说明 
:-: | :-: 
`<el-container>` | 必须的外层容器 
`<el-header>` | 顶栏容器 
`<el-aside>` | 侧边栏容器 
`<el-main>` | 主要区域容器 
`<el-footer>` | 底栏容器 

属性 | 说明 | 类型 | 默认值
:-: | :-: | :-: | :-:
height | 顶/底栏高度 | str | 60px
width | 侧边栏宽度	 | str | 300px

### 字体和投影参考  

字体
```
font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
```
基础投影 
```
box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)
```
浅色投影 
```
box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1)
```

### Icon图标  
> 直接给相应标签添加类 `el-icon-iconName` 即可。  

```
// 纯图标
<i class="el-icon-loading"></i>

// 按钮组合图标
<el-button type="primary" icon="el-icon-search">搜索</el-button>
```

### Button按钮

属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
size | 尺寸  | str | / | medium / small / mini
type | 类型	 | str | / | primary / success / warning / danger / info / text 
icon | 图标类名  | str | / | /
native-type | 原生-类型  | str | button | 	button / submit / reset
plain | 朴素按钮  | boo | false | /
round | 圆角按钮  | boo | false | /
circle | 圆形按钮  | boo | false | /
loading | 加载中状态  | boo | false | /
disabled | 禁用状态  | boo | false | /
autofocus | 默认聚焦  | boo | false | /

### Link文字链接  

属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
type | 类型，影响颜色 | str | default| primary / success / warning / danger / info
underline | 悬浮下划线 | boo | true | /
icon | 图标类名  | str | / | /
disabled | 禁用状态  | boo | false | 	/
href | 原生-链接 | str | / | /
round | 圆角按钮  | boo | false | /
circle | 圆形按钮  | boo | false | /

### Radio单选框   
> 选中时，将相应的 `label` 的值赋值给绑定值。  

```
<el-radio v-model="selectFruit" label="apple">苹果</el-radio>
<el-radio v-model="selectFruit" label="peach">桃子</el-radio>
```

项属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
v-model | 绑定值	 | str/num/boo | / | /
label | 单选框的值 | str/num/boo | / | /
disabled | 禁用 | boo | false | /
border | 边框 | boo | false | /
size | 尺寸，需要 `border` 为真 | str | medium / small / mini | /
name | 原生-name | str | / | /

#### 单选框组
> 适用于在多个互斥的选项中选择的场景，无需给项绑定变量。  
```
<el-radio-group  v-model="selectFruit">
  <el-radio label="apple">苹果</el-radio>
  <el-radio label="peach">桃子</el-radio>
</el-radio-group>
```

组属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
v-model | 绑定值	 | str/num/boo | / | /
disabled | 禁用 | boo | false | /
size | 尺寸，需要 `border` 为真/按钮形式 | str | medium / small / mini | /
text-color | 按钮形式，激活文本颜色 | str | #ffffff | /
fill | 按钮形式，激活填充色 | str | #409EFF | /

项/组事件 | 说明 | 回调参数
:-: | :-: | :-:
change | 绑定值变化时触发的事件	| 选中项的 label 值

#### 按钮样式  
> 只需替换项元素即可。  
```
<el-radio-group  v-model="selectFruit">
  <el-radio-button label="apple">苹果</el-radio-button>
  <el-radio-button label="peach">桃子</el-radio-button>
</el-radio-group>
```

按钮属性  
> label、disabled、name。  

### Checkbox多选框  
> 单独使用可以表示两种状态之间的切换。  
> 
> 标签中的内容为按钮后的介绍。  

```
<el-checkbox v-model="isChecked">大西瓜</el-checkbox>
```

项属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
v-model | 绑定值	 | str/num/boo | / | /
label | 需要组标签，选中项的值 | str/num/boo | / | /
checked | 设为勾选 | boo | false | /
disabled | 禁用 | boo | false | /
border | 边框 | boo | false | /
size | 尺寸，需要 `border` 为真 | str | medium / small / mini | /
name | 原生-name | str | / | /

#### 多选框组  
```
<el-checkbox-group v-model="fruitList">
  <el-checkbox label="apple"></el-checkbox>
  <el-checkbox label="peach"></el-checkbox>
  <el-checkbox label="orange"></el-checkbox>
</el-checkbox-group>
```

组属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
v-model | 绑定值	 | arr | / | /
disabled | 禁用 | boo | false | /
min | 可被勾选最小数量 | num | / | /
max | 可被勾选最大数量 | num | / | /
size | 尺寸，需要有边框/按钮形式 | str | medium / small / mini | /
text-color | 按钮形式，激活文本颜色 | str | #ffffff | /
fill | 按钮形式，激活填充色 | str | #409EFF | /

项/组事件 | 说明 | 回调参数
:-: | :-: | :-:
change | 绑定值变化时触发的事件	| 更新后的值

#### 按钮样式2  
> 只需要把项元素替换为 `el-checkbox-button` [即可](#按钮样式)。  

按钮属性  
> label、disabled、name、checked...。  








