---
layout: post
title: JavaScript高级程序设计第3版
subtitle: JavaScript高级程序-延伸思考
date: 2018-05-11
categories: 读书笔记
postPatterns: circuitBoard
tags: 
---

- 将十进制数转换为其他进制数

```
var num = 10;
var num2 = num.toString(2);
var num8 = num.toString(8);
var num16 = num.toString(16);
```

- 将其他进制数转换为十进制数

```
var num16to10 = parseInt('0xa', 16);
var num2to10 = parseInt('1010', 2);
var num8to10 = parseInt('010', 8);
```

- 构造函数和全局对象

1. 构造函数 `Date()`/`RegExp()`/`Object()`/`Array()`/`Function()`等

构造函数可以看做是一个类，在使用时需要将其初始化为对象(new)，才能调用其属性和方法。
如：
```
var d = new Date();
d.getFullYear();
```

2. 全局对象 `Math`/`JSON`

全局对象已经是对象，因此可以直接使用其属性和方法，如`Math.max()`
