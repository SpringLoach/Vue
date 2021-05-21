#### 项目创建和GitHub托管的不唯一方法  

1. 使用 vue-cli-3 创建
```
vue create linshi
```
:bug: 这里不要选择版本，vue3 以上的版本在[安装路由](https://blog.csdn.net/m0_46442996/article/details/108961492)等方面都与原先完全不同。

2. 在 GitHub 上创建一个仓库
> 可以选择一个 `MIT license`，先不创建 `README` 文件了，会和脚手架自带的冲突。

3. 在 `clone` 复制地址，在需要建项目文件的地方执行下面代码
```
git clone https://github.com/SpringLoach/mail.git
```

4. 手动将 `linshi` 中的 `.git` 以外的文件复制到 `mail`

5. 进入到 `mail` 中，可以查看文件状态  
```
git status
```
6. 将文件添加到缓存区
```
git add .
```
7. 将文件添加到工作区
```
git commit -m '初始化项目'
```
8. 将文件添加到远程仓库
```
git push
```
9. 那么 `linshi` 这个文件夹就可以删除了  

----

#### 配置Git的环境变量

1. 忘记 git 安装在哪
> 随便找个文件夹，右键打开 Git Bash，执行
```
where git
```

2. [环境配置](https://blog.csdn.net/Andone_hsx/article/details/87937329)

----

#### 划分目录结构  

- src
  + assets
    - img
    - css
  + components
    - commom
    - content
  + views
  + router
  + store
  + network
  + commom
    - const.js
    - utils.js
    - mixin.js
  + App.vue
  + main.js 

 文件 | 作用
 :-: | :-: 
 components | 公共组件文件夹 
 common | 不同项目通用组件
 content | 和当前业项目业务相关组件 
 views | 对应的视图组件文件夹   
 router | 路由相关 
 store | 状态相关  
 network | 网络相关 
 commom | 公共的 js 文件 
 const.js | 公共常量 
 utils.js | 公共方法 
 mixin.js | 混入
 
#### css文件的引入  

- css
  + normalize.css
  + base.css
 
1\. 添加 `normalize.css`
> 一个常用的对 css 进行规范的文件，可以对标签风格进行统一。  

在[github](https://github.com/necolas/normalize.css)的 `Download` 上右键链接另存为。
 
2\. 拷贝 `base.css`  
> 就直接从项目那拿过来了，它已经引用了 `normalize.css`。  

```
/* App.vue */
@import "./assets/css/base.css";
```

#### 别名及代码风格规范  

- 项目文件
  - vue.config.js  
  - .editorconfig

`vue.config.js` 可以将配置整合到默认配置上。
```
module.exports = {
  configureWebpack: {
    resolve: {
      alias: {
        'assets': '@/assets',
        'commom': '@/commom',
        'components': '@/components',
        'network': '@/network',
        'router': '@/router',
      }
    }
  }
}
```
:snowflake: 在 HTML 中，使用别名时加 `~` 前缀；在 export 时则不用加。  

在高版本脚手架中，没有 `.editorconfig` 这个文件，从项目拷贝。
 
#### tabbar的引入  

将之前做的小项目 `tabbar` 中的两个文件夹引进。由于 `MainTabBar.vue` 中用于将图片和文字插入组件，将其归属为不可复用的业务组件；而 `tabber` 不关系嵌入的内容，属于真正封装的组件。   

- components
  + commom
    - tabber
      + TabBar.vue
      + TabBarItem.vue
  + content  
    - mainTabbar
      + MainTabBar.vue

```
npm install vue-router --save
```

1. 将 `MainTabBar.vue` 注册到 `App.vue` 并使用，注意**路径**。 

2. 修改 `MainTabBar.vue` 中图片和引用的**路径**。  

3. 将图片，router 下的 `index.js`，views 拿过来。  

:bug: 经测试，vue-cli 在 4.5 版本中使用 `<keep-alive>` 和 `<router-view/>` 来[缓存组件](https://github.com/SpringLoach/Vue/blob/main/learning/section2.md#路由中使用keep-alive)是没问题的。  

#### 改图标  

将项目文件中 `public` 下的 `favicon.ico` 更替为自己需要的图片。 

:palm_tree: `public` 下的 `index.html` 中使用了 `<%= BASE_URL %>` ，这是 jsp 的语法，用于动态获取路径，表示当前文件夹下。


#### 首页导航栏的封装和使用  
> 需要搭建的项目中，每个页面都有顶部导航栏，有的只有文字，有的两侧有图片，甚至有动态属性，选项卡。此时封装需要考虑到拓展性，要预留插槽。  

- commom
  + navbar
    - NavBar.vue  

HTML   
> 使用左中右三个具名插槽，分别定义样式。

CSS
> 在该组件中添加公共的样式。

采用 flex 布局，左右两个插槽固定宽度，中间插槽占据全部剩余宽度（`flex: 1`）；对整体设置水平居中并添加阴影效果。

- 顶部导航高度一般为 44 px，加上状态栏则为 64 px

- 对于块级元素，设置行高并添加了内容时，可以不加高度（等于行高），这样做可以垂直居中。  

导入 `Home.vue` 中，并对该模块设置字体和背景颜色。  


#### 请求首页的多个数据  

将先前封装好的 `network` 中的 `request.js` 拉过来。  

安装axios

为了减少耦合度，以及方便以后的管理，建立一个新的文件用于管理首页 `Home.vue` 的请求，这样就可以直接在 `Home.vue` 中添加异步处理了。  

- network
  + home.js

```
import {request} from "./request";

export function getHomeMulidata() {
  return request({
    url: '/home/multidata'
  })
}
```

将文件导入到 `Home.vue` 中。在首页组件创建好后请求数据，并将请求的数据保存到 `data` 中。

```
data() {
  return {
    banner: [],
    recommends: []
  }
},
created() {
  getHomeMulidata().then(res => {
    this.banners = res.data.banner.list;
    this.recommends = res.data.recommend.list;
  })
}
```













