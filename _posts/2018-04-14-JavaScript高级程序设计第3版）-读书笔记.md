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

除了对数值进行加法运算外，使用"+"为链接符还会将数据类型进行转换类似调用`toString()`

```javascript
var arr = [1,2,3];

console.info(arr); // [ 1, 2, 3 ]
console.info('arr=='+arr); // arr==1,2,3
console.info(arr.toString()); // 1,2,3
```

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
```

- `with` 

- `switch`

switch语句在比较值时使用的是全等操作符.

```
var num = 1;

switch(num) {
    case 1:
        console.info('number 1');
        break;
    case '1':
        console.info('string 1');
        break;
}

//参数可以是多种类型
var num = 25;

switch(true) {
    case num<0:
        console.info('less 0');
        break;
    case num>0 && num<20:
        console.info('than 0 less 20');
        break;
    default:
        console.info('than 20');
        break;
}
```

### 函数

JS的函数参数与大多数语言的函数参数不同；
JS的函数不关心传递进来多少个参数，也不在乎传进来的参数类型。

```javascript
function fun() {
    var argLen = arguments.length;

    if(argLen == 1) {
        console.info(arguments[0] + 10);
    }else if(argLen > 1) {
        var sum = 0;
        for(var i = 0; i < argLen; i++) {
            sum += arguments[i];
        }
        console.info(sum);
    }
}

fun(2); // 12
fun(1,2,3); // 6
```

arguments的值永远和参数值保持同步
arguments对象的长度是由传入的参数个数决定的，不是由定义函数时的形参个数决定的。

```javascript
function fun(par1, par2) {
    arguments[1] = 10;
    console.info(par1);
    console.info(par2);
    console.info('\n');
}

fun(); // undefined undefined
fun(1);// 1 undefined 
fun(1, 2); // 1 10
```

- 没有重载

重载(overload)是指一个函数编写两个定义，只要两个定义的签名（接受的参数类型和数量）不同即可。

重写(overriding)，才是方法的覆盖。

javascript没有重载，但是可以通过检查参数的类型和数量模拟重载。

```
function fun(num) {
    return num + 100;
}

function fun(num) {
    return num + 200;
}

console.info(fun(100)); // 300
```

## 变量、作用域和内存问题

基本类型值在内存中占据的空间大小是固定的，因而被放在栈内存中；

引用类型是对象，被放在堆内存中。

### 基本类型和引用类型的值

javascript不允许直接访问内存中的位置，即不能直接操作对象的内存空间。操作对象时，实际是操作的对象的引用。

基本类型没有方法和属性，引用类型可为其动态的添加方法和属性。

- 复制变量值

基本类型复制时，是新建一个内存空间进行保存。

引用类型复制时，复制的值和被复制的值实际是引用同一个对象，都指向同一个内存空间。

```
var o1 = new Object();
o1.name = 'o1';
var o2 = o1;
o2.name = 'other o';
console.info(o1.name); // other o
```

- 参数的传递

参数的传递和变量的复制一样。

```
function setName(obj) {
    obj.name = 'tom'; // obj 引用自p
    obj = new Object();// obj重写为新的对象 当前为局部对象和p已无关
    obj.name = 'jack';
}
var p = new Object();
setName(p);
console.info(p.name);
```

- instanceof

### 执行环境及作用域

- 作用域琏

- 没有块级作用域 有函数作用域（function scope）
### 垃圾收集

执行环境中自动管理，在一定间隔时间内找出不再使用的变量，释放其内存空间。
垃圾收集器会跟踪哪个变量没用了，并打上标记。

- 标记清除（mark-and-sweep）

标记变量“进入环境”，和“离开环境”

- 引用计数

跟踪每个值被引用的次数，当引用次数为0时表示可以被释放。
循环引用会导致内存泄露。

```
function problem(){
    var objectA = new Object();
    var objectB = new Object();

    objectA.someOtherObject = objectB;
    objectB.someOtherObject = objectA;
}
```

- 性能问题

IE7以后性能临界值动态变化，有效的解决了IE的由于垃圾收集导致的性能问题.

- 管理内存

由于系统给浏览器分配的内存往往小于其他桌面程序，因此js的内存应及时释放。

引用解除

```
function getObj(){
    var obj = new Object;    
    obj.name = 'tom';
    return obj;
}

var globalVar = getObj();
// do something
globalVar = null; // 全局变量在使用结束后，进行引用解除
```

## 引用类型

### Object类型

### Array类型

数组的length属性不是只读的，可以修改值，修改值后可以从数组的末尾移除或向数组添加新项。

```javascript
var arr = [1,2,3];
arr.length = 2; 
console.info(arr); // [1,2] 3被移除

arr[arr.length] = 'aa'; // 数组的最后一项索引始终是length-1，因此它的下一项新位置就为length
arr[arr.length] = 'bb'; // 类似于 arr.push('bb');
console.info(arr); // [1,2,aa,bb] aa,bb被动态加入数组

