---
title: Array.diff-6kyu-Codewars
date: 2022-07-25 17:44:09
tags:
categories: [Codewars, 6kyu]
---

# 一、题目

Your goal in this kata is to implement a difference function, which subtracts one list from another and returns the result.
It should remove all values from list a, which are present in list b keeping their order.
arrayDiff([1,2],[1]) == [2]
If a value is present in b, all of its occurrences must be removed from the other:
arrayDiff([1,2,2,2,3],[2]) == [1,3]

这道题要求从数组 A 中去掉数组 B 中包含的所有值，返回结果数组
题目有几点需要注意：

1. 若数组 B 中的一个值在 A 中出现多次，则应都去除
2. 不改变数组 A 的原始顺序

# 二、思路

1. 借用一个新的数组变量 res，将符合要求的值存入其中并返回
2. 使用 arr.filter 直接返回过滤后的数组

这两种方法都需要进行一个相同的处理：判断一个值是否出现在数组 B 中，完成这一点有几种方式： 1. 使用 arr.includes 直接判断。代码简洁，但这个 api 的实现是遍历一遍数组，速度较慢。复杂度 O(n²) 2. 使用 arr.indexOf 判断。和第一种方式类似。 3. 使用 set 的数据结构，将数组 B 转换为 set，这样仅需遍历一次。复杂度 O(n)

# 三、解法

1. 解法一

```
function arrayDiff(a, b) {
  let res = []
  a.forEach(item => {
    if (!b.includes(item)) {
      res.push(item)
    }
  })
  return res
}
```

2. 解法二

```
function arrayDiff(a, b) {
  return a.filter(item => !b.includes(item))
}
```

3. 解法三

```
function arrayDiff(a, b) {
  const setB = new Set(b)
  return a.filter(item => !setB.has(item))
}
```
