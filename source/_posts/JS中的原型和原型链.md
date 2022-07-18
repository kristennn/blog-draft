---
title: JS中的原型和原型链
date: 2022-07-18 20:04:59
tags:
categories: [前端, Javascript]
---

# 一、为什么设计原型？

1. 在 JS 中，所有的数据类型都被设计为对象（原始类型也都有着对应的包装对象）
2. JS 在设计之初只是为了让浏览器可以互动，是一门简单的脚本语言，引入面向对象范式中的“类”机制有些过于正式，增大学习难度
3. 但是，确实需要一种机制来将所有对象联系起来。否则所有对象的属性都是私有的，一方面写多个对象时，需要一个个手写；另一方面也会比较占用内存
4. 最终，选择通过“原型”来实现对象的继承机制

# 二、从构造函数说起

### 2.1 对象的创建 — 构造函数

一般创建对象有两种方式：

- 通过字面量创建

```
let obj = {}
let ary = []
let fun = function(){}
```

- 通过构造函数创建

```
let obj = new Object()
let ary = new Array()
let fun = new Function()
```

但事实上，字面量创建本身也是通过调用构造函数来实现的，它是构造函数创建的语法糖。

构造函数创建对象的步骤是这样的：

- 创建新对象
- 将 this 指向这个新对象
- 执行构造函数中的代码
- 返回 this

```
function Foo(name) {
	this.name = name
}
let f = new Foo('js')
// 调用函数时发生了这些事：
// 创建空对象 obj
// 将 this 指向 obj
// 赋值 即：obj.name = name
// 返回 this
```

### 2.2 构造函数的原型

1. 构造函数有一个属性 prototype，属性值为一个对象
2. 使用时，将实例化对象的私有属性写在构造函数里，共有属性写在构造函数的 prototype 属性对象里

```
function People(name) {
	this.name = name
}
People.prototype.nation = 'China'   // 设置一个共有属性
let a = new People('luck')
let b = new People('Ann')
a.name   // 'luck'
a.nation  // 'China'
b.name   // 'Ann'
b.nation  // 'China'

People.prototype.nation = 'Canada'    // 改变共有属性
a.nation   // 'Canada'   属性值更新
b.nation   // 'Canada'   属性值更新
```

# 三、了解原型链前必须了解的 6 大规则

声明：以下使用对象来代表 对象、数组、函数（即除 null 外的所有引用类型）

1. 每个对象都是由构造函数创建的。这一点在上面已经讲得很清楚，不再举例

2. 所有对象都可自由扩展属性。如：

```
let obj = {}, ary = [], fun = function(){}
obj.a = 1
obj.a   // 1
ary.a = 1
ary.a   // 1
fun.a = 1
fun.a   // 1
```

可以看出，并不是只有对象可以自由增加属性，数组和函数也可以，因为它们本质上就是对象

3. 所有对象都有一个 **proto** 属性，属性值是一个普通对象：

![](1.png)

4. 所有函数都有一个 prototype 属性，属性值是一个普通对象：

![](2.png)

5. 所有对象的 **proto** 属性值指向它构造函数的 prototype 属性值：

![](3.png)
注意：例子中用的是严格相等符，也就是说除非它们指向同一个对象才会返回 true

6. 当查找一个对象的属性时，如果对象本身没有这个属性，会去它的 _proto_ （即它构造函数的 prototype）中寻找。用上面的例子说明：

```
function People(name) {
	this.name = name
}
People.prototype.nation = 'China'
let a = new People('luck')

a.name   // 'luck'   这个很正常，就是 a 自己的属性
a.nation  // 'China'
// 在 a 中找不到叫做 nation 的属性，
// 于是找 a.__proto__，也就是 a 构造函数的原型： People.Prototype
// 在原型中找到了 nation 属性，返回这个属性的值
```

# 四、基于原型链的继承

### 4.1 原型链到底是什么？

简单从字面意思上理解：原型链就是由一个个原型组成的链条。如图：

![](4.png)

### 4.2 原型链如何实现继承？

- 当查找 foo 对象的属性时，先确定它本身有没有这个属性
- 若没有，则找它构造函数的原型，也就是 Foo.prototype 有没有这个属性
- 若没有，则找 Foo.prototype 的构造函数的原型，也就是 Object.prototype，确定是否有这个属性（注意 Foo.prototype 本身也是一个对象（参见上面第 4 条原则），所以它也有 **proto** 属性，及自己的构造对象，也就是 Object）
- 若还是没有，则返回 undefined
- 图中这条向上查找的链条，就是原型链

注意：为了避免死循环，原型链是有“尽头”的。如图所示，Object.prototype 的原型对象是 null，null 没有原型，并作为原型链的最后一个环节，即：
![](5.png)