arr.length = 5;
console.info(arr[4]); // undefined 长度增长到5时，4的位置没有赋值则为undefined
```

- Function:Array.isArray()

```
var arr = [];
console.info(Array.isArray(arr)); // true

console.info(arr instanceof Array); // true

var tmpArr = ['aa', 'bb', 'cc'];
console.info(tmpArr); // ['aa', 'bb', 'cc']
alert(tmpArr); // 'aa','bb','cc' 由于alert()需要接受字符串参数，因此在调用alert前需要先调用toString()处理。
```
```

- Function: join

```
var arr = ['aa', 'bb', 'cc'];
console.info(arr.join()); // 'aa','bb','cc'
console.info(arr.join(',')); // 'aa','bb','cc'
console.info(arr.join('||')); // 'aa'||'bb'||'cc'
```

- Function: push

- Function: pop

- Function: shift

- Function: unshift

- Function: reverse

- Function: sort

比较的是字符串的值，默认以升序排列。
sort可以接受比较函数作为参数。

```
function compare(val1, val2) {
    if(val1 < val2) {
	// 第一个参数小于第二个参数，则返回负数
        return -1;
    }else if(val1 > val2){
	// 第一个参数大于第二个参数，则返回正数
        return 1;
    }else if(val1 == val2) {
	// 两者相等则返回0
        return 0;
    }
}

var arr = [1,5,3, 2, 15];

console.info(arr.sort());// [ 1, 15, 2, 3, 5 ]
console.info(arr.sort(compare));// [ 1, 2, 3, 5, 15 ]
```

**操作方法**

- Function: concat([str], [Array])
@return Array

```
var arr = [1,2];
var $arr = arr; // 传递的是引用
var barr = arr.concat(); // 复制一个副本
$arr.push(3);
barr.push(5);
console.info($arr);
console.info(barr);
console.info(arr);

```

`concat`可以连接字符串或数组

```
var arr = ['aa', 'bb'];
var newArr = [11,22];
var a = arr.concat('cc', newArr);
console.info(a);
```

- Function: slice(begin, [end])
@return Array

- Function: splice(begin, removeNum, [insertStr...])
@return removedArr

**位置方法**

- Function: indexOf(findStr, begin)
@return index || -1

- Function: lastIndexOf(findStr, begin)
@return index || -1

查找的字符要和原数组中的字符全等"==="才能被找到

```
var arr = [1,2,3];
console.info(arr.indexOf('2')); // -1
```

**迭代方法**

- Function: every(function(value, index, array), [scope])
- Function: some(function(value, index, array), [scope])
- Function: filter(function(value, index, array), [scope])
- Function: map(function(value, index, array), [scope])
- Function: forEach(function(value, index, array), [scope])

```javascript
var arr = [1,2,3,4,5,4,3,2];

// every
// 方法参数的所有返回值都返回true 最后结果才为true
// 参数value为数组值，index为数组索引值，array为数组本身
var everyRes = arr.every(function (value, index, array) {
        return value>2;
});
console.info(everyRes);// false

// some
// 方法参数的某一次值返回true，最后结果就返回true
var someRes = arr.some(function(value, index, array){
        return value>2;
});
console.info(someRes); // true

// filter
// 按方法参数给定条件进行筛选，返回true的项将保留到最后的新数组中
var filterRes = arr.filter(function (value, index, array) {
        return value>2;
});
console.info(filterRes); // [ 3, 4, 5, 4, 3 ]

// map
// 对数组的每个值运用方法参数进行计算，最后得出新的数组
var mapRes = arr.map(function(value, index, array){
        return value*2;
});
console.info(mapRes); // [ 2, 4, 6, 8, 10, 8, 6, 4 ]

// forEach
// 类似于for循环
arr.forEach(function(value, index, array){
        if(value === 3 && array.length > 5) {
            console.info('Good!');
        }
});
// Good!
// Good!
```

**缩小方法**

- Function: reduce(function(prev, cur, index, array){});
- Function: reduceRight(function(prev, cur, index, array){});

```
// 累加数组值
var arr = [1,2,3,4,5];
var sum = arr.reduce(function(prev, cur, index, array){
    return prev+cur; 
});

console.info(sum);
```

### Date类型

- Function: parse(timestr)
- Function: UTC(year, month, [day], [hour], [minutes], [seconds], [millisecond])
- Function: now()

```
// 实例化Date对象
// 获取当前格式化时间
var date = new Date();

// 获取当前时间时间戳
var timestamp2 = Date.now();
var timestamp1 = +new Date();

// 将时间戳转换为格式化时间
var date0 = new Date(timestamp1);

// 将格式化时间转换为时间戳
var timestamp3 = Date.parse('2018-05-30 16:39:00');
var timestamp4 = Date.UTC(2018, 4, 30, 16, 39);
```
