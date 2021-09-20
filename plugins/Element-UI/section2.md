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

----

#### Menu导航菜单  

#### 菜单项左对齐排列及边缘凹进解决  

```
.el-menu {
  text-align: left;
  /* 添加颜色为菜单的背景颜色 */
  border: 1px solid #333;
}
```

#### Container布局容器  

索引 | 操作 | 说明
:-: | :- | :-  
① | 默认内边距 | header、main等有默认内边距，可按需修改  
② | 固定高度 | 设置了容器高度时，可以通过 `height: 100%` 继承到内部以实现垂直居中  

















