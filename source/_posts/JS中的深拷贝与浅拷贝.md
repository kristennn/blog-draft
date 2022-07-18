---
title: JS中的深拷贝与浅拷贝
date: 2022-07-18 20:10:49
tags:
categories: [前端, Javascript]
---

在 js 中，一般数组和对象会涉及到深拷贝与浅拷贝，其他基本数据类型不涉及。

# 一、原始类型与引用类型

- 原始类型直接存储在栈（stack）里
- 引用类型的数据本身存储在堆（heap）里，栈里存储了指向该数据的地址/引用

# 二、赋值、浅拷贝、深拷贝的区别

### 2.1 赋值：

在对引用类型数据赋值时，其实赋的是该数据存储在栈里的 引用 ，而不是在堆中存储的数据本身。
也就是说，赋值后，两个变量的引用都指向了一个存储空间。
所以这两个变量的值是联动的，无论谁发生变化，变动的是同一个存储空间的数据。

举个栗子：

```
let ary1 = [1, 2, {a: 1}]
let ary2 = ary1
ary2[0] = 3
ary2[2].a = 2
console.log(ary1, ary2)
```

![](1.png)
可以看到，不管改变 ary2 的原始类型元素还是引用类型元素，ary1 都发生了变化

### 2.2 浅拷贝：

浅拷贝是按位拷贝对象，会创建一个新对象，并将原有对象的每个属性值都拷贝一份。
若属性值为原始类型，则拷贝的是它的值
若属性为引用类型，则拷贝的是它存储在栈中的地址，而不是存储在堆中的真实数据
因此若改变新对象的原始类型值，原对象不会受到影响；若改变引用类型值，则原对象会跟着变化，它们是联动的

举个栗子：

```
let obj1 = {a: 1, b: 2, c: {d: 3}}
let key, obj2 = {}
for (key in obj1) {
	obj2[key] = obj1[key]
}
obj2.a = 2
obj2.c.d = 4
console.log(obj1, obj2)
```

![](2.png)
可以看到，原对象属性 a 的值并未改变，而属性 c 的值发生了变化

### 2.3 深拷贝：

深拷贝会创建一个新对象，遍历原对象的每个属性。
若属性值为原始类型，则直接赋值属性值。
若属性值为引用类型，则创建一个新对象作为属性值，并复制，依次类推，直到对象的属性值都为原始类型。
深拷贝后，对象的引用类型属性均为新创建的对象，和原对象不共享内存。
所以修改新对象不会影响原对象。

举个栗子:

```
let obj1 = {a: 1, b: 2, c: {d: 3}}
let obj2 = JSON.parse(JSON.stringify(obj1))
obj2.a = 2
obj2.c.d = 4
console.log(obj1, obj2)
```

![](3.png)
可以看到 a 属性的值和 c 属性的值都没有变化

# 三、如何实现浅拷贝

### 3.1 Object.assign()

- 作用是将一个或多个源对象的可枚举属性复制到目标对象
- 接收两个参数：target（目标对象） 、source（源对象）

举个栗子：

```
let obj1 = {a: 1, b: 2, c: {d: 3}}
let obj2 = Object.assign({}, obj1)
console.log(obj1, obj2)
```

![](4.png)

### 3.2 Array.prototype.concat()

- 作用是合并两个或多个数组，不改变原有数组，返回新数组

举个栗子：

```
let ary1 = [1, 2, {a: 1}]
let ary2 = ary1.concat()
console.log(ary1, ary2)
```

![](5.png)

### 3.3 Array.prototype.slice()

- 作用是取某两个索引之间的元素，组成新数组返回

举个栗子：

```
let ary1 = [1, 2, {a: 1}]
let ary2 = ary1.slice()
console.log(ary1, ary2)
```

![](6.png)

# 四、如何实现深拷贝

### 4.1 JSON.parse(JSON. stringify())

将对象或数组转成 JSON 再解析成新的对象或数组

### 4.2 递归实现

```
static copyDeep = function(msg){
      let arr = [], obj = {},i,key;
      if(msg instanceof Array){
          if(msg.length){
              for(i=0;i<msg.length;i++){
                  arr[i] = this.copyDeep(msg[i]);
              }
          }
          return arr;
      } else if(msg instanceof Object){
          for(key in msg){
              obj[key] = this.copyDeep(msg[key]);
          }
          return obj;
      } else if(typeof msg ==='string'){
          return msg;
      } else {
          return msg;
      }
  }
```

### 4.3 JS 函数库 lodash

函数库提供 cloneDeep 来做深拷贝
