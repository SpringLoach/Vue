**#模块化思路**
1. 建立一个 TabBar 组件，并以 TabBarItem 组件作为它的子组件。  
2. 在 TabBar 组件中使用插槽，使外界可以动态插入任意数量的 TabBarItem 组件。  
3. 在 TabBarItem 组件中，使用图片插槽和文字插槽。  
  
:palm_tree: 组件自身相关的 HTML 和样式写到自己内部，然后引入根组件注册并使用。  
:palm_tree: 添加插槽是为了是让外界动态决定插入的内容，有利于组件的复用。    
:palm_tree: 注意 TabBarItem 中根容器即为弹性项目，即`class="tab-bar-item"`。

- src
  + assets
    - css
      + base.css
    - img
      + tabbar
  + components
    - tabbar
      + TabBar.vue
      + TabBarItem.vue

**#样式**

1. 初始化的样式很多（这里初始化内外边距），可以写到 `base.css` 中，然后引入 `App.vue`  
```
/* App.vue */

<style>
  @import "./assets/css/base.css"; 
</style>
```

2. 水平方向平均分布，固定底部，阴影效果，控制弹性项目中图片和文字的布局。

  - 容器框类型改为 `flex`；背景#f6f6f6；定位为 `fixed`，且左右底为0；`box-shadow: 0 -1px 1px rgba(100,100,100,.1)`
  - 弹性项目添加样式 `flex: 1` ；居中；高度49px

:palm_tree: `tab-bar-item` 的高度为 49px，是最佳实践。  
:palm_tree: `fixed` 定位时左右为 0 ，能保证占据全部宽度。  
:palm_tree: `<img>` 底部默认有 3 像素，`vertical-align: middle` 可以将其去除。

----

#### 传入active图片  

在 TabBarItem 组件中新增一个具名插槽 `name="item-icon-active"` 用于放入活跃状态的图片。   

然后结合[条件渲染](#https://github.com/SpringLoach/Vue/blob/main/learning/section1.md#条件渲染)决定渲染哪张图片。  

:palm_tree: 插槽被替换时，属性也会被替换掉。可以将插槽包含在一个 `<div>` 内部，根据需要对 `<div>` 添加属性和类。  



