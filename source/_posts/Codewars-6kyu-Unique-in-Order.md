<!--
 * @Author: kristennn 13949836783@163.com
 * @Date: 2022-07-16 17:36:47
 * @LastEditors: kristennn 13949836783@163.com
 * @LastEditTime: 2022-07-18 20:05:29
 * @FilePath: /blog/source/_posts/Codewars-6kyu-Unique-in-Order.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->

---

title: Unique in Order - 6kyu - Codewars
date: 2022-07-16 17:36:47
tags: []
categories: [Codewars, 6kyu]

---

# 一、题目

Implement the function unique_in_order which takes as argument a sequence and returns a list of items without any elements with the same value next to each other and preserving the original order of elements.
For example:

```
uniqueInOrder(‘AAAABBBCCDAABBB’) == [‘A’, ‘B’, ‘C’, ‘D’, ‘A’, ‘B’]
uniqueInOrder(‘ABBCcAD’)         == [‘A’, ‘B’, ‘C’, ‘c’, ‘A’, ‘D’]
uniqueInOrder([1,2,2,3,3])       == [1,2,3]
```

# 二、思路

1. 题目说函数入参是一个 sequence（有顺序的数据结构，string 和 array 都是），并没有说明是什么数据结构，所以第一步需要判断传入数据类型

```
iterable instanceof Array
typeof iterable === 'object'
```

2. 这道题并不是数组去重，而是去除相邻的重复元素，用遍历的方式，比较前一个元素和当前元素即可
3. 要求不能改变顺序

# 三、解法

1. 解法一
   由于 string 和 array 都具有 length 属性，且都可通过 for 循环遍历，所以未判断数据结构

```
var uniqueInOrder=function(iterable){
  let arr = []
  for (let i = 0; i < iterable.length; i++) {
    if (iterable[i] !== iterable[i - 1]) {
      arr.push(iterable[i])
    }
  }
  return arr
}
```

2. 解法二

```
var uniqueInOrder=function(iterable){
  const arr = iterable instanceof Array ? iterable : iterable.split('')
  return arr.filter((val, i) => val !== arr[i - 1])
}
```
