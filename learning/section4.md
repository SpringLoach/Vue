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

## 首页

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

安装[axios](https://github.com/SpringLoach/Vue/blob/main/learning/section3.md#axios框架的基本使用)  

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
import {getHomeMulidata} from "network/home"

data() {
  return {
    banners: [],
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

#### 轮播图的展示  

将项目中的 `swiper` 拉过来。  

- components
  + commom
    - swiper
      - index.js
      - Swiper.vue
      - SwiperItem.vue

其中的 `index.js` 将另外两个组件统一导出，这样在其它组件中使用时就不用导入两次了。  

由于 `Home.vue` 只需关心功能的集结，轮播图的功能实现我们可以新建一个组件来完成，此时需要[从父组件获取数据](https://github.com/SpringLoach/Vue/blob/main/learning/section1.md#父子组件通信)。

- views
  + home
    - childComps
      + HomeSwiper

使用方法为 `sweiper` （预留了插槽）包围 `swiper-item` 。由于有多个轮播图，可以直接[遍历](https://github.com/SpringLoach/Vue/blob/main/learning/section1.md#v-for遍历数组和对象)数组。轮播图可以实现链接的功能，所以用 `<a>` 包围 `<img>`。  

```
<swiper>
  <swiper-item v-for="item in banners :key="item.title">
    <a :href="item.link">
      <img :src="item.image" alt="" />
    </a>
  </swiper-item>
</swiper>

/* 新增 option */
props: {
  banners: {
    type: Array,
    default() {
      return []
    }
  }
}
```

最后注册到父组件并在使用时传递数据就可以了。  

:bug: 有时候轮播图是在轮播，但是没有图片，一片空白，只有第一张图。有小伙伴认为是异步操作导致的。：banners的动态绑定，发现所需的数据是经过异步请求传过来的。我们也知道，异步请求的结果是在回调函数中，所以先渲染的模板可能会出现空数据的情况，即还没有拿到返回的数据，后来拿到了数据，但模板已经渲染完了，并没有响应式的刷新。  
:bug: 但我发现这个现象仅发生在使用 `F12` 之后的第一次刷新。  
:herb: 之后尝试用状态管理调整一下。  

#### 推荐信息的展示  

- views
  + home
    - childComps
      + RecommendView.vue

导入 `home.vue` 并从父组件请求数据；

```
<div class="recommond">
  <div v-for="item in recommends">
    <a :href="item.link">
      <img :src="item.image" alt="">
      <div>{{item.title}}</div>
    </a>
  </div>
</div>
```
:snowflake: 给所有弹性项目加上 `flex: 1` 可以做到在一行内均等分布。  

#### 特性类别的展示 

- views
  + home
    - childComps
      + FeatureView.vue

```
<div class="feature">
  <a href="https://act.mogujie.com/zzlx67">
    <img src="~assets/img/home/recommend_bg.jpg" alt="">
  </a>
</div>
```

然后把置顶和置底的导航栏的固定以及遮蔽问题解决 `positon: sticky`。  

#### 表格切换控制的封装  

- components
  + content
    - tabControl
      + TabControl.vue

由于这个组件在不同页面使用时，仅文字不同。不包含图片、ul、不同布局。建议不用插槽，而是可以直接从父组件获取数据，使用 `v-for` 根据数量创建。  

需要动态添加一个激活的类样式  

```
<div class="tab-control">
  <div v-for="(item, index) in titles"
      class="tab-control-item"
      :class="{active: index=== currentIndex}" @click="itemClick(index)" >
    <span>{{item}}</span>
  </div>
</div>

data() {
  return {
    currentIndex: 0;
  }
},
methods: {
  itemClick(index) {
    this.currentIndex = index;
  }
}
```

由于并非所有页面中都有粘连顶部的功能，所以将这个样式设置到 `home.vue` 中。

:snowflake: 在向页面导入其它组件时，可以按私有组件、公用组件、插入数据的顺序进行排序。  

#### 保存商品的数据结构  
> 考虑到切换表格时还有进行加载会给用户带来不好的体验，先将不同表格的前面一部分直接请求下来，只有下拉加载更多时，才会继续请求数据。  

goods：（流行/新款/精选）  

其中 `page` 用于记录当前的页数，`list` 用于记录已经加载的数据。
```
goods: {
  'pop': {page: 5, list: [150]},
  'new': {page: 2, list: [60]},
  'sell': {page: 1, list: [30]}
}
```

初始化  
```
/* Home.vue */
data() {
  return {
    goods: {
      'pop': {page: 0, list: []},
      'new': {page: 0, list: []},
      'sell': {page: 0, list: []}
    }
  }
}
```

#### 商品数据的请求和保存  

封装请求并在 `Home.vue` 导出，只要传入特定的 `type` 和 `page` 就可以请求到对应的数据了。
```
/* Home.js */
export function getHomeGoods(type, page) {
  return request({
    url: '/home/data',
    params: {
      type,
      page
    }
  })
} 
```

请求的时机应该是组件创建好之后 `created`，这里把这里的方法封装到了 `methods` 选项中。

- 由于接口的设计，请求下一页只需要在当前请求页的基础上加一即可。  
- 进行数组合并将请求的数据添加到之前的数据中。  
- 请求结束后当前请求页 +1。  

```
created() {
  this.getHomeMultidata();
  this.getHomeGoods('pop');
  this.getHomeGoods('new');
  this.getHomeGoods('sell');
},
methods: {
  getHomeMultidata() {
    getHomeMultidata().then(res => {
      this.banners = res.data.banner.list;
      this.recommends = res.data.recommend.list;
    })
  },
  getHomeGoods(type) {
    const page = this.goods[type].page + 1;
    getHomeGoods(type, page).then(res => {
      this.goods[type].list.push(...res.data.list);
      this.goods[type].page += 1;
    })
  }
}
```

#### 商品数据的展示  
> `GoodsList.vue` 从父组件获取 `list` 数据；`GoodsListItem.vue` 则从父组件获取 list 中每一个具体的 `对象`。  

- components
  + content
    - goods
      + GoodsList.vue
      + GoodsListItem.vue

:snowflake: 仅当需要复用的组件结构不同时，采用插槽。仅数据不同时，不必使用。  

#样式  
1. 采用弹性布局时，要保证元素之前是直接的父子关系，所有 `v-for` 要直接用到组件上。  
2. 设置[文本溢出](https://www.w3school.com.cn/tiy/t.asp?f=css3_text-overflow)效果。   
3. 设置图片[固定宽高比](https://blog.csdn.net/qq_26173001/article/details/102770651)。  
4. 利用弹性布局设计两栏效果。  

```
.goods-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  padding: 3px;
}
.list-item {
  width: 48%;
}
```

#### TabControl切换商品  
> 当点击不同的分类时，切换对应的展示数据。  

将点击的索引值[传给父组件](https://github.com/SpringLoach/Vue/blob/main/learning/section1.md#父子组件通信)，并判断具体需要展示的数据，再通过计算属性 `showGoods` 保存需要展示的数据。  
```
/* TabControl.vue */
methods: {
  itemClick(index) {
    this.$emit('tabClick', index)
  }
}
```

#### 实现手机端浏览  
使手机和电脑连接在同一个 wifi 下，通过电脑的 [IPv4](https://jingyan.baidu.com/article/6079ad0e58c8c369ff86db9c.html) 地址加上 `:8080/home` 即 `localhost` 后面的那段即可连接。  

#### Better-scoll的安装和使用  
> 这个框架提供给了移动端更顺滑的滚动能力（减少了卡顿，拖拽增强，提供了弹簧效果）。  

安装  
```
/* 项目文件下 */
npm install --save better-scroll@1.13.2  
```
:bug: 如果出现找不到声明文件的错误，尝试卸载重装最新版本并重新打开 `VS Code`。  

使用测试
/* Category.vue */
```
import BScroll from 'better-scroll'

data() {
  return {
    scroll: null
  }
},
mounted() {
  this.scroll = new BScroll(document.querySelector('.wrapper'), {})
}

/* CSS */
.wrapper {
  background-color: skyblue;
  height: 200px;
  overflow: hidden;
}
```
:bug: 不能在 `created` 的处理程序中获取元素，此时尚未渲染。  
:snowflake: 必须对挂载元素设置一个高度。  

#### Better-scroll的基本使用
> BScroll 实例至少需要接受一个元素作为容器元素，在旧版本中这个容器内的第一个元素会被添加上相应的功能，新版本中添加了[更多功能](https://better-scroll.gitee.io/docs/zh-CN/guide/base-scroll-options.html)。  

第二个参数为一个对象，可以在其中配置一些 option

 选项 | 说明
 :-: | :-: 
 probeType | 设置位置侦测，值为 3 时侦测所有滚动   
 click | 设置为 true 时，开启浏览器的原生 click 事件
 pullUpLoad | 设置为 true 时，监听 `pullingUp` 事件，滚动到底部时发生

```
/* 引入文件后 */
const bscroll = BetterScroll.createBScroll(document.querySelector('.wrapper'), {
  probeType: 3,
  click: true,
  pullUpLoad: true
})

bscroll.on('scroll', (position) => {
  console.log(position);
})

bscroll.on('pullingUp', () => {
  console.log("加载更多");
  setTimeout(() => {
    bscroll.finishPullUp()
  }, 2000)
})
```
:snowflake: 该实例的 `on` 方法接受一个事件和处理程序。  
:snowflake: 默认情况下，滚动到底部的处理程序只会执行一次，除非调用了该实例的 `finishPullUp()` 的方法，该方法通常在新的数据展示完成后调用。  

#### Better-scroll的封装和使用  

- commom
  + scroll
    - Scroll.vue

```
<div class="wrapper" ref="wrapper">
  <div class="content">
    <slot></slot>
  </div>
</div>

import BScroll from 'better-scroll'

data() {
  return {
    scroll: null
  }
},
mounted() {
  this.scroll = new BScroll(this.$refs.wrapper, {
    observeDOM: true,
    click: true
  })
}
```
:snowflake: `ref` 绑定在组件中时，`this.$refs.refname` 获取到的是一个[组件对象](https://github.com/SpringLoach/Vue/blob/main/learning/section1.md#父访问子)； `ref` 绑定在元素中时，获取到的则是当前组件中的该元素。  


导入并使用该组件，并根据需求设置高度  
```
/* Home.vue */
<scroll class="content">
  // 顶部导航栏和底部导航栏以外的内容
</scroll>
```

定义的样式仅在当前组件中起作用
```
<style scoped>
.content {
  height: calc(100vh - 44px - 49px);
}
</style>
```
:snowflake: `calc()` 为 CSS3 中新增的计算值，`vh` 表示视口高度。  


