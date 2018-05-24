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

## 3.1语法

#### 严格模式

1.在ES5引入严格模式(strict mode)概念；
2.处理ES3不确定的行为和不安全的操作；
3.使用严格模式：`"use strict";`

#### 数据类型

Undefined/Null/Boolean/Number/String/Object

#### typeof操作符

1.使用方式

```javascript
// typeof 是操作符，不是函数，因此后面的括号可以省略
typeof var
typeof(var)
```

#### Undefined

声明变量但未初始化，其值为`undefined`

```
var msg;
console.info(msg == undefined); // 输出 undefined 注意：undefined是值不是字符串
```

#### NULL

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

#### Boolean

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

#### Number

- 八进制字面量（如057）在严格模式下无效.

- 保存浮点数值所需空间是保存整数值的两倍.

以下情况js会将浮点数解析会整数：
```
var floatNum1 = 1.; // 小数点后没有数字——floatNum1解析为1
var floatNum2 = 10.0; // 本身为整数——floatNum2解析为10
```

##### 1.浮点数

- 可用'e'表示法

```
var f1 = 2e9; // f1==2*10^9
var f2 = 2e-9; // f2==2*10^-9
```

- 浮点数最高精度是17位小数

- 浮点数在进行算术运算时其精度不如整数

例如：0.1+0.2结果是0.30000000000000004;这是使用基于IEEE754数值的浮点计算的通病。

- 永远不要测试某个特定的浮点数值

##### 2.数值范围

- ECMAScript的最大、最小值分别保存在：

1. `Number.MAX_VALUE`
2. `Number.MIN_VALUE`

- 超出最小和最大值会被转换成`-Infinity`(负无穷)/`Infinity`(正无穷)

1. `Number.NEGATIVE_INFINITY`
2. `Number.POSITIVE_INFINITY`

- Function: isFinite()

用于判断数值是不是无穷（即是不是在`Number.MIN_VALUE`和最大值`Number.MAX_VALUE`之间）

##### 3.NaN

表示非数值(Not a Number)
> 这个数值用于表示一个本来要返回数值的操作数未返回数值的情况。在JS中，任何数除以0会返回NaN。

- NaN有两个特点

1. 任何设计NaN的操作都返回NaN
```
console.info(NaN/10); // ==>NaN
```

2.NaN与任何值都不相等，包括NaN本身
```
console.info(NaN == NaN); // ==>false
```

- Function: isNaN()

任何不能被转换为数值的值都会使isNaN()返回true

```
console.info(isNaN(10)); // ==> false
console.info(isNaN('10')); // ==> false
console.info(isNaN(true)); // ==> false
console.info(isNaN(NaN)); // ==> true
console.info(isNaN('hello word!')); // ==> true
```

##### 4.数值转换

- Function: Number()

`Number`被称为转型函数

```
Number(true);           // ==>1
Number(false);          // ==>0
Number(99);             // ==>99
Number(null);           // ==>0 **注意**
Number(undefined);      // ==>NaN

// start 参数为字符串的情况
Number('123');          // ==>123
Number('0123');         // ==>123
Number('1.1');          // ==>1.1
Number('01.1');         // ==>1.1
Number('0xef');         // ==>239 将十六进制数转换为十进制 **注意**
Number('');             // ==>0 **注意**
// end 其他情况字符串返回NaN
```

- Function: parseInt()

`parseInt`: 1.会忽略第一个字符前所有的空格;
            2.第一个字符如果不是数值或负号则直接返回`NaN`。
            3.可以转换八进制、十六进制为十进制
            4.第二个参数，可以指定转换为几进制

```
parseInt('   123');     // ==>123
parseInt('   s23');     // ==>NaN
parseInt(055);          // ==>45 将八进制转换为十进制
parseInt(0x55);         // ==>85 将十六进制转换为十进制
parseInt('123blue');    // ==>123
parseInt('-123');       // ==>-123
parseInt(12.3);         // ==>12
```

```
// 转换为指定进制数
parseInt('10', 2);        // ==>2 由二进制转换为十进制
parseInt('10', 8);        // ==>8 由八进制转换为十进制
parseInt('10', 10);       // ==>10 由十进制转换为十进制
parseInt('10', 16);       // ==>16 由十六进制转换为十进制

// 在没有指定进制数时，ES3和ES5对八进制的解析不一样
parseInt(010);      // ES3==>8
parseInt(010);      // ES5==>10

// 注意：当前chrome65测试显示，当转换的字符为数值时,在由八进制转换为十进制时，转换不受`parseInt`第二参数控制
parseInt(010);          // ==>8
parseInt(010, 10);      // ==>8
parseInt('010', 10);    // ==>10
```
- Functin: parseFloat()

`parseFloat`: 1.只能解析十进制数(十六进制数会被解析会0)
              2.只识别从左到右第一个小数点

```
parseFloat(0123);       // ==> 123
parseFloat('0x123');    // ==> 0
parseFloat('12.3.4');   // ==> 12.3
parseFloat('1.2e2');    // ==> 120
```

##### String

- 1.单双引号的使用对字符串解析没有影响，和PHP不一样；
- 2.类似`\u03a3`这样的转义序列只表示一个字符

