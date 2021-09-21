#### 页面占据视口全部高度  
> 若分为头部和内容，可设置内容 `height: calc(100vh - 头部高度)`

#### 三栏布局_两端留白版  

方案 | 说明
:-: | :- 
① | 通过设置布局宽度及偏移宽度控制
② | 给出宽度，左右外边距auto，需宽度＜父元素宽  
③ | 给父元素设置左右相等内边距

```
<!-- 顶部栏 -->
<el-row class="filter_tags">
  <el-col :span="18" :offset="3">
    顶部栏
  </el-col>
</el-row>
<!-- 内容区 -->
<el-row class="filter_tags">
  <el-col :span="18" :offset="3">
    内容区
  </el-col>
</el-row>
```

```
.div {
  width: 1000px;
  margin: auto;
}
```

```
.father {
  padding: 0 80px;
}
```

#### 改变组件默认样式  
> 对于某些组件，如 `el-select`，其内部存在嵌套较深的元素，可以在调试窗口找到。  
> 
> 由于使用了 `scoped`，样式无法影响到这些元素。  

索引 | 操作 | 说明
:-: | :- | :-  
① | 导入全局样式 | 适合对elementUI的整体修改 
② | [样式穿透](#选择器_去除默认边框及改变宽度) | 需要使用 less 或 sass    

- src  
  + assets
    - global.css  

```
/* main.js */
import "./assets/style/global.css";
```

----

#### Menu导航菜单  

#### 导航菜单_垂直菜单项左对齐排列及边缘凹进解决  

```
.el-menu {
  text-align: left;
  /* 添加颜色为菜单的背景颜色 */
  border: 1px solid #333;
}
```

#### 导航菜单_水平菜单项高度更改  
> 默认高度和行高为 `60px`。  

```
.el-menu--horizontal {
  &>.el-menu-item {
    height: 40px;
    line-height: 40px;
  }
}
```

#### 导航菜单_水平菜单激活项下划线去除&设置图片样式  
> 需要将默认的边界动画去除。  

```
.el-menu--horizontal {
    .is-active {
      border: none;
    }
    &>.el-menu-item {
      transition: background-color .3s,color .3s;
    img {
      width: 18px;
    }
  }
}
```

#### Container布局容器  

索引 | 操作 | 说明
:-: | :- | :-  
① | 默认内边距 | header、main等有默认内边距，可按需修改  
② | 固定高度 | 设置了容器高度时，可以通过 `height: 100%` 继承到内部以实现垂直居中  

#### Select选择器  

#### 选择器_去除默认边框及改变宽度

```
// 需要使用 less 或 sass  
@deep: ~'>>>';

.el-select {   
  width: 120px;
  @{deep} .el-input__inner {
    border: none; 
  }
}
```

#### Tag标签  

#### 标签_更改默认字体颜色高度和边框  
> 可给特定标签添加类，以添加特定样式。    

```
.el-tag {
  border: none;
  height: 28px;
  line-height: 28px;
}
.tag_active {
  color: rgb(18, 180, 145);
}
```

#### Badge标记  

#### 标记_修复添加后不对齐  
> 默认添加的 `vertical-align` 会导致文字下移。。  

```
.el-badge {
  vertical-align: baseline;
}
```




















