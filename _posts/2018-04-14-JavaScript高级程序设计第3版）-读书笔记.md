---

layout: post
title: JavaScript高级程序设计第3版）-读书笔记
subtitle: JavaScript高级程序设计（第3版）
date: 2018-04-14
categories: notes
postPatterns: circuitBoard

## tags: JavaScript

# 第1章 JavaScript简介

# 第2章 在HTML中使用JavaScript

## 2.3 文档模式

1.通过文档类型切换(doctype)实现；
2.文档模式分为：混杂模式(quirks mode)、标准模式(standards mode)、准标准模式(almost stadards mode)；
3.主要影响CSS内容的呈现，在某些情况会影响javascript的解释执行；
4.没有文档类型声明，会默认开启混杂模式。

## 2.4 noscript元素

在以下情况会显示noscript元素内容
1.不支持js;
2.禁用了js。

# 第3章 基本概念

## 严格模式

1.在ES5引入严格模式(strict mode)概念；
2.处理ES3不确定的行为和不安全的操作；
3.使用严格模式：`"use strict";`

## 数据类型

Undefined/Null/Boolean/Number/String/Object

### typeof操作符

1.使用方式

```javascript
// typeof 是操作符，不是函数，因此后面的括号可以省略
typeof var
typeof(var)
```

### Undefined

声明变量但未初始化，其值为`undefined`

```
var msg;
console.info(msg == undefined); // 输出 undefined 注意：undefined是值不是字符串
```

### NULL

特殊值null是一个空的对象指针
一个即将保存对象的变量，最好初始化为null

```
typeof null; // 返回'object'
```


`undefined`是派生自null值，因此它们的相等性测试会返回`true`

```
// null和undefined值相等，但类型不相等
alert(null == undefined);  // true
alert(null === undefined);  // false
```

### Boolean

Boolean类型的字面值true和false是区分大小的，也就是说，True和False（以及其他混合大小写形式）都不是Boolean值。

```
var s = false;  // false作为Boolean类型可以直接使用
var n = False;  // 此方式会报错，False不是Boolean类型，只能字符串使用

// False没有实际意义，只是标示符，甚至可以当做变量使用
var False = true; 
console.info(False); // 返回 true
```

- Function: Boolean()

将一个值转换为Boolean类型

```
var msg = "Hello world!";
var msgAsBoolean = Boolean(msg);
console.info(msgAsBoolean); // 返回true
```

- Boolean类型的自动转换

数据类型    |    转换为true     |    转换为false
-|-|-
Boolean     |        true       |        false
String      |   任何非空字符串  |       ""
Number      |   非零数字值      |       0和NaN
Object      |   任何对象        |       null
Undefined   |   n/a             |       undefined

*注：n/a(或N/A)即not applicable 意思是“不适用”*

### Number

- 八进制字面量（如057）在严格模式下无效.

- 保存浮点数值所需空间是保存整数值的两倍.

以下情况js会将浮点数解析会整数：
```
var floatNum1 = 1.; // 小数点后没有数字——floatNum1解析为1
var floatNum2 = 10.0; // 本身为整数——floatNum2解析为10
```

#### 浮点数总结

- 可用'e'表示法

```
var f1 = 2e9; // f1==2*10^9
var f2 = 2e-9; // f2==2*10^-9
```

- 浮点数最高精度是17位小数

- 浮点数在进行算术运算时其精度不如整数

例如：0.1+0.2结果是0.30000000000000004;这是使用基于IEEE754数值的浮点计算的通病。

- 永远不要测试某个特定的浮点数值
