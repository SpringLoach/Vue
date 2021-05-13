#### JavaScript原始功能  

多人开发时，引入多个 `.js` 文件特别是其中包含的全局变量会导致混乱。  

**#实现模块化**
> ES5 及以前，只能通过下面这样通过匿名函数实现闭包（立即执行）的方式来避免全局变量的问题。

```
(function() {
    var hi = 'nihao';
    var show = function() {
        document.write('from a.js');
    }
})()
```

但此时，其它 `.js` 文件要复用这些变量，只能重新创建。所以最好将需要的变量返回到一个对象中，用模块专属的全局变量接受。  

```
/* a.js */
var adata = (function() {
    var out = {};
    var hi = 'nihao';
    var show = function() {
        document.write('from a.js');
    }

    out.hi = hi;
    out.show = show;
    return out;
})()

/* b.js */
;
(function() {
        adata.show();
        console.log(adata.hi);
})()
```
:snowflake: 在每个 `.js` 文件开头加上 `;`，可以避免引用时的一些冲突。

**#CommonJS规范**
> 这种模块化规范被 Node.js 很好的实现了。当然了，缺乏底层支撑原生的 Javascript 不能直接这样做。

```
/* 从a.js导出 */
var hi = 'nihao';
var show = function() {
    document.write('from a.js');
}

module.exports = {
    hi,
    show
}

/* 从b.js导入 */
let {hi, show} = require('./a.js')
```
:star2: 这里使用了 ES6 的对象字面量的增强写法和对象解构。

**#ES6方法**

1. `module` 类型能够将作用域隔离，因此哪怕在文件中使用 `var` 也不会产生冲突。  
2. 为了能够从其它文件获取需要的数据，需要通过 `export` 关键字导出，`import` 关键字导入。

```
/* index.html */
<script src="a.js" type="module"></script>
<script src="b.js" type="module"></script>

/* a.js */
let hi = 'nihao';
let show = function() {
    alert('from a.js');
}

export {
    hi, show
}

/* b.js */
import {hi, show} from "./a.js";

show();
console.log(hi);
```
:star2: 使用这个方法会有跨域的问题，在 VScode 中使用插件 `Live Server` 可以解决。  

export的更多用法  
```
/* a.js */
export let x = 3;
export function y(a, b) {return a + b};
export class z {};


/* b.js */
import {x, y, z} from "./a.js";

console.log(x);
console.log(y(6,6));
let xxx = new z;
```

导出自命名数据

1. 使用 `export default` 导出可以给功能自命名。但在同一模块中，只允许存在一个。    
2. 导入时加入的 `objectA` 可替换成自己想要的名字。  
```
/* a.js */
export default {
    hi, show
}

/* b.js */
import objectA from "./a.js";

objectA.show();
console.log(objectA .hi);
```

从文件导入所有数据并添加到对象
```
import * as objectA from "./a.js";

objectA.show();
```
:herb: 使用通配符不能导入 `export default` 中的数据。  

----

**#webpack的模块化和打包**   

- 模块化  
  + 以往通过模块化开发的项目，在模块间会产生各种依赖（如导出导入）。  
  + 而 webpack 其中一个核心就是让我们可能进行模块化开发，并且会帮助我们处理模块间的依赖关系。
- 打包  
  + 打包就是将 webpack 中的各种资源模块进行打包合成一个或多个包。
  + 打包的过程中，还可以对资源进行处理，比如压缩图片，将 scss 转成 css，将 ES6 语法转成 ES5，将 TypeScript 转化为 JavaScript等。  

**#和 grunt/gulp 的对比**  

- grunt/gulp 的核心是 Task。  
  + 配置一系列的 task， 并且定义 task 要处理的事务（例如 ES6、ts 转化，图片压缩，scss 转化）。  
  + 然后让 grunt/gulp 依次执行这些 task，让整个流程自动化。      
  + 故它们也被称为前端自动化任务管理工具。  
- 当工程模块化依赖非常简单，甚至没有用到模块化的概念时，只需要处理事务时，就可以使用 grunt/gulp。  
- 与 webpack 的区别  
  + grunt/gulp 更强调前端流程的自动化。  
  + webpack 更加强调模块化开发管理，而文件压缩合并、预处理等功能，是它附带的功能。  

**#使用webpack的前置条件**  

