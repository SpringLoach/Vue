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

#### Layout布局  
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


























