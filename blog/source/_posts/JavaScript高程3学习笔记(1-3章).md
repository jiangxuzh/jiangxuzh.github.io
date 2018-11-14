---
title: JavaScript高程3学习笔记(1-3章)
date: 2018-01-16 11:43:37
tags:
---

## [第二章]在HTML中使用JavaScript

#### defer与async
使用defer属性可以让脚本在文档完全呈现之后在执行。延迟脚本总是按照指定他们的顺序执行。
使用async属性可以表示当前脚本不必等待其他脚本，也不必阻塞文档呈现。不能保证异步脚本按照他们在页面中数显的顺序执行。
defer、async属性仅适用于外部脚本。
> - 如果 async="async"：脚本相对于页面的其余部分异步地执行（当页面继续进行解析时，脚本将被执行）。
> - 如果不使用 async 且 defer="defer"：脚本将在页面完成解析时执行。
> - 如果既不使用 async 也不使用 defer：在浏览器继续解析页面之前，立即读取并执行脚本。


## [第三章]基本概念

#### 数据类型
5种简单数据类型（基本数据类型）：Undefined,Null,Boolean,Number,String。
1种复杂数据类型：Object。

**Undefined类型：**
包含undefined值的变量与尚未定义的变量是不一样的。
```
var foo;
var bar = undefined;
// var baz;

console.log(foo); // "undefined"
console.log(bar); // "undefined"
console.log(baz); // 产生错误

console.log(typeof foo); // "undefined"
console.log(typeof baz); // "undefined"
```

**Null类型：**00
null值表示一个空对象指针，如果定义的变量是准备在将来用于保存对象，最好将变量初始化为null，这样直接检查null值就可以知道相应的变量是否已经保存了一个对象的引用。
```
if (car != null) {
    ... ...
}
```

位于null和undefined之间的相等操作符(==)总是时返回true
```
console.log(null == undefined); // true
```

**Boolean类型：**
各种数据类型转化为Boolean值所对应的转化规则：

| 数据类型  | 转化为true的值             | 转化为false的值 |
|:---------:|:--------------------------:|:---------------:|
| Boolean   | true                       | false           |
| String    | 任何非空字符串             | " "(空字符串)   |
| Number    | 任何非空字符串(包括无穷大) | 0和NaN          |
| Object    | 任何对象                   | null            |
| Undefined | are neat                   | undefined       |

**Number类型：**
isFinite()函数在参数位于最小与最大数值之间时会返回true，因此可以确定一个数值是不是有穷的。
```
var result = Number.MAX_VALUE + Number.MAX.VALUE;
console.log(result); // false
```
isNaN()函数可接受一个任何类型的参数，返回参数是否“不是数值”。
```
console.log(isNaN("10")); // false (可被转为数值)
console.log(isNaN("blue")); // true
console.log(isNaN(true)); // false (可被转为数值)
```

**String类型：**
字符字面量（转义符）：

| 字面量 | 含义      |
|:---- -:|:---------:|
| \n     | 换行      |
| \t     | 制表      |
| \b     | 退格      |
| \r     | 回车      |
| \f     | 进纸      |

toString()与toString()方法都可以把一个值转化为字符串，区别在于toString()无法转换null和undfined。

**Object类型：**
object的每个实例都具有下列属性和方法：
- constructor: 保存着用于创建当前对象的函数。
- hasOwnProperty(propertyName): 用于检查给定的属性在当前对象实例中是否存在，作为参数的属性名必须是字符串。
- isPrototypeOf(object): 用于检查传入的对象是否是当前对象的原型。
- propertyIsEnumerable(propertyName): 用于检查给定属性是否能使用for-in语句，作为参数的属性名必须是字符串。
- toLocaleString(): 返回对象的字符串表示，该字符串与执行环境的地区对应。
- toString(): 返回对象的字符串表示。(toLocaleString()与toString()在对数字和时间的转换上有所区别)。
- valueOf(): 返回对象的字符串、数值或布尔值表示。通常与toString()方法的返回值相同。

