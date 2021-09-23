
- filepackage
  + node_modules
  + public
   - favicon.ico
   - index.html
  + src
    - api
      + demo
        - index.ts
      + hardware
        - index.ts
      + index.ts
    - assets
      + css
        - Hardware
        - Lab
        - ...
        - global.scss
        - login.scss
      + fonts
    - components
    - js
      + demo
      + hardware
      + utils
    - router
      + index.ts
      + router.demo.ts 
    - store
      + index.ts
    - views
      + Demo
        - Home.vue
        - demo-form.vue
        - ...
      + Hardware
        - Home.vue
        - hardeware-info.vue
      + About.vue
      + Home.vue
      + Login.vue
    - App.vue
    - main.ts
  + static
  + ..

文件 | 说明  
:- | :- 
api | 放置对应各个页面的文件夹
api 所包含页面文件内的 index.ts | 包括页面具体请求方法
api 下的 index.ts | 导出所有页面的请求方法  
router 下的 index.ts | 主要几个路由，并导入其他页面路由
router 下的 router.demo.ts等 | 页面包含的路由
store | 登录、加载相关
static | 用于放置图片