![webpack打包](./img/webpack打包.jpg)

- 运行 webpack 必须依赖 `node环境`   
- `node环境` 为了可以正常的执行不同的代码，其中必须包含各种依赖的包  
- `node环境` 自带 npm 工具（node packages manager）管理各种包
 
 步骤 | 操作 | 备注
 :-: | :-: | :-:
 打开命令行 | 使用快捷键 `window` + `R`，输入 `cmd` | /
 查看 node 版本 | `node - v` | 如果没安装，到官网下载
 全局安装 webpack | `npm install webpack@3.6.0 -g` | 版本号是暂时指定的
 查看 webpack 版本 | `webpack - v` | /

#### 使用webpack  

- 项目文件
  + src
    - main.js
    - js
      + mathUtils.js
      + ...
    - css
      + normal.css
      + special.less
      + ...
    - img
  + dist
  + index.html

 文件 | 说明
 :-: | :-: 
 src文件夹 | 源码，进行开发的地方
 dist文件夹 | 翻译为发布。最终会打包、发送到服务器
 index.html | 最终也会添加到 `dist` 中
 main.js | 源文件，也可以命名为 index.js
 
1. 建立一些**存在依赖关系的**模块
```
/* main.js*/
import {fruit, eat} from "./js/mathUtils"

console.log(fruit);
eat();

/* mathUtils.js */
let fruit = '波罗蜜'
function eat() {
    console.log('午饭');
}

export {fruit, eat}
```
> 这里使用了 ES6 的模块化规范，也可以使用 CommonJS 规范。  
> 
> 使用 webpack 打包时，`import .. from` 后面的文件可以不加后缀 `.js`

2. 打包源文件 
  - 在 VScode 中调出终端 `Ctrl`+`~` ，并切换到 cmd[Command Prompt\]  
  - 使用 `cd` 到项目文件
  - 将源文件打包 `webpack ./src/main.js ./dist/bundle.js`
  
:star2: webpack 将自动处理模块间的依赖关系。  
:herb: 找不到 webpack 时试试以管理员身份打开 VScode。 

3. 引用打包后的文件  
```
/* index.html */
<script src="./dist/bundle.js"></script>
```

4. 更改源码后的更新  
```
重新打包，即再次执行第二步
```

#### 配置webpack文件  

**#配置默认打包源文件路径**

1. 添加配置文件 `webpack.config.js`

- 项目文件
  + webpack.config.js

 键 | 说明
 :-: | :-: 
 entry | 需要打包的源文件
 path | 打包的绝对路径
 filename | 打包后的文件
 ... | node 相关的一些内容
 require | 导出
 path.resolve() | 拼接路径
 \_\_dirname | 当前文件所在路径，注意是双下划线

```
/* webpack.config.js */
const path = require('path')

module.exports = {
    entry: './src/main.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    }
}
```
上面代码中的首行中使用了需要依赖 node 相关的包，故需要进行安装，此时最好初始化 `node`。  

2. 初始化 node，生成 package.json 文件
```
/* cmd，项目文件下 */
npm init
```

 提示 | 操作 | 说明
 :-: | :-: | :-: 
 packagename | meetwebpack | 不能使用中文和符号
 ... | 回车 | /
 entry point | index.js | 这里是随便写的，因为暂时没用上
 ... | 回车 | / 
 Is this OK？| 回车 | /
 
3. 若此时需要依赖其它的东西  
> 将会根据 package.json 中的依赖进行安装。  
```
/* cmd，项目文件下 */
npm install
```

4. 以后打包时，就可以简化命令了
```
/* cmd，项目文件下 */
webpack
```

**#配置命令（快捷键）**  

1. 打开 `package.json` 文件  
2. 在 "script"对象下添加键值对  

```
"script": {
    ...,
    "build": "webpack"
},
```

3. 以后打包时，就可以简化命令了
```
/* cmd，项目文件下 */
npm run build

/* 相当于 */
webpack
```

**#webpack的局部安装**  
> 项目一般在（本地）局部安装 webpack，因为不同的项目依赖的版本不同。  

```
/* cmd，项目文件下 */
npm install webpack@3.6.0 --save-dev
```
> `--save-dev`：开发时依赖，项目打包后不需要继续使用的东西。  