**typeof 操作符：**
对一个值使用typeof操作符可能返回下列某个字符串：
"undefined" *——如果这个值未定义；*
"boolean" *——如果这个值是布尔值；*
"string" *——如果这个值是字符串；*
"number" *——如果这个值是数值；*
"object" *——如果这个值是对象或者null；*
"function" *——如果这个值是函数；*
> Chrome7,Safari5对正则表达式应用typeof会返回"function"。其他则返回"object"

#### 操作符

**递增和递减操作符的前置与后置区别：**
```
var num1 = 2;
var num2 = 20;
var num3 = --num1 + num2;  // 等于21
var num4 = num1 + num2;  // 等于21

var num1 = 2;
var num2 = 20;
var num3 = num1-- + num2;  // 等于22
var num4 = num1 + num2;  // 等于21
```

**相等操作符：**

| 表达式            | 值        |
|:-----------------:|:---------:|
| null == undefined | true      |
| 'NaN' == NaN      | false     |
| 5 == NaN          | false     |
| NaN == NaN        | false     |
| NaN != NaN        | true      |
| false == 0        | true      |
| ture == 1         | true      |
| ture == 2         | false     |
| undefined == 0    | false     |
| null == 0         | false     |
| "5" == 5          | true      |

#### 语句

**for语句：**
由于ECMAScript中不存在块级作用域，因此循环内部定义的变量也可以在外部访问到：
```
var count = 10;
for (var i = 0; i < count; i++) {
    alert(i);
}
alert(i);  //10
```

for语句中的初始化表达式、控制表达式和循环后表达式都是可选的。将这三个表达式全部省略，会创建一个无限循环：
```
for (;;) {
    doSomething();
}
```
而只给出控制表达式相当于吧for循环转换成了while循环：
```
var count = 10;
var i = 0;
for(; i < count; ) {
    alert(i);
    i++;
}
```
> 使用while做不到的使用for同样做不到

**for-in语句：**
for-in语句是一种精准迭代语句，可用来枚举对象的属性：
```
for (var propName in window) {
    document.write(propName);
}
```

**label语句与break、continue语句：**
break与continue语句的区别在于，break语句会立刻退出循环强制继续执行循环后面的语句，continue语句虽然也是立即退出循环，但是退出循环后会从循环的顶部继续执行：
```
var num = 0;
for (var i = 1; i < 10; i++) {
    if (i % 5 == 0) {
        break;
    }
    num++;
}
alert(num);  //4

var num = 0;
for (car i = 1; i < 10; i++) {
    if (i % 5 == 0) {
        continue;
    }
    num++;
}
alert(num);  //8
```
使用label语句可以在代码中添加标签，可以在将来由break或continue语句引用：
```
// outermost标签表示外部for循环，添加在内部循环break后，使得break语句即退出内部循环也退出了外部 循环
var num = 0;
outermost:
for (var i = 0; i < 10; i++) {
    for (var j = 0; j < 10; j++) {
        if (i == 5 && j == 5) {
            break outermost;
        }
    }
}
alert(num);  //55

// continue退出当前循环强制继续执行outermost所表示的循环，因此内部循环就少执行了5次
var num = 0;
outermost:
for (var i = 0; i < 10; i++) {
    for (var j = 0; j < 10; j++) {
        if (i == 5 && j == 5) {
            continue outermost;
        }
    }
}
alert(num);  //95
```

#### 函数
**理解参数：**
ECMAScript中的参数在内部是用一个数组来表示的，也就是说，即便定义的函数只接收两个参数，在调用这个函数的时候也不一定要传两个参数，可以传一个、多个或者不传。在函数体内可以用arguments对象来访问这个参数数组从而获取传递给函数的每一个参数。
```
function sayHi(name, message) {
    alert("Hello " + name + "," + message);
}
// 可以写为
function sayHi() {
    alert("Hello " + arguments[0] + "," + arguments[1]);
}
```

**没有重载：**
ECMAScript函数不能像传统意义上那样实现重载，如果定义了两个名字相同的函数，则该名字只属于后定义的函数。
```
function addSomeNumber(num) {
    return num + 100;
}

function addSomeNumber(num) {
    return num + 200;
}

alert(addSomeNumber(100));  // 300
```
