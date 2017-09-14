# 方法 （2017.09.13）
1. `在一个对象中绑定函数，称为这个对象的方法`
```
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};

xiaoming.age; // function xiaoming.age()
xiaoming.age(); // 今年调用是25,明年调用就变成26了

如果以对象的方法形式调用，比如xiaoming.age()，该函数的this指向被调用的对象，也就是xiaoming，这是符合我们预期的。

如果单独调用函数，比如getAge()，此时，该函数的this指向全局对象，也就是window。
```
2. `Nodejs的对外接口`
```
可以把模块中希望被外界访问的内容定义到exports对象中。例如：
var name='';
function setName(n){
    name=n;
} 
function printName(){
    console.log(name);
}
exports.setName=setName;
exports.printName=printName;

那么就可以这样使用：

var test=require('./test');
test.setName('Byron');
test.printName();
```

```
exports 对象

var Student=function(){
    var name='';

     this.setName=function(n){
        name=n;
    }; 

    this.printName=function(){
        console.log(name)    ;
    };
};

exports.Student=Student;

使用：

var Student=require('./test').Student;
var student=new Student();
student.setName('Byron');
student.printName();

同时，可以改成这个样子：

var Student=function(){
    var name='';

     this.setName=function(n){
        name=n;
    }; 

    this.printName=function(){
        console.log(name)    ;
    };
};

module.exports=Student;

使用的时候，就可以变成这个样子：

var Student=require('./test');
```
```
首先，每个模块都会自动创建 一个 module 对象，对象有一个 modules 属性，初始值是个空对象。module 的公开接口 module.exports 就是公开这个属性。

为了方便，模块中也会有一个 exports对象，它和 module.exports指向的是同一个变量，所以我们修改exports对象的时候也会自动修改module.exports对象。

所以当module.exports对象不为空的时候， exports对象就自动忽略了，因为最后是返回 module.exports对象，而 module.exports对象 和exports对象指向已经不一样了。所以对exports对象的改动会失效
```
3. `一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数`
```
function add(x, y, f) {
    return f(x) + f(y);
}
当我们调用add(-5, 6, Math.abs)时，参数x，y和f分别接收-5，6和函数Math.abs，根据函数定义，我们可以推导计算过程为：

x = -5;
y = 6;
f = Math.abs;
f(x) + f(y) ==> Math.abs(-5) + Math.abs(6) ==> 11;
return 11;
用代码验证一下：
add(-5, 6, Math.abs); // 11
```
4. `apply的使用` : `重新定义this的指向`
```
要指定函数的this指向哪个对象，可以用函数本身的apply方法，它接收两个参数，第一个参数就是需要绑定的this变量，第二个参数是Array，表示函数本身的参数。

用apply修复getAge()调用：

function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```
5. `call的使用` : `把参数按顺序传入`
```
另一个与apply()类似的方法是call()，唯一区别是：
apply()把参数打包成Array再传入；
call()把参数按顺序传入。

比如调用Math.max(3, 5, 4)，分别用apply()和call()实现如下：

Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
```
6. `map的使用`
```
function pow(x) {
    return x * x;
}
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]

把Array的所有数字转为字符串：
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.map(String); // ['1', '2', '3', '4', '5', '6', '7', '8', '9']
```
7. `reduce` : Array的reduce()把一个函数作用在这个Array的[x1, x2, x3...]上，这个函数必须接收两个参数，reduce()把结果继续和序列的下一个元素做累积计算，其效果就是：[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)
```
var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x + y;
}); // 25
```
```
要把[1, 3, 5, 7, 9]变换成整数13579，reduce()也能派上用场：

var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x * 10 + y;
}); // 13579
```
8. `sort()` : `默认把所有元素先转换为String再排序`
```
// 看上去正常的结果:
['Google', 'Apple', 'Microsoft'].sort(); // ['Apple', 'Google', 'Microsoft'];
// apple排在了最后:
['Google', 'apple', 'Microsoft'].sort(); // ['Google', 'Microsoft", 'apple']
// 无法理解的结果:
[10, 20, 1, 2].sort(); // [1, 10, 2, 20]
第二个排序把apple排在了最后，是因为字符串根据ASCII码进行排序，而小写字母a的ASCII码在大写字母之后。
第三个排序结果是什么鬼？简单的数字排序都能错？
这是因为Array的sort()方法默认把所有元素先转换为String再排序，结果'10'排在了'2'的前面，因为字符'1'比字符'2'的ASCII码
```
```
幸运的是，sort()方法也是一个高阶函数，它还可以接收一个比较函数来实现自定义的排序。
要按数字大小排序，我们可以这么写：

var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
}); // [1, 2, 10, 20]
如果要倒序排序，我们可以把大的数放前面：

var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return 1;
    }
    if (x > y) {
        return -1;
    }
    return 0;
}); // [20, 10, 2, 1]
```
9. `函数作为返回值` : `当我们调用lazy_sum()时，返回的并不是求和结果，而是求和函数`
```
function lazy_sum(arr) {
    var sum = function () {
        return arr.reduce(function (x, y) {
            return x + y;
        });
    }
    return sum;
}

调用函数f时，才真正计算求和的结果：
var f = lazy_sum([1, 2, 3, 4, 5]); // function sum()

在这个例子中，我们在函数lazy_sum中又定义了函数sum，并且，内部函数sum可以引用外部函数lazy_sum的参数和局部变量，当lazy_sum返回函数sum时，相关参数和变量都保存在返回的函数中，这种称为“闭包（Closure）”的程序结构拥有极大的威力。

请再注意一点，当我们调用lazy_sum()时，每次调用都会返回一个新的函数，即使传入相同的参数：
var f1 = lazy_sum([1, 2, 3, 4, 5]);
var f2 = lazy_sum([1, 2, 3, 4, 5]);
f1 === f2; // false
```
10. `闭包牢记`
```
返回闭包时牢记的一点就是：返回函数不要引用任何循环变量，或者后续会发生变化的变量
```
11. `generator` : `ES6标准引入的新的数据类型`
```
function* foo(x) {
    yield x + 1;
    yield x + 2;
    return x + 3;
}
generator和函数不同的是，generator由function*定义（注意多出的*号），并且，除了return语句，还可以用yield返回多次
```
12. `generator还有另一个巨大的好处，就是把异步回调代码变成“同步”代码`
13. `typeof操作符获取对象的类型`
```
typeof 123; // 'number'
typeof NaN; // 'number'
typeof 'str'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Math.abs; // 'function'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
```
14. `包装对象` : number、boolean和string都有包装对象。没错，在JavaScript中，字符串也区分string类型和它的包装类型。包装对象用new创建 所以`闲的蛋疼也不要使用包装对象`！`尤其`是针对`string类`
```
var n = new Number(123); // 123,生成了新的包装类型
var b = new Boolean(true); // true,生成了新的包装类型
var s = new String('str'); // 'str',生成了新的包装类型
虽然包装对象看上去和原来的值一模一样，显示出来也是一模一样，但他们的类型已经变为object了！所以，包装对象和原始值用===比较会返回false：

typeof new Number(123); // 'object'
new Number(123) === 123; // false

typeof new Boolean(true); // 'object'
new Boolean(true) === true; // false

typeof new String('str'); // 'object'
new String('str') === 'str'; // false
```
15. `toString`
```
123.toString(); // SyntaxError
遇到这种情况，要特殊处理一下：

123..toString(); // '123', 注意是两个点！
(123).toString(); // '123'
```
16. `module.exports VS exports`
```
给exports赋值是无效的，因为赋值后，module.exports仍然是空对象{}

如果要输出一个键值对象{}，可以利用exports这个已存在的空对象{}，并继续在上面添加新的键值；

如果要输出一个函数或数组，必须直接对module.exports对象赋值。

所以我们可以得出结论：直接对module.exports赋值，可以应对任何情况

强烈建议使用module.exports = xxx的方式来输出模块变量
```
17. `单元测试`
```
单元测试是用来对一个模块、一个函数或者一个类来进行正确性检验的测试工作
```
18. `devDependencies` : 开发的时候用
```
如果一个模块在运行的时候并不需要，仅仅在开发时才需要，就可以放到devDependencies中。这样，正式打包发布时，devDependencies的包不会被包含进来。
然后使用npm install安装。
```
12. `mocha` : `单元测试`
```
mocha默认会执行test目录下的所有测试，不要去改变默认目录
```
13. `npm test` : `执行命令`
```
我们在package.json中添加npm命令：

{
  ...

  "scripts": {
    "test": "mocha"
  },

  ...
}
然后在hello-test目录下执行命令：

C:\...\hello-test> npm test
可以得到和上面一样的输出。这种方式通过npm执行命令
```
