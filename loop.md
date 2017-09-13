#  循环笔记 (2017.09.12)
1. `for in`
`for循环`的一个变体是`for ... in循环`，它可以把一个对象的`所有属性`依次循环出来：
```
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    alert(key); // 'name', 'age', 'city'
}
要过滤掉对象继承的属性，用`hasOwnProperty()`来实现：

var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    if (o.hasOwnProperty(key)) {
        alert(key); // 'name', 'age', 'city'
    }
}
由于Array也是对象，而它的每个元素的索引被视为对象的属性，因此，for ... in循环可以直接循环出Array的索引：

var a = ['A', 'B', 'C'];
for (var i in a) {
    alert(i); // '0', '1', '2'
    alert(a[i]); // 'A', 'B', 'C'
}
for ... in对Array的循环得到的是String而不是Number。 
```
2. `Map`
```
avaScript的默认对象表示方式{}可以视为其他语言中的Map或Dictionary的数据结构，即一组键值对。

但是JavaScript的对象有个小问题，就是键必须是字符串。
实际上Number或者其他数据类型作为键也是非常合理的,为了解决这个问题，最新的ES6规范引入了新的数据类型Map
```
```
map的使用
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
```
3. `set` : Map和Set是`ES6标准新增的数据类型`
```
Set和Map类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在Set中，没有重复的key,重复元素在Set中自动被过滤.
var s = new Set([1, 2, 3, 3, '3']);
s; // Set {1, 2, 3, "3"}
注意数字3和字符串'3'是不同的元素。

通过add(key)方法可以添加元素到Set中，可以重复添加，但不会有效果：

>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.add(4)
>>> s
{1, 2, 3, 4}
通过delete(key)方法可以删除元素：

var s = new Set([1, 2, 3]);
s; // Set {1, 2, 3}
s.delete(3);
s; // Set {1, 2}
```
4. `iterable` : 迭代
    `set map array`都属于`iterable类型`
具有 iterable类型的集合可以通过`for ... of`循环来遍历-----`ES6`引入的新语法

5. `函数`就是最基本的一种代码`抽象的方式`
6. `定义函数的两种方式`
```
法一：
function abs(x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
上述abs()函数的定义如下：

function指出这是一个函数定义；
abs是函数的名称；
(x)括号内列出函数的参数，多个参数以,分隔；
{ ... }之间的代码是函数体，可以包含若干语句，甚至可以没有任何语句

abs()函数实际上是一个函数对象，而函数名abs可以视为指向该函数的变量

法2：
var abs = function (x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};
在这种方式下，function (x) { ... }是一个匿名函数，它没有函数名。但是，这个匿名函数赋值给了变量abs，所以，通过变量abs就可以调用该函数

调用函数时，按顺序传入参数即可：
abs(10); // 返回10
abs(-9); // 返回9

传入的参数比定义的少也没有问题：
abs(); // 返回NaN

由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，
abs(10, 'blablabla'); // 返回10
abs(-9, 'haha', 'hehe', null); // 返回9
```
7. `arguments关键字`
```
arguments，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。arguments类似Array但它不是一个Array：

function foo(x) {
    alert(x); // 10
    for (var i=0; i<arguments.length; i++) {
        alert(arguments[i]); // 10, 20, 30
    }
}
foo(10, 20, 30);
```
8. 由于JavaScript函数允许接收任意个参数，于是我们就不得不用`arguments来获取所有参数`

9. ES6标准引入`rest参数`：
```
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]

foo(1);
// 结果:
// a = 1
// b = undefined
// Array []
```
```
rest参数只能写在最后，前面用...标识，从运行结果可知，传入的参数先绑定a、b，多余的参数以数组形式交给变量rest，所以，不再需要arguments我们就获取了全部参数。

如果传入的参数连正常定义的参数都没填满，也不要紧，rest参数会接收一个空数组（注意不是undefined）。

因为rest参数是ES6新标准，所以你需要测试一下浏览器是否支持。
```
10. `return语句的注意事项` : JavaScript引擎有一个在`行末自动添加分号的机制`，这可能让你`栽到return语句的一个大坑`
```
###
function foo() {
    return { name: 'foo' };
}

foo(); // { name: 'foo' }
如果把return语句拆成两行：

###
function foo() {
    return
        { name: 'foo' };
}

foo(); // undefined
要小心了，由于JavaScript引擎在行末自动添加分号的机制，上面的代码实际上变成了：

function foo() {
    return; // 自动添加了分号，相当于return undefined;
        { name: 'foo' }; // 这行语句已经没法执行到了
}

###
所以正确的多行写法是：

function foo() {
    return { // 这里不会自动加分号，因为{表示语句尚未结束
        name: 'foo'
    };
}
```