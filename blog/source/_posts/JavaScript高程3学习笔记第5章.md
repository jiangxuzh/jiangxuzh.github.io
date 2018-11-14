---
title: JavaScript高程3学习笔记第5章
date: 2018-03-06 17:48:19
tags:
---

## [第五章]引用类型

#### 1.Object类型：
访问对象属性时可用点表示法，也可使用方括号语法，以字符串的形式放在方括号中
```
alert(person["name"]);  // "Nicholas" 
alert(person.name);  // "Nicholas" 
```
方括号语法的主要优点是可以通过变量来访问属性
```
var propertyName = "name";
alert(person[propertyName]);  // "Nicholas" 
```
还可用于属性名中包含会导致语法错误的字符，或属性名使用的是关键字或保留字的时候
```
person["first name"] = "Nicholas";
```

#### 2.Array类型：
**栈方法：**
push()方法可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度。pop()方法则从数组末尾移除最后一项，减少数组的length值，然后返回移除的项。
```
var colors = ["red", "blue"];
var count = colors.push("black");
console.log(count);  // 3
var item = colors.pop();
console.log(item);  // "black"
console.log(colors);  // ["red", "blue"]
```

**队列方法：**
shift()能够移除数组中的第一项并返回该项，同时数组长度减1.unshift()与shift()的用途相反，能在数组前端添加任意个项并返回数组的新长度.

**重新排序方法：**
reverse()方法会反转数组项的顺序
```
var values = [1, 2, 3, 4, 5];
values.reverse();
alert(values);  // 5,4,3,2,1 
```

sort()方法会按升序排列数组项
在默认情况下会先调用每个数组项的toString()，然后比较得到的字符串以确定如何排序
```
var values = [0, 1, 5, 10, 15];
values.sort();
alert(values);  // 0,1,10,15,5
```
sort()方法可以接收一个比较函数作为参数，以便我们指定哪个值位于哪个值前面。
```
function compare(value1, value2) {
    if (value1 < value2) {
        return -1;
    } else if (value1 > value2) {
        return 1;
    } else {
        return 0;
    }
}
var values = [0, 1, 5, 10, 15];
values.sort(compare);
alert(values);  // 0,1,5,10,15
```

**操作方法：**
concat()方法可以基于当前数组中的所有项创建一个**新数组**，在没有参数的情况下只是复制当前数组并返回副本；如果参数是一个或多个数组，则会将这些数组中的每一项都添加到结果数组中；如果传递的值不是数组，这些值会被添加到结果数组的后面
```
var colors = ["red", "green", "blue"];
var colors2 = colors.concat("yellow", ["black", "brown"]);
alert(colors);  // red,green,blue
alert(colors2);  // red,green,blue,yellow,black,brown
```

slice()方法能够基于当前数组的一个或多各项创建一个**新数组**，可接收一个或两个参数，即要返回项的起始和结束位置；只有一个参数表示返回指定位置到数组末尾的所有项
```
var colors = ["red", "green", "blue", "black", "brown"];
var colors2 = colors.slice(1,4);
alert(colors2);  // green,blue,yellow

```

splice()方法可实现数组的删除，插入，替换操作并始终返回一个数组，该数组中包含从原始数组中删除的项(如果没有删除项则返回一个空数组)
```
var colors = ["red", "green", "blue"];
var removed = colors.splice(0, 1);
alert(colors);  // green,blue
alert(removed);  // red

removed = colors.splice(1, 0, "yellow", "orange");
alert(colors);  // green,yellow,orange,blue
alert(removed);  // 

removed = colors.splice(1, 1, "red", "purple");
alert(colors);  // green,red,purple,     orange,blue
alert(removed);  // yellow
```

**位置方法：**
indexOf()和lastIndexOf()方法都接收两个参数：要查找的项和表示查找起点位置的索引(可选)。
```
var numbers = [1,2,3,4,5,4,3,2,1];
alert(numbers.indexOf(4));  // 3
alert(numbers.lastIndexOf(4));  // 5

alert(numbers.indexOf(4, 4));  // 5
alert(numbers.lastIndexOf(4, 4));  // 3
```

**迭代方法：**
every()：对数组中的每一项运行给定函数，如果该函数对每一项都返回 true，则返回 true。
some()：对数组中的每一项运行给定函数，如果该函数对任一项返回 true，则返回 true。
```
var numbers = [1,2,3,4,5,4,3,2,1];
var everyResult = numbers.every(function(item, index, array){
 return (item > 2);
});
alert(everyResult); //false
var someResult = numbers.some(function(item, index, array){
 return (item > 2);
});
alert(someResult); //true 
```

filter()：对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。
map()：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
```
var numbers = [1,2,3,4,5,4,3,2,1];
var filterResult = numbers.filter(function(item, index, array){
    return (item > 2);
});
alert(filterResult); //[3,4,5,4,3] 
var mapResult = numbers.map(function(item, index, array){
    return item * 2;
});
alert(mapResult); //[2,4,6,8,10,8,6,4,2] 
```

forEach()：只是对数组中的每一项运行传入的函数，这个方法没有返回值
```
var numbers = [1,2,3,4,5,4,3,2,1];
numbers.forEach(function(item, index, array){
    // 执行某些操作
});
```

**归并方法：**
reduce()和reduceRight()都会迭代数组的所有项，然后构建一个最终返回的值。
函数接收4个参数:前一个值、当前值、项的索引和数组对象。
```
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
    // 第一次执行回调函数，prev是1，cur是2
    return prev + cur;
});
alert(sum); //15 

var sum2 = values.reduceRight(function(prev, cur, index, array){
    // 第一次执行回调函数，prev是5，cur是4
    return prev + cur;
});
alert(sum2); //15 
```

#### 3.Date类型：
ECMAScript5添加了Data.now()方法，返回表示调用这个方法时的日期和时间的毫秒数。
```
// 取得开始时间
var start = Date.now();
// 调用函数
doSomething();
// 取得停止时间
var stop = Date.now();
var result = stop – start;
```

#### 4.RegExp类型：
ECMAScript通过RegExp类型来支持正则表达式。使用下面类似Perl的语法，就可以创建一个正则表达式。
```
var expression = /pattern/flags;
```
> 正则部分后续详细记录

#### 5.Function类型：
**函数声明与函数表达式：**
解析器会率先读取函数声明，并使其在执行任何代码之前可用。函数表达式则必须等到解析器执行到它所在的代码和才会真正被解析执行。
