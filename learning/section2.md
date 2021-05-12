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