```
var s = 'text\u03a3';
s.length;   // ==> 5
```

- Function: toString()

Number/Boolean/Object/String类型都有方法`toString`,null/undefined没有`toString`。

数值型转换时可以在toString中传入基数，获取相应的进制数

```
var num = 10;
num.toString(2);	// ==> "1010"
num.toSring(8);		// ==> "12"
num.toSring(16);	// ==> "a"
```

- Function: String()

```
String(10); 	// ==> '10'
String(true);	// ==> 'true'
String(null);	// ==> 'null'
String(undefined);	// ==> 'undefined'
```

##### Object

```
var o1 = new Object();
var o2 = new Object;	// 有效，不推荐
```

Object的每个实例都具有以下属性和方法：

1. `Constructor`

```
var t = new Array();
console.info(t.constructor==Array); // ==>true
var d = new Date();
console.info(d.constructor==Date); // ==>true
```

2.`hasOwnProperty()`

```
function fun(){
   this.name = 'tom';
}

var f = new fun();
console.info(f.hasOwnProperty('name'));	// ==>true

// hasOwnProperty检查对象实例中有无指定属性
// 不是检查实例原型中的属性
console.info(f.hasOwnProperty('constructor'));	// ==>false
```

3.`isPrototypeOf()`

```
function siteAdmin(nickName){
 this.nickName=nickName;
}

var matou=new siteAdmin("tom");
console.info(siteAdmin.prototype.isPrototypeOf(matou));	// ==>true
```

4.`propertyIsEnumerable()`

检查属性是否可以被枚举即`for...in`（所有自定义属性都可以被枚举）

```
function person() {
   this.name = 'zhangsan';
}

var p = new person();
console.info(p.propertyIsEnumerable('name')); // ==>true

for(k in p) {
console.info(k+','+p[k]); // ==>name,zhangsan
}
```

5.`toLocaleString()`

转换为本地格式字符串

```
var date = new Date();
date.toString();  // ==>"Fri May 11 2018 22:24:57 GMT+0800 (CST)"
date.toLocaleString();	// ==>"5/11/2018, 10:24:57 PM"
```

6.`toString()`

7.`valueOf()`

```
var array = new Array("a","b","c");
console.log(array.valueOf());	// Array(3) 返回数组本身
console.log(array.toString());	// 'a','b','c'
console.log(array.toLocaleString()); // 'a','b','c'
```

## 3.5操作符

### 一元操作符

要点：

1.对非数值前应用一元加和一元减操作符，该操作符会像Number()转型函数一样将值进行转换（一元减将值变为负数）。

```
var s1 = '0123';
var s2 = '1.2.3';
var s3 = null;
var s4 = undefined;
var s5 = false;
var s6 = '1.2';

+s1 // ==>123
-s1 // ==>-123
+s2 // ==>NaN
-s2 // ==>NaN
+s3 // ==>0
-s3 // ==>-0
+s4 // ==>0
-s4 // ==>-0
+s5 // ==>0
-s5 // ==>-0
+s6 // ==>1.2
-s6 // ==>-1.2
```

### 位操作符

### 布尔操作符

1. `!!`相当于Boolean()

```
!!'12' === Boolean('12'); // true
```

2. `&&`执行的是短路操作(一假则假)

```
123 && null; // null
null && 123; // null
true && 123; // 123
```

3. `||` 一真则真（只要有一个为真，则总是返回那个真值）

```
123 || null; // 123
null || 123; // 123
false || null; // null
```

### 乘性操作符

### 加性操作符

### 关系操作符

1. 字符串相比较时，是比较其第一个字符的字符编码大小

```
"Black" < "after"; // B的字符编码是66 a的字符编码是97 因此结果是true
"23" > "3"; // "2"的字符编码是50 "3"的字符编码是51 因此结果是false
```

2. 字符串和数字比较时，另个字符串会转换为数字来比较

```
"23" > 3; // true
```

3. NaN与任何值比较都返回false

```
NaN < 3; // false
NaN >= 3; // false
```
## 语句

- `for`

```
for(;;) {dosomething}; // 无限循环

var i = 0;
for(;i<10;) {
   i++;    
}
```

- `for-in`

`for (property in expression) statement`

expression是数组，则property为索引；
expression是对象，则property为属性名；

```
for(var propName in window) {
    document.write(propName);   
}

for(var i in ['a','b','c']) {
    console.info(i);
}// 0 1 2 i是key

for(var i in {"a": 1, "b": 2}) {
        console.info(i);
}// a b
```

- `label`

`label: statement`

```
//exam.
// start为标签(label),可用于break/continue引用
start: for(var i=0; i<10; i++) {
    if(i == 2) {
        break start;     
    }
}
console.info(i);// 2
```

- `break`/`continue`

break立即退出整个循环；
continue立即退出当前循环，继续从循环顶部继续执行。

```
var num1 = 0;
var num2 = 0;
// break
for(var i=1; i<10; i++) {
    if(i % 5 == 0){
        break;     
    }   
    num1++;
}
console.info(num1); // 4

// continue
for(var j=1; j<10; j++) {
    if(j % 5 == 0){
       continue;
    }   
    num2++;
}
console.info(num2); // 8
