# JavaScript基本语法 、 数据类型与变量 （2017.09.12）
1. `let` and `var`
```
let作用于代码块使用let命令声明变量之前，该变量都是不可用，即let不会出现变量提升的情况，const也是同样的情况
var作用于全局，
```
2. `块作用域`
```
考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数 ; 确实需要，也应该写成函数表达式，而不是函数声明语句
```
3. `函数`
 ` 函数声明语句`
```
{
  let a = 'secret';
  function f() {
    return a;
  }
}
```
`函数表达式`
```
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```
4. `块域声明函数规则`
另外，还有一个需要注意的地方。`ES6` 的`块级作用域允许声明函数的规则`，只在`使用大括号的情况下成立`，如果没有使用大括号，就会报错。
```
// 不报错
'use strict';
if (true) {
  function f() {}
}

// 报错
'use strict';
if (true)
  function f() {}
```
5. `do的使用`
```
let x = do {
  let t = f();
  t * t + 1;
};


上面代码中，变量x会得到整个块级作用域的返回值。
```
6. `const`
```
const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值
```
7. `对象数组的注意事项`
```
但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指针，const只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。
```
8. `typeof`
```
JS中的变量是松散类型（即弱类型）的，可以用来保存任何类型的数据。
typeof 可以用来检测给定变量的数据类型，可能的返回值：
1. 'undefined' --- 这个值未定义；
2. 'boolean'    --- 这个值是布尔值；
3. 'string'        --- 这个值是字符串；
4. 'number'     --- 这个值是数值；
5. 'object'       --- 这个值是对象或null；
6. 'function'    --- 这个值是函数。
```

9. `=>的意思`
```
=>是es6语法中的arrow function

(x) => x + 6

相当于

function(x){
    return x + 6;
};
```

10. `声明变量的6种方法`
```
var 
function
let
const
import
class
```
11. `顶层对象`
```
  1. 顶层对象，在浏览器环境指的是window对象，在Node指的是global对象
  2.ES6中，var命令和function命令声明的全局变量，依旧是顶层对象的属性；let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。
```
12. `勉强可以使用的2种方法：来汲取顶层的对象`
```
// 方法一
(typeof window !== 'undefined'
   ? window
   : (typeof process === 'object' &&
      typeof require === 'function' &&
      typeof global === 'object')
     ? global
     : this);

// 方法二
var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};
```
13. `垫片库system.global模拟了这个提案，可以在所有环境拿到global` `保证各种环境里，global对象都是存在的`
```
// CommonJS 的写法
require('system.global/shim')();

// ES6 模块的写法
import shim from 'system.global/shim'; shim();
```
14. `将顶层对象放入变量global。`
```
// CommonJS 的写法
var global = require('system.global')();

// ES6 模块的写法
import getGlobal from 'system.global';
const global = getGlobal();
```
15. `Number`
```
  1. JavaScript`不区分整数和浮点数`，统一用Number表示
  2. Number可以`直接做四则运算`，规则和数学一致

123;       // 整数123
0.456;    // 浮点数0.456
1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
-99;      // 负数
NaN;      // NaN表示Not a Number，当无法计算结果时用NaN表示
Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
```
16. `字符串`
```
字符串是以单引号'或双引号"括起来的任意文本，比如'abc'，"xyz"等等。请注意，''或""本身只是一种表示方式，不是字符串的一部分，因此，字符串'abc'只有a，b，c这3个字符
```
17. `数据类型`
```
  1. JavaScript允许对任意数据类型做比较
  2. 始终坚持使用`===比较`

  第一种是==比较，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果；

  第二种是===比较，它不会自动转换数据类型，如果数据类型不一致，返回false，如果一致，再比较
  ```
18. `NaN` : NaN表示Not a Number，当无法计算结果时用NaN表示
 ```
  1. NaN这个特殊的Number`与所有其他值都不相等`，包括它自己

  NaN === NaN; // false
  isNaN(NaN); // true
  ```
19. `浮点数相等的比较`
  ```
  计算机无法精确表示无限循环小数。要比较两个浮点数是否相等，只能计算它们之差的绝对值，看是否小于某个阈值

  Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true
  ```
20. `null和undefined`
  ```
  多数情况下，我们都应该用null。undefined仅仅在判断函数参数是否传递的情况下有用
  ```
21. `JavaScript的数组可以包括任意数据类型`
  ```
  [1, 2, 3.14, 'Hello', null, true];

  出于代码的可读性考虑，强烈建议直接使用[],而不用new Array(1, 2, 3); // 创建了数组[1, 2, 3]
  ```
22. `对象`
  ```
    1. JavaScript的对象是一组由键-值组成的无序集合
    2. 键都为字符串类型，又称为对象的属性
    3. 值 任意数据类型
    4.使用方式 对象变量。属性名
  ```
23. `变量名` ：大小写英文、数字、$和_的组合，且不能用数字开头 不能是JavaScript的关键字
  ```
  var a;              // 申明了变量a，此时a的值为undefined
  var $b = 1;        // 申明了变量$b，同时给$b赋值，此时$b的值为1
  var s_007 = '007'; // s_007是一个字符串
  var Answer = true; // Answer是一个布尔值true
  var t = null;      // t的值是null
  ```
24. `赋值`
  ```
  使用等号=对变量进行赋值,注意只能用var申明一次
  ```
25. `静态语言在定义变量时必须指定变量类型`
  ```
  如Java为静态语言
  int a = 123;  // a是整数类型变量，类型用int申明
  a = "ABC";    // 错误：不能把字符串赋给整型变量
  ```
26.  `全局变量`
  ```
  如果一个变量没有通过var申明就被使用，那么该变量就自动被申明为全局变量：
  i = 10; // i现在是全局变量
  ```
27. `strict模式的作用`
```
  ECMA在后续规范中推出了strict模式，在strict模式下运行的JavaScript代码，强制通过var申明变量，未使用var申明变量就使用的，将导致运行错误。

  启用strict模式的方法是在JavaScript代码的第一行写上：
  'use strict';

  为了避免这一缺陷，所有的JavaScript代码都应该使用strict模式
```





