# JavaScript的array
1. `取得array的长度`: `length属性`
```
var arr = [1, 2, 3.14, 'Hello', null, true];
arr.length; // 6
```
2. `直接给Array的length赋一个新的值会导致Array大小的变化`
```
var arr = [1, 2, 3];
arr.length; // 3
arr.length = 6;
arr; // arr变为[1, 2, 3, undefined, undefined, undefined]
arr.length = 2;
arr; // arr变为[1, 2]
```
3. `索引赋值`
```
如果通过索引赋值时，索引超过了范围，同样会引起Array大小的变化：

var arr = [1, 2, 3];
arr[5] = 'x';
arr; // arr变为[1, 2, 3, undefined, undefined, 'x']

在编写代码时，不建议直接修改Array的大小，访问索引时要确保索引不会越界
```
4. `indexOf`
```
与String类似，Array也可以通过indexOf()来搜索一个指定的元素的位置：

var arr = [10, 20, '30', 'xyz'];
arr.indexOf(10); // 元素10的索引为0
arr.indexOf(30); // 元素30没有找到，返回-1
arr.indexOf('30'); // 元素'30'的索引为2
```
5. 数组的`slice` 其作用相对应于 `String的substring()版本` 用来截取Array的部分元素，返回一个新的Array
```
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
arr.slice(0, 3); // 从索引0开始，到索引3结束，但不包括索引3: ['A', 'B', 'C']
arr.slice(3);    // 从索引3开始到结束: ['D', 'E', 'F', 'G']
```
6. `如果不给slice()传递任何参数，它就会从头到尾截取所有元素。利用这一点，我们可以很容易地复制一个Array`
```
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
var aCopy = arr.slice();
aCopy;         // ['A', 'B', 'C', 'D', 'E', 'F', 'G']
aCopy === arr; // false
```
