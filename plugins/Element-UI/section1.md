[分组_Select](#分组_Select)  

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
> 可以不添加文本节点来制作图标按钮。  

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
change | 绑定值变化时触发	| 更新后的值

#### 按钮样式2  
> 只需要把项元素替换为 `el-checkbox-button` [即可](#按钮样式)。  

按钮属性  
> label、disabled、name、checked...。  

### Input输入框  
> 需要绑定值才能正常使用。  
> 
> 不支持 `v-model`修饰符。  

```
<el-input v-model="variable" placeholder="请输入"></el-input>
```

项属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
type | 类型 | str | text | textarea（文本域） / 原生-type
v-model | 绑定值	 | str/num | / | /
placeholder | 占位文本 | str | / | /
disabled | 禁用 | boo | false | /
clearable | 清空 | boo | false | /
show-password | 切换密码状态 | boo | false | /
autosize | （需文本域类型）自适应内容高度 | boo/obj | false | /
size | 输入框尺寸 | str | / | large / small / mini
maxlength | 原生-最大输入长度 | num | / | /
minlength | 原生-最小输入长度 | num | / | /
show-word-limit | 显示输入字数统计 | boo | false | /

#### 复合元素使用  
> 可以添加按钮、[图标](#Icon图标)、使用插槽添加文本，添加一到多个元素。  
```
// 文本
<el-input>
  <template slot="append">.com</template>
</el-input>
// 图标按钮
<el-input>
  <el-button slot="append" icon="el-icon-search"></el-button>
</el-input>
```

slot值 | 说明 
:-: | :-: 
prefix | 头部内容 
suffix | 尾部内容
prepend | 前置内容 
append | 后置内容 

#### 带建议的输入框  
> 在元素渲染完毕后，将请求到的数据保存到本地。  
> 
> 建议方法的回调参数为输入字符串、接收筛选后建议列表数组的回调。  
> 
> 筛选数组的每个项的 `value` 值会被输出成建议。  
> 
> 也可以[复合元素使用](#复合元素使用)。  

```
<el-autocomplete v-model="selectRes" :fetch-suggestions="querySearch"></el-autocomplete>

data() {
  return {
    restaurants: [],
    selectRes: ''
  };
},
methods: {
  querySearch(queryString, cb) {
    let restaurants = this.restaurants;
    let results = queryString ? restaurants.filter(this.createFilter(queryString)) : restaurants;
    // 调用建议列表的数据
    cb(results);
  },
  createFilter(queryString) {
    return (restaurant) => {
      return (restaurant.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0);
    };
  },
  loadAll() {
    return [
      { value: "三全鲜食（北新泾店）" },
      { value: "Hot honey 首尔炸鸡（仙霞路）" }
    ];
  }
},
mounted() {
  this.restaurants = this.loadAll();
}
```

属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
v-model | 绑定值	 | str/num | / | /
:fetch-suggestions | 返回输入建议的方法	 | F(queryString, callback) | / | /
placeholder | 占位文本 | str | / | /
:trigger-on-focus | 激活即列出输入建议 | boo | true | /
:popper-append-to-body | 将下拉列表插入 body，可能**影响定位** | boo | true | /
disabled | 禁用 | boo | false | /
:debounce | 获取输入建议的去抖延时 | num | 300 | /

#### 自定义模板  
> 可以改变建议模板，而不是只能选择 `value` 作为输出。  
> 
> 模板标签的 `slot-scope` 属性指向每一条建议。  

```
<el-autocomplete v-model="selectRes" :fetch-suggestions="querySearch">
  <template slot-scope="{ item }">
    <div class="name">{{ item.value }}</div>
    <span class="addr">{{ item.address }}</span>
  </template>
</el-autocomplete>
```

#### 输入长度建议  
> 需要配合限制长度属性使用。  

```
<el-input v-model="any" :maxlength="12" show-word-limit></el-input>
```

### InputNumber计数器  
> 可手动或通过控制器改变数值。只需绑定值即可。    

```
<el-input-number v-model="num" :min="1" :max="3"></el-input-number>
```

属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
v-model | 绑定值 | num | 0 | /
:min | 允许的最小值 | num | -Infinity | /
:max | 允许的最大值	| num | Infinity | /  
step | 计数器步长 | num | 1 | /  
step-strictly | 强制转换输入为步长倍数 | boo | false | / 
disabled | 禁用计数器 | boo | false | /  
precision | 数值精度 | num | / | /
size | 尺寸 | str | / | medium / small / mini
:controls | 使用控制器 | boo | true | /
controls-position | 控制按钮位置 | str | / | right
name | 原生 | str | / | /

事件 | 说明 | 回调参数
:-: | :-: | :-:
change | 绑定值变化时触发	| 新值，旧值

### Select选择器  
> 即供以选择的下拉菜单。  

```
<el-select v-model="value">
  <el-option v-for="item in options" :key="item.value"
    :label="item.label" :value="item.value">
  </el-option>
</el-select>

data() {
  return {
    options: [{
      value: '选项1',
      label: '黄金糕'
    }, {
      value: '选项2',
      label: '双皮奶'
    }],
    value: ''
  };
}
```

Select属性 | 说明 | 类型 | 默认值 / 回调参 | 可选值
:-: | :-: | :-: | :-: | :-:
v-model | 绑定值 | num / boo / str / arr | / | /
placeholder | 占位符 | str | 请选择 | /
disabled | 完全禁用 | boo | false | /
clearable | 提供清空按钮 | boo | false | /
multiple | 启用多选 | boo | false | /
collapse-tags | 多选时将选中值按文字的形式展示 | boo | false | /  
filterable | 可根据输入搜索 | boo | false | /
filter-method | 自定义搜索方法 | func | 输入值 | /
remote | 远程搜索 | boo | false | /
remote-method | 远程搜索方法 | func | 输入值 | /
allow-create | （需 filterable 真）允许用户创建新条目 | boo | false | /
default-first-option | 提供回车选择 | boo | false | /

Option属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
:value | 选项的值 | str / num / obj | / | /
:label | 展示值 | num / str | / | /

#### 自定义模板_Select
> 直接在项标签中插入相应内容。  

```
<el-select ...>
  <el-option v-for="item in options"...>
    <span>{{item.label}}</span>anything<span>{{item.value}}</span>
  </el-option>
</el-select>
```

#### 分组_Select  
> 使用 `el-option-group` 对选项进行分组。  

```
<el-select v-model="value">
  <el-option-group v-for="group in options" :key="group.label" :label="group.label">
    <el-option v-for="item in group.options" :key="item.value" 
      :label="item.label" :value="item.value">
    </el-option>
  </el-option-group>
</el-select>

data() {
  return {
    options: [{
      label: '热门城市',
      options: [{
        value: 'Shanghai',
        label: '上海'
      }, {
        value: 'Beijing',
        label: '北京'
      }]
    }, {
      label: '城市名',
      options: [{
        value: 'Chengdu',
        label: '成都'
      }, {
        value: 'Shenzhen',
        label: '深圳'
      }]
    }],
    value: ''
  };
}
```

Option属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
:label | 分组名 | str | / | /
disabled | 禁用该分组 | boo | false | /

#### 搜索_Select   
> 给选择容器添加 `filterable` 属性，即能从选项中筛选出 `label` 属性包含输入值的项。  
> 
> 可以通过传入 `filter-method` 来改变搜索逻辑，在输入值改变时调用。  

#### 远程搜索_Select  
> 需要开启 `filterable` 和 `remote`，并传入 `remote-method`。  

#### 创建条目_Select  
> 需开启 `allow-create` 和 `filterable`，容器绑定对象类型时，使用 `item.value` 作为 key 值。  

### Cascader级联选择器  

```
<el-cascader v-model="value" :options="options" 
  :props="casProps" ></el-cascader>

data() {
  return {
    value: '',
    casProps: { expandTrigger: 'hover' },
    options: [{
      value: 'zhinan',
      label: '指南',
      children: [{
        value: 'shejiyuanze',
        label: '设计原则',
        children: [{
          value: 'yizhi',
          label: '一致'
        }, {
          value: 'fankui',
          label: '反馈'
        }]
      }, {
        value: 'daohang',
        label: '导航',
        children: [{
          value: 'cexiangdaohang',
          label: '侧向导航'
        }]
      }]
    }]
  };
}
```

属性 | 说明 | 类型 | 默认值 / 回调参 | 可选值
:-: | :-: | :-: | :-: | :-:
v-model | 绑定值 | / | / | /
options | 选项数据源 | arr | / | /
props | [配置选项](#Props配置选项_Cascader) | obj | / | /
clearable | 提供清空按钮 | boo | false | /
placeholder | 占位符 | str | 请选择 | /
:show-all-levels | 显示选中值的完整路径 | boo | true | /
filterable | 可搜索选项 | boo | / | /
filter-method | 自定义搜索逻辑 | func(node, keyword) | 节点，搜索关键词 | /

#### Props配置选项_Cascader

属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
expandTrigger | 次级菜单的展开方式 | str | 'click' | 'hover'
value | 以选项对象的某属性作为**选项值** | str | 'value' | /
label | 以选项对象的某属性作为**选项展示值** | str | 'label' | /
children | 以选项对象的某属性作为**子选项** | str | 'children' | /
disabled | 以选项对象的某属性作为**禁用** | str | 'disabled' | /
checkStrictly | 可选非叶子节点 | boo | false | /
:emitPath | 选中项改变时，返回各级节点值的数组 | boo | true | /
multiple | 开启多选 | boo | false | /

#### 可搜索_Cascader  
> 将 `filterable` 设为真即可。默认匹配所有节点的 `label`。  
> 
> 未开启 `show-all-levels` 时，匹配父节点的 `label`。  
> 
> 可以传入 `filter-method` 来自定义搜索逻辑，通过返回布尔值表示是否通过筛选。  

#### 自定义节点展示内容_Cascader  
> 传入的两个对象分别表示当前节点对象和数据。  
```
<el-cascader :options="options">
  <template slot-scope="{ node, data }">
    <span>{{ data.label }}</span>
    <span v-if="!node.isLeaf"> ({{ data.children.length }}) </span>
  </template>
</el-cascader>
```

### Switch开关  
> 模拟开关左右滑动的组件。  

```
<el-switch v-model="value"></el-switch>

data() {
  return {
    value: true
  };
}
```

属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
v-model | 绑定值 | boo / str / num | / | / 
width | 宽度 | num | 40 | /
active-color | 打开时的背景色 | str | #409EFF | /
inactive-color | 关闭时的背景色 | str | #C0CCDA | /
active-text | 打开时的文字描述 | str | / | /
disabled | 禁用 | boo | false | /
active-value | 打开时的值 | boo / str / num | true | / 
inactive-value | 关闭时的值 | boo / str / num | false | / 

### Slider滑块   

```
<el-slider v-model="value"></el-slider>

data() {
  return {
    value: 15
  };
}
```

属性 | 说明 | 类型 | 默认值/回调参 | 可选值
:-: | :-: | :-: | :-: | :-:
v-model | 绑定值 | num | 0 | / 
min | 最小值 | num | 0 | /
max | 最大值 | num | 100 | /
step | 步长 | num | 1 | /
:show-tooltip | 显示提示 | boo | true | /
:format-tooltip | 格式化提醒 | func | val | /
show-input | （非范围选择）显示输入框 | boo | false | /
input-size | 输入框尺寸 | str | small | large / medium / small / mini
vertical | 竖向模式 | boo | false | /
height | 竖向模式高度，带单位 | str | / | /
disabled | 禁用 | boo | false | /
range | 范围选择 | boo | false | /

### TimePicker时间选择器  

#### 固定或任意时间点  
```
// 固定时间点
<el-time-select v-model="value" :picker-options="pickOpt">
</el-time-select>

// 任意时间点
<el-time-picker v-model="value2" :picker-options="pickOpt2">
</el-time-picker>

data() {
  return {
    value: '',
    pickOpt: {
      start: '08:30',
      step: '00:15',
      end: '18:30'
    },
    // 任意时间点
    value2: '',
    pickOpt2: {
      selectableRange: '18:30:00 - 20:30:00'
    }
  };
}
```

属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
v-model | 绑定值 | num | 0 | / 
placeholder | 占位符 | str | / | /
:picker-options | [时间段选项](#picker-options) | obj | {} | /
arrow-control | （需picker标签）使用箭头进行时间选择 | boo | false | / 
is-range | （需picker标签）时间范围选择 | boo | false | / 
start-placeholder | 范围选择时开始时间占位符 | str | / | / 
end-placeholder | 范围选择时结束时间占位符 | str | / | / 

#### picker-options  

选项 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
start | 开始时间 | str | 09:00 | / 
end | 结束时间 | str | 18:00 | /
step | 间隔时间 | str | 00:30 | /
minTime | 禁用的最小时间 | str | 00:00 | /
maxTime | 禁用的最大时间 | str | / | /  
selectableRange | 限制可选时间范围 | str / arr | 栗 `'18:30:00 - 20:30:00'` | /  

#### 固定时间范围
> 设置两个[固定时间](#固定或任意时间点)，将开始时间的绑定值设置为结束时间的禁用最小时间即可。  

### Upload上传  
> 通过点击或者拖拽上传文件。  

#### 点击上传  

```
<el-upload action="https://jsonplaceholder.typicode.com/posts/"
  multiple :file-list="fileList">
  <el-button size="small" type="primary">点击上传</el-button>
  // 可选的提示语句
  <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
</el-upload>
  
data() {
  return {
    fileList: [
      {name: 'a.jpeg', url: '...'}, 
      {name: 'b.jpeg', url: '...'}
    ]
  };
}
```

属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
:action | **必选**，上传的地址 | str | / | / 
:headers | 设置上传的请求头部 | obj | / | /
multiple | 支持多选文件 | boo | / | /
:limit | 最大允许上传个数 | num | / | /
:file-list | 上传的文件列表 | arr | \[\] | 每一项都为有 `name` 和 `url` 的对象  
:show-file-list | 显示已上传文件列表 | boo | true | /
list-type | 文件列表的[类型](#文件列表类型) | str | text | picture / picture-card
drag | 支持拖拽上传 | boo | false | /
:on-preview | 预览钩子 | func(file) | / | /
:before-remove | 移除前钩子，返回 `false` 则停止删除 | func(file, fileList) | / | /
:on-remove | 移除时钩子 | func(file, fileList) | / | /
:on-exceed | 文件超出个数限制时的钩子 | func(files, fileList) | / | /
:before-upload | 上传前钩子，返回 `false` 则停止上传 | func(file) | / | /

#### 文件列表类型  
> 即 `list-type` 属性。  

值 | 说明 
:-: | :-: 
'fileList' | 默认
'picture-card' | 图片列表缩略图 
'picture-card' | 照片墙 

### Rate评分  

```
// 无颜色差异
<el-rate v-model="value1"></el-rate>
// 有颜色差异
<el-rate v-model="value2" :colors="colors"></el-rate>
      
data() {
  return {
    value1: null,
    value2: null,
    colors: ['#99A9BF', '#F7BA2A', '#FF9900']  // 等同于 { 2: '#99A9BF', 4: { value: '#F7BA2A', excluded: true }, 5: '#FF9900' }
  };
}
```

属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
v-model | 绑定值 | num | 0 | / 
:colors | 分段颜色 | arr / obj | \['#F7BA2A', '#F7BA2A', '#F7BA2A'\] | /
:max | 最大分值	 | num | 5 | / 
low-threshold | **低分**和中等分数的界限值	 | num | 2 | / 
high-threshold | **高分**和中等分数的界限值	 | num | 4 | / 
disabled | 只读 | boo | false | / 
show-text | 显示辅助文字 | boo | false | / 
show-score | 显示分数，与文字冲突 | boo | false | / 
texts | 辅助文字数组 | arr | \['极差', '失望', '一般', '满意', '惊喜'\] | / 
text-color | 辅助文字/分数颜色 | str | #1F2D3D | / 
score-template | 分数显示模板 | {value} | str | / 

#### 只读评分  
> 可以提供 `score-template` 作为显示模板，`{value}` 会被解析为分值。
```
<el-rate v-model="value" disabled show-score text-color="#ff9900" score-template="{value}分"></el-rate>

data() {
  return {
    value1: 3.7
  };
}
```

### Form表单  
> 表单由一到多个表单域，表单域中可以放置各种类型的表单控件。  
> 
> 如输入框、选择器、开关、单选框、多选框等。  

```
<el-form ref="form" :model="form" label-width="80px">
  <el-form-item label="活动名称">
    <el-input v-model="form.name"></el-input>
  </el-form-item>
</el-form>

data() {
  return {
    form: {
      name: ''
    }
  }
}
```

容器属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
:model | 表单数据 | obj | / | / 
:rules | 表单[验证规则](#验证规则) | obj | / | / 
ref | 指向该节点 | str | / | / 
inline | 行内表单模式，使表单元素集中到一行 | boo | false | / 
label-position | 表单域标签的位置 | str | right | right/left/top
label-width | 表单域标签的宽度（需单位） | str | / | / 
size | 控制该表单内组件的尺寸 | str | / | medium / small / mini 

容器节点方法 | 说明 | 类型 | 回调参数 
:-: | :-: | :-: | :-: 
validate | 对整个表单进行校验，参数为回调函数 | F(F(boo, obj)) | 是否校验成功， 未通过校验的字段 | / 
resetFields | 对整个表单重置：将所有字段值重置为初始值并移除校验结果 | / | /
clearValidate | 移除表单项的校验结果 | / | /

项属性 | 说明 | 类型 | 默认值 | 可选值
:-: | :-: | :-: | :-: | :-:
:label | 标签文本 | str | / | / 
:prop | 校验的字段名，与 `label` 的叶属性一致 | str | / | / 
label-width | 表单域标签的宽度（需单位） | str | / | / 
size | 控制该表单域下组件的尺寸 | str | / | medium / small / mini 

#### 验证规则
```
// 验证规则可以有多个。  
// require：必填  message：错误信息  trigger：验证时机  min：最小字符数  max：最大字符数

rules: {
  name: [
    { required: true, message: '请输入活动名称', trigger: 'blur' },
    { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
  ],
  region: [
    { required: true, message: '请选择活动区域', trigger: 'change' }
  ]
}
```  

#### 自定义校验规则  
> 通过自定义校验规则，可以完成密码的二次验证。  

#### 动态增减表单项  
> 通过对数据数组使用 `v-for` 进行渲染 DOM，通过操作数据数组动态改变 DOM。  
> 
> 可以用 `Date.now()` 来创建 `key`，防止重复。  

#### 限制输入为数字类型
> 给对应的 `v-model` 加上 `.number` 修饰符即可。但无法输入小数点。  










