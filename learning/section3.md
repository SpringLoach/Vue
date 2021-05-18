## tabber

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
  + views
    - cart
    - category
      + Category.vue
    - home
      + Home.vue
    - profile

文件 | 说明 
 :-: | :-: 
 components | 放公共、功能性组件的文件
 views | 放独立组件的文件
 home | 文件夹用小写命名
 Home.vue | vue 件用大写命名


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
:palm_tree: 命名类名时，与组件标签一致，都是用 `-` 连接。  

----

#### 传入active图片  

在 TabBarItem 组件中新增一个具名插槽 `name="item-icon-active"` 用于放入活跃状态的图片。   

然后结合[条件渲染](https://github.com/SpringLoach/Vue/blob/main/learning/section1.md#条件渲染)决定渲染哪张图片。  

:palm_tree: 插槽被替换时，属性也会被替换掉。可以将插槽包含在一个 `<div>` 内部，根据需要对 `<div>` 添加属性和类。  

----

#### 结合路由进行页面跳转

- 在创建好组件后，进行[路由映射配置](https://github.com/SpringLoach/Vue/blob/main/learning/section2.md#路由映射配置)以及[路由的默认值和模式修改](https://github.com/SpringLoach/Vue/blob/main/learning/section2.md#路由的默认值和模式修改)，而且使用的是[路由懒加载](https://github.com/SpringLoach/Vue/blob/main/learning/section2.md#路由懒加载)的方式。
- 在 `TabBarItem` 组件中进行[父传子](https://github.com/SpringLoach/Vue/blob/main/learning/section1.md#父子组件通信)来获取自定义属性 `path`，最后[通过代码跳转路由](https://github.com/SpringLoach/Vue/blob/main/learning/section2.md#通过代码跳转路由)。  
- 在 `App.vue` 中插入 `<router-view>`，以确定映射的组件显示的位置。

:bug: 通过代码跳转路由步骤，在 `this.$router.replace(this.path)` 后添加 `.catch(err => {})` 可以解决报错问题，但是否真的解决了呢？

----

#### 颜色动态控制  

1. 通过判断活跃路由是否为当前路由，选择对变量的赋值，从而进行条件渲染。  
2. 对于字体颜色，我们希望可以让外界动态决定，故需要设置自定义属性（用于获取颜色）并动态绑定样式。同时我们希望给它一个默认值，并添加到自定义属性中。  
3. 添加计算属性，通过判断活跃路由是否为当前路由，决定添加到[动态样式](https://github.com/SpringLoach/Vue/blob/main/learning/section1.md#绑定style)的内容。

----

#### 进一步抽离 

- src
  + components
    - MainTabBar.vue

1. 将 `App.vue` 当中 `TabBar` 模板和 `TabBarItem` 模板的内容转移至 `MainTabBar`。  
2. 在 `MainTabBar` 中注册 `TabBar` 组件和 `TabBarItem` 组件。  
3. 在 `App.vue` 中注册并使用 `MainTabBar` 组件。
4. 改写一下图片的路径。  

----

#### 起别名  

#cli-2

`项目文件/build/webpack.base.conf.js` 中的有一个 `resolve` 对象，其中的 `alas` 属性为别名。  
```
resolve: {
    ...,
    alias: {
        '@': resolve('src'),
        'asserts': resolve('@/asserts'),
        'components': resolve('@/components'),
        'views': resolve('@/views'),
    }
}
```

使用别名
```
/* src中使用需要前缀~ */
src="~assets/img/tabber/home.svg"

/* import则不用 */
import TabBar from 'components/tabbar/TabBar'
```
:bug: 实际测试时，除了默认配置的 `@`，其它都不能用。  

----

#### promise

```
new Promise((resolve,refuse) => {
  setTimeout(() => {
    console.log('Hello World!');
    }, 2000);
  }
)
```