**#webpack的全局使用与本地使用**  
- 全局使用  
  + 项目文件下使用 `webpack`  
- 局部使用    
  + 项目文件下使用先前配置好的脚本对应命令 `npm run build` ，优先在本地查找变量  
  + 项目文件下的 node_modules/.bin/webpack 中使用相关命令  

:snowflake: `package.json` 中的脚本在执行时，先寻找本地的 node_modules/.bin 路径中的命令，找不到就到全局环境找。  

----
#### 拓展loader
> 给 webpack 拓展对应的 loader，就可以具备加载css、图片等的能力。  

1. 通过 npm 安装需要使用的 loader，指令可以到[官网](https://www.webpackjs.com/loaders/)中找。  

2. 在 `webpack.config.js` 中的 `modules` 关键字下进行配置。

**#webpack中使用css文件**  
> 

1. 添加依赖
> 在 `main.js` 最后加上这段代码，为指定 css文件添加依赖。  
```
require("./css/normal.css")
```

2. 安装loader  
```
/* 项目文件下 */
/* css-loader，负责将 css 文件进行加载 */
npm install css-loader@2.0.2 --save-dev

/* style-loader，负责将样式添加到 DOM 中 */
npm install style-loader@0.23.1 --save-dev
```

3. 配置loader    
> 创建 `module.exports` 并添加相应对象。  
```
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [ 'style-loader', 'css-loader' ]
      }
    ]
  }
}
```
:star2: 使用多个 loader 时，使用顺序是从右向左。

4. 这时就可以正常的将 css 文件打包了

**#webpack中使用less文件**  

0. 新建文件 `special.less`
```
@fontSize: 100px;

body {
    font-size: @fontSize;
}
```

1. 添加依赖
> 在 `main.js` 最后加上这段代码，为指定 less文件添加依赖。  
```
require("./css/special.less")
document.writeln("<h2>你好，生活！</h2>");
```

2. 安装loader  
```
/* less-loader */
npm install --save-dev less-loader@4.1.0 less@3.9.0
```

3. 配置loader
```
把 rules 数组中的对象添加到相应位置。
```

**#webpack中使用图片文件**  

0. 更新文件 `normal.css`
```
body {
    background: url(../img/阿米娅2.png);
}
```
1. 添加依赖
```
之前已经将该文件添加了依赖
```

2. 安装loader  

- `url-loader` 像 `file loader` 一样工作，但如果文件小于限制，可以返回 data URL。  
- `file-loader` 将文件发送到输出文件夹，并返回（相对）URL。  

```
/* url-loader */
npm install --save-dev url-loader@1.1.2

/* file-loader */
npm install --save-dev file-loader@3.0.1
```

3. 配置loader  
> 其中的 `limit` 将限制图片文件大小。  
```
...
options: {
    limit: 17000
}
...
```
:href: 只需要配置 `url-loader`，不然会报错。  

4. 图片限制  
- 若图片小于限制   

  + 将图片编译成 base64 字符串形式。  

- 若图片大于限制    

  + 需要使用 `file-loader` 模块进行加载。且将会在 `dist` 目录下，生成一个通过哈希算法命名的图片文件。  
  
  + 由于此时的 `index.html`文件不在 `dist` 中，默认找的路径不对。需要在 `webpack.config.js` 中添加如下代码，拼接路径。
```
output: {
    ...
    publicPath: 'dist/'
}
```

5. 更改图片默认命名方式  
> 默认的 32 位 hash 值，不一定能满足需求。此时可以在 `webpack.config.js`-`url-loader` 对应的 `options` 中，添加如下选项。    

```
name: 'img/[name].[hash:8].[ext]'
```

 选项 | 说明
 :-: | :-: 
 img | 文件要打包到的文件夹
 name | 获取图片原来的名字
 hash8 | 防止图片名称冲突
 ext | 使用图片原来的拓展名
 [\] | 代表变量

**#webpack中将ES6语法转化为ES5**   

1. 安装loader  
```
/* es6 转换成 es5 */
npm install --save-dev babel-loader@7.1.5 babel-core@6.26.3 babel-preset-es2015@6.24.1
```

2. 配置loader  
```
/* 按官网配置后，修改这个配置 */
presets: ['es2015']
```
:star: 其中的 `exclude` 属性表示排除哪些文件。  





