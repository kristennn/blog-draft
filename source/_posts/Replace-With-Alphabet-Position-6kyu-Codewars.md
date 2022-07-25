---
title: Replace With Alphabet Position-6kyu-Codewars
date: 2022-07-25 17:37:40
tags:
categories: [Codewars, 6kyu]
---

# 一、题目

Welcome.
In this kata you are required to, given a string, replace every letter with its position in the alphabet.
If anything in the text isn’t a letter, ignore it and don’t return it.
“a” = 1, “b” = 2, etc.

Example

```
alphabetPosition(“The sunset sets at twelve o’ clock.”)
Should return “20 8 5 19 21 14 19 5 20 19 5 20 19 1 20 20 23 5 12 22 5 15 3 12 15 3 11” ( as a string )
```

本题要求将给定字符串中的字母转换为 26 个字母表中的顺序数字，并返回数字组成的 string

# 二、思路

要解出本题，需要几个部分：

1. 给定字符串中含有空格和字符，且字母有大小写，需提前处理
   1. 使用正则匹配出所有的字母
   2. 不预先处理，在后续处理中判断（如 string.charCodeAt 的值是否属于字母的区间，若不是则为符号）
2. 如何获取到单个字母的字母表顺序
   1. 自己定义一个包含 26 个字母的 string，手动比对
   2. 使用 string.charCodeAt，获得字母对应的 UTF-16 编码单元，这个数字减去 96 刚好对应着 1-26 的字母顺序。（注意：大小写字母转换后的值不同，所以需要提前转换）

# 三、解法

1. 解法一

```
function alphabetPosition(text) {
  const str = 'abcdefghijklmnopqrstuvwxyz';
  const arr = text.toLowerCase().match(/[a-z]/g)
  if (!arr || !arr.length) {
    return ""
  }
  return arr.map(c => str.indexOf(c) + 1).join(' ')
}
```

2. 解法二

```
function alphabetPosition(text) {
  const arr = text.toLowerCase().match(/[a-z]/g)
  if (!arr || !arr.length) {
    return ""
  }
  return arr.map(c => c.charCodeAt(0) - 96).join(' ')
}
```
