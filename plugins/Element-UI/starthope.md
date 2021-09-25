- [工作相关网站](#工作相关网站)
- [操作者相关账号更改](#操作者相关账号更改)
- [项目操作A](#项目操作A)
- [工作流程](#工作流程)
  + [工作流程_修改样式bug](#工作流程_修改样式bug)

#### 工作相关网站

名称 | 说明 | 网址
:- | :- | :-
AICP | / | http://starthope.tpddns.cn:8443/svn/AICP
腾讯邮箱 | / | https://exmail.qq.com/
蓝湖 | 设计稿 | https://lanhu.dm-ai.cn/web/#/item/project/board?pid=a694a765-e65e-4a5c-9871-0b9c2981ef0d
Github | 个人账号 | https://github.com/masterspringl
GitLab | 代码管理库、登录失败 | https://github.dm-ai.cn/users/sign_in
TAPD | 腾讯协作云平台，改样式bug | https://www.tapd.cn/
SVN | / | /

#### 操作者相关账号更改

[全局更改git用户信息](https://github.com/SpringLoach/origin-2021/blob/main/git/Git速查.md#提交操作者信息)    

名称 | 说明
:- | :- 
用户名 | masterspringl
邮箱 | fanhaodong@starthope.com.cn

更改SVN账户信息  
> 项目/桌面 → 右键 TortoiseSVN → 设置 → 已保存数据 → 清除 → 再次svn更新/提交 → 输入账号密码  

通过svn提交 
> 在文件栏，项目处右键 → svn提交  

#### 项目操作A  

顺序 | 步骤 
:- | :- 
① | 在 aicp_ui 项目作修改  
② | 一般每个组件的样式都不直接写，会单独导入样式文件
③ | 修改样式时，尽可能复用已有类名，而不是新增类名，好维护 
④ | 修改好后，拖拽对应文件到 `aicp-teaching..`
⑤ | 使用的分支叫 starthope
⑥ | 提交修改  

可能需要拖拽全局样式、特定组件样式、字体样式  

#### 工作流程  

#### 工作流程_修改样式bug  
> 对于一些在大部分页面重复的按钮样式，可以在全局文件将其统一修改。
> 
> 还强调一个查询头部样式的修改，以及将一些标签用 ant design 替代。  
> 
> 图标可以直接打开在 `font` 下的一个页面预览，`icon-` 拼接当作类名使用即可。

顺序 | 步骤 
:- | :- 
① | 在 TAPD 查看流转到手上的处理清单 
② | 在项目页面找到对应的位置
③ | 可以在蓝湖查看对应的设计稿，效果图
④ | 通过路径找到对应组件，如果是个标签页（不更改路由的子组件），一般以 `com-` 作为开头
⑤ | 点进对应的样式文件，搜索类名等  
⑥ | 项目处左键，更新SVN，提交SVN  
⑦ | 将消息流转为已处理









