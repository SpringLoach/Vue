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
  + 以往通过模块化开发的项目，还需要手动处理模块间的各种依赖（如导出导入）。  
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



