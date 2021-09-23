
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

> script 需要添加 `lang="ts"`  
> 
> 一开始导入的内容是为 ts 服务  
> 
> 注意数据区域  
```
<template>
</template>

<script lang="ts">
	import {
		Vue,
		Component,
		Emit,
		Prop
	} from "vue-property-decorator";

	@Component
	export default class demo_home extends Vue {
		openKeys: Array < string > = [];

		//===================================computed区域begin
		
		//===================================computed区域end
		titleClick(): void {};
		handleClick(): void { 
		};
		onOpenChange():void{};
		handlerMenuClick():void{
			let breadcrumbData = [{
				name:"登录",
				path:"/login"
			},{
				name:"上一级",
				path:"back"//碰到这个会执行返回上一页操作
			},{
				name:"我是上一页面传来的"
			}];
			this.$router.push({
				path:"/demo/demo-list",
				query:{
					breadcrumbData:JSON.stringify(breadcrumbData)
				}
			});
		};
	};
</script>

<style lang="scss" scoped>
</style>
```

```
getMsgFormSon(data:any){
  this.stopOrResume = data
};

updateDmOpenlink(...args:any[]) {
  console.log("updateDmOpenlink");
};

initData(){
  (this.$refs['usbDownload'] as any).initPageData();
};
```

```
<template>
</template>

<script lang="ts">
	import {Vue,Component,Emit,Prop} from "vue-property-decorator";
	
	import empty from "../../components/empty.vue";
	@Component({
		components: {
			empty
		},
	})
	export default class tempDemo extends Vue{
		//===================================data区域begin
		name:string = "";
		user:any = {}; 
		pageData:any = {
			helpList:[],
			labPlatformList:[]
		};
		//===================================data区域end
		
		//===================================prop区域begin
		//主题
		@Prop({default: 'white'}) themeMode?: string;
		//是否显示首页
		@Prop({default: true}) showHome?: boolean;
		//是否显示导航
		@Prop({default: true}) showNavigationTitle?:boolean;
		//导航部分的内容
		@Prop({default: ''}) navigationTitle?: string;
		//是否要求登录：不要求登录，与登录相关的数据将全部不显示，与登录相关的请求不执行
		@Prop({default: true}) requireLogin?:boolean;
		//是否显示个人中心
		@Prop({default: true}) showUserCenter?:boolean;
		//===================================prop区域end
		
		
		//===================================computed区域begin
		get demoCompute():boolean {
		    return true;
		};
		//===================================computed区域end
		
		
		//===================================methods区域begin
		demoMethod():void{
			this.$confirm({
				title: '提示',
				content: "是否退出登录?",
				onOk() {
					console.log('onOk');
				},
				onCancel() {
					console.log('Cancel');
				}
			});
		};
		//===================================methods区域end
		mounted(){
		};
		created() {

		};
	}
</script>

<style lang="scss" scoped>
	@import '../../assets/css/login.scss';
</style>

```

```
		//===================================data区域begin
		form: any = {

		};
		fileList: any = [];
		headers: any = {};
		previewImage: string = '';
		previewVisible: boolean = false;
		//===================================data区域end
```

```
	//1、data区
	//2、computed：计算属性
	//3、methods
	//		包含事件方法（handle开头），普通方法（包括提供给父页面调用的方法），普通方法一律放到事件后
	//4、emit 提供给子页面调整的则使用emit开头
```
