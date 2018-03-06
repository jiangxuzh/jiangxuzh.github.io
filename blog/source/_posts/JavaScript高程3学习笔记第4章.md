---
title: JavaScript高程3学习笔记第4章
date: 2018-02-12 15:49:42
tags:
---

变量、作用域和内存问题

## 基本类型和引用类型的值

#### 动态的属性
只能给引用类型值动态地添加属性，以便将来使用
```
var person = new Object();
person.name = "Nicholas";
alert(person.name); //"Nicholas" 

var name = "Nicholas";
name.age = 27;
alert(name.age); //undefined 
```

#### 复制变量值
如果从一个变量向另一个变量复制基本类型的值，会在变量对象上创建一个新值，然后把该值复制到为新变量分配的位置上，此后，**这两个变量可以参与任何操作而不会相互影响**。
```
var num1 = 5;
var num2 = num1; 
```

从一个变量向另一个变量复制引用类型的值时，同样也会将存储在变量对象中的值复制一份放到为新变量分配的空间中。不同的是，这个值的副本实际上是一个指针，而这个指针指向存储在堆中的一个对象。复制操作结束后，两个变量实际上将引用同一个对象。因此，**改变其中一个变量，就会影响另一个变量**。
```
var obj1 = new Object();
var obj2 = obj1;
obj1.name = "Nicholas";
alert(obj2.name); //"Nicholas" 
```

#### 传递参数
ECMAScript中所有函数的参数都是按值传递的。
```
function setName(obj) {
 obj.name = "Nicholas";
 obj = new Object();
 obj.name = "Greg";
}
var person = new Object();
setName(person);
alert(person.name); //"Nicholas" 
```
以上例子表明即使在函数内部修改了参数的值，但原始的引用仍然保持未变。实际上，当在函数内部重写 obj 时，这个变量引用的就是一个局部对象了。而这个局部对象会在函数执行完毕后立即被销毁。**可以把ECMAScript函数的参数想象成局部变量。**

#### 检测类型
可以使用typeof检测基本数据类型，但在使用typeof检测引用类型的值回出现一个问题，无论引用的是什么类型的对象，它都返回"object"。
为此ECMAScript提供了instanceof操作符。如果变量是给定引用类型的实例，那么instanceof操作符就会返回true。
```
var oStringObject = new String("hello world"); 
console.log(typeof oStringObject);   // "object"
console.log(oStringObject instanceof String);   // "true"
```
根据规定，所有引用类型的值都是Object的实例。因此，在检测一个引用类型值和Object构造函数时，instanceof操作符始终会返回true。当然，如果使用instanceof操作符检测基本类型的值，则该操作符始终会返回 false，因为基本类型不是对象。
```
console.log(Object instanceof Object);   // true 
console.log(Function instanceof Function);   // true 
console.log(Number instanceof Number);   // false 
console.log(String instanceof String);   // false 
console.log(Function instanceof Object);   // true 

function Foo(){}
var foo = new Foo();
console.log(foo instanceof Foo);   // true
console.log(Foo instanceof Function);   //true 
console.log(Foo instanceof Foo);   //false
```

#### 执行环境及作用域
> - 执行环境有全局执行环境（也称为全局环境）和函数执行环境之分，在Web浏览器中，全局执行环境被认为是window对象；
> - 每次进入一个新执行环境，都会创建一个用于搜索变量和函数的作用域链；
> - 函数的局部环境不仅有权访问函数作用域中的变量，而且有权访问其包含（父）环境，乃至全局环境；
> - 全局环境只能访问在全局环境中定义的变量和函数，而不能直接访问局部环境中的任何数据；
> - 变量的执行环境有助于确定应该何时释放内存。某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数也随之销毁。全局执行环境直到应用退出（例如关闭网页或浏览器）时才会被销毁。

**延长作用域链：**try-catch语句的catch块、with语句，这两个语句都会在作用域链的前端添加一个变量对象。
```
function buildUrl() {
 var qs = "?debug=true";
 with(location){
        var url = href + qs;
    }
    return url;
}
```

#### 垃圾回收
> - 离开作用域的值将被自动标记为可以回收，因此将在垃圾收集期间被删除。
> - “标记清除”是目前主流的垃圾收集算法，这种算法的思想是给当前不使用的值加上标记，然后再回收其内存。
> - 另一种垃圾收集算法是“引用计数”，这种算法的思想是跟踪记录所有值被引用的次数。JavaScript引擎目前都不再使用这种算法；但在 IE 中访问非原生 JavaScript 对象（如 DOM 元素）时，这种算法仍然可能会导致问题。
> - 当代码中存在循环引用现象时，“引用计数”算法就会导致问题。
> - 解除变量的引用不仅有助于消除循环引用现象，而且对垃圾收集也有好处。为了确保有效地回收内存，应该及时解除不再使用的全局对象、全局对象属性以及循环引用变量的引用。
