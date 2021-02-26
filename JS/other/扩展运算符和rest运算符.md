# 扩展运算符和rest运算符
## 1、扩展运算符简介
扩展运算符（ spread ）是三个点（...），可以将一个数组转为用逗号分隔的参数序列。

说的通俗易懂点，有点像化骨绵掌，把一个大元素给打散成一个个单独的小元素。




基本用法：拆解字符串与数组
```js
var array = [1,2,3,4];
console.log(...array);//1 2 3 4 
var str = "String";
console.log(...str);//S t r i n g
```

## 2、扩展运算符应用
### 2.1 某些场景可以替代apply

在使用Math.max()求数组的最大值时，ES5可以通过 apply 做到（用一种不友好且繁琐的方式）
```js
// ES5 apply 写法
var array = [1,2,3,4,3];
var max1 = Math.max.apply(null,array);
console.log(max1);//4
幸运的是JavaScript的世界在不断改变，扩展运算符可用于数组的析构，优雅的解决了这个问题。

// ES6 扩展运算符 写法
var array = [1,2,3,4,3];
var max2 = Math.max(...array);  
console.log(max2);//4
```
先把 array 打散成 1 2 3 4 3，再在里面找最大的那一个，就显而易见了。

### 2.2 代替数组的push、concat 等方法

实现把 arr2 塞到 arr1 中
```js
// ES5 apply 写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
Array.prototype.push.apply(arr1, arr2);
//arr1   [0, 1, 2, 3, 4, 5]
```
扩展运算符又要施展化骨大法了
```js
// ES6 扩展运算符 写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1.push(...arr2);
//arr1   [0, 1, 2, 3, 4, 5]
```
通俗的解释下， 扩展运算符先把 arr2 打散成 3 4 5 ， 之后再往arr1里push，就轻松多了。

同理可推，concat 合并数组的时候：
```js
var arr1 = ['a', 'b'];
var arr2 = ['c'];
var arr3 = ['d', 'e'];
​
// ES5的合并数组
arr1.concat(arr2, arr3)  // [ 'a', 'b', 'c', 'd', 'e' ]
​
// ES6的合并数组
[...arr1, ...arr2, ...arr3]  // [ 'a', 'b', 'c', 'd', 'e' ]
​
```
ES5的合并数组写法，像是 arr1 把 arr2，arr3 给吸收了。

而ES6的合并数组写法，则是先把 arr1，arr2， arr3 拆解，之后塞到新的数组中。
### 2.3 拷贝数组或对象
```js
//拷贝数组
var array0 = [1,2,3];
var array1 = [...array0];
console.log(array1);//[1, 2, 3]
​
//拷贝数组
var obj = {
    age:1,
    name:"lis",
    arr:{
        a1:[1,2]
    }
}
var obj2  = {...obj};
console.log(obj2);//{age: 1, name: "lis", arr: {…}}
```
无论是像克隆数组还是对象，先用化骨绵掌之扩展运算符，将其打散，之后再拼装的到一起就可以了，多么简单易用。






### 2.4 将伪数组转化为数组
```js
//伪数组转换为数组
var nodeList = document.querySelectorAll('div');
console.log([...nodeList]);  // [div, div, div ... ]
```
上面代码中，querySelectorAll 方法返回的是一个 nodeList 对象。它不是数组，而是一个类似数组的对象。

这时，扩展运算符可以将其转为真正的数组，原因就在于 NodeList 对象实现了 Iterator。

注意：使用扩展运算符将伪数组转换为数组有局限性，这个类数组必须得有默认的迭代器且伪可遍历的。

## 3、rest 运算符简介
剩余运算符（the rest operator），它的样子看起来和展开操作符一样，但是它是用于解构数组和对象。在某种程

度上，剩余元素和展开元素相反，展开元素会“展开”数组变成多个元素，剩余元素会收集多个元素和“压缩”成一个

单一的元素。

说的通俗点，有点像吸星大法，收集多个元素，压缩成单一的元素 。


rest参数用于获取函数的多余参数，这样就不需要使用arguments对象了。rest参数搭配的变量是一个数组，该变

量将多余的参数放入数组中。

例如实现计算传入所有参数的和

使用arguments参数：
```js
function sumArgu () {
     var result = 0;
     for (var i = 0; i < arguments.length; i++) {
        result += arguments[i];
    }
    return result
}
console.log(sumArgu(1,2,3));//6
使用rest参数：

function sumRest (...m) {
    var total = 0; 
    for(var i of m){
        total += i;
    }
    return total;
}
console.log(sumRest(1,2,3));//6
```
上面代码利用 rest 参数，可以向该函数传入任意数目的参数。传递给 sumRest 函数的一组参数值，被整合成了数组 m。

就像是吸星大法，把分散的元素收集到一起。

所以在某些场景中，无需将arguments转为真正的数组，可以直接使用rest参数代替。

### 4、rest 运算符应用
## 4.1 rest 参数代替arguments变量
```js
// arguments变量的写法
function sortNumbers() {
  return Array.prototype.slice.call(arguments).sort();
}
​
// rest参数的写法
const sortNumbers = (...numbers) => numbers.sort();
```
上面的两种写法，比较后可以发现，rest 参数的写法更自然也更简洁。

不过，rest参数和arguments对象有一定的区别：


### 4.2 与解构赋值组合使用
```js
var array = [1,2,3,4,5,6];
var [a,b,...c] = array;
console.log(a);//1
console.log(b);//2
console.log(c);//[3, 4, 5, 6]
```
备注：rest参数可理解为剩余的参数，所以必须在最后一位定义，如果定义在中间会报错。
```js
var array = [1,2,3,4,5,6];
var [a,b,...c,d,e] = array;
//  Uncaught SyntaxError: Rest element must be last element
```

## 5、总结
### 5.1 扩展运算符和rest运算符是逆运算

扩展运算符：数组=>分割序列

rest运算符：分割序列=>数组


### 5.2 扩展运算符应用场景

由于其繁琐的语法，apply 方法使用起来并不是很方便。当需要拿一个数组的元素作为函数调用的参数时，

扩展运算符是一个不错的选择。

扩展运算符还改善了数组字面量的操作，你可以更方便的初始化、连接、复制数组了。

使用析构赋值你可以提取数组的一部分。通过与迭代器协议的组合，你可以以一种更灵活的方式使用该表达式。

### 5.3 rest运算符应用场景

rest运算符主要是处理不定数量参数，rest参数使得收集参数变得非常简单。它是类数组对象arguments一个合理的替代品。

rest参数还可以与解构赋值组合使用。

在实际项目中灵活应用扩展运算符、rest运算符，能写出更精简、易读性高的代码。