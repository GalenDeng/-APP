# 字符串知识 （2017.09.12）

1. `多行字符串`
```
由于多行字符串用\n写起来比较费事，所以最新的ES6标准新增了一种多行字符串的表示方法，用反引号 ` ... ` 表示

alert(`多行
字符串
测试`);
```
2. `多个字符串的连接方式`
```
## 用 + 号连接
var name = '小明';
var age = 20;
var message = '你好, ' + name + ', 你今年' + age + '岁了!';
alert(message);

## 模板字符串
var name = '小明';
var age = 20;
var message = `你好, ${name}, 你今年${age}岁了!`;
alert(message);
```
3. `字符串的注意事项`
```
字符串是不可变的，如果对字符串的某个索引赋值，不会有任何错误，但是，也没有任何效果：

var s = 'Test';
s[0] = 'X';
alert(s); // s仍然为'Test'
```
4. `toUpperCase` and `toLowerCase`
```
## toUpperCase()把一个字符串全部变为大写
var s = 'Hello';
s.toUpperCase();             // 返回'HELLO'
## toLowerCase()把一个字符串全部变为小写
var s = 'Hello';
var lower = s.toLowerCase(); // 返回'hello'并赋值给变量lower
lower;                       // 'hello'
```
5. `indexOf` and `substring`
```
##indexOf()会搜索指定字符串出现的位置：
var s = 'hello, world';
s.indexOf('world'); // 返回7
s.indexOf('World'); // 没有找到指定的子串，返回-1

##substring()返回指定索引区间的子串
var s = 'hello, world'
s.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello'
s.substring(7);    // 从索引7开始到结束，返回'world'
```