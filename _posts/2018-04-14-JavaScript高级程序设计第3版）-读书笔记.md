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

| 数据类型      | 转换为true | 转换为false  |
| --------- | ------- | --------- |
| Boolean   | true    | false     |
| String    | 任何非空字符串 | ""        |
| Number    | 非零数字值   | 0和NaN     |
| Object    | 任何对象    | null      |
| Undefined | n/a     | undefined |

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
num.toString(2);    // ==> "1010"
num.toSring(8);        // ==> "12"
num.toSring(16);    // ==> "a"
```

- Function: String()

```
String(10);     // ==> '10'
String(true);    // ==> 'true'
String(null);    // ==> 'null'
String(undefined);    // ==> 'undefined'
```

##### Object

```
var o1 = new Object();
var o2 = new Object;    // 有效，不推荐
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
console.info(f.hasOwnProperty('name'));    // ==>true

// hasOwnProperty检查对象实例中有无指定属性
// 不是检查实例原型中的属性
console.info(f.hasOwnProperty('constructor'));    // ==>false
```

3.`isPrototypeOf()`

```
function siteAdmin(nickName){
 this.nickName=nickName;
}

var matou=new siteAdmin("tom");
console.info(siteAdmin.prototype.isPrototypeOf(matou));    // ==>true
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
date.toLocaleString();    // ==>"5/11/2018, 10:24:57 PM"
```

6.`toString()`

7.`valueOf()`

```
var array = new Array("a","b","c");
console.log(array.valueOf());    // Array(3) 返回数组本身
console.log(array.toString());    // 'a','b','c'
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

// 实例化后的日期对象和Date对象含有的属性方法不一样
// 例如日期对象date没有now()方法，而Date对象有
var timestamp5 = date.valueOf();
var timestamp6 = Date.now();
```

**日期格式化**

- Function: toString();
- Function: toLocaleString();
- Function: toDateString();
- Function: toLocaleDateString();
...

以上方法为日期实例化对象的方法，在每个浏览器的表现都不一样，因此不能用于表现同一格式的时间。

### RegExp类型

experssion = /pattern/flags

flags有：i g m

- 正则表达式创建方式

```
// 以下两者完全等价
var reg1 = /abc/ig;
var reg2 = new RegExp('abc', 'ig');
```

- 需要转义的元字符

`([{}])^$?.+|*\`

- 双重转义

在使用RegExp构造正则表达式时，要匹配元字符对应的字符串需要双重转义

```
// 字面量模式/\[c:\]/
new RegExp('\\[c:\\]');

// 字面量模式/\w\\hello\\123/
new RegExp("\\w\\\\hello\\\\123")
```

- 正则实例的属性和方法

属性
global
ignoreCase
multiline
source
lastIndex

- Function: exec()

- Function: test()

```
var str = 'this is a cat';
var reg = /(\w+)\scat/ig;

// c表示console.info
// 实例属性
// global
c(reg.global);
// ignoreCase
c(reg.ignoreCase);
// multiline
c(reg.multiline);
// source
c(reg.source);
// lastIndex 开始检索的起始位置
c(reg.lastIndex);

// 实例方法
// exec
var matches = reg.exec(str);
c(matches.input); // 需要检索的字符串 此处是 this is a cat
c(matches.index); // 表示匹配项在字符串的位置
c(matches[0]); // 匹配上的字符串 此处是 a cat
c(matches[1]);
// test
c(reg.test(str));
```

正则示带`g`的在调用exec时，每调用一次就会向后匹配一次

```
tr = 'bat cat dat mat';
var pattern1 = /.(at)/g;

var m1 = pattern1.exec(str);
c(pattern1.lastIndex);
c(m1.index);
c(m1[0]); // bat
c(m1[1]);

var m2 = pattern1.exec(str);
c(pattern1.lastIndex);
c(m2.index);
c(m2[0]); // cat
c(m2[1]);
```

- 构造函数RegExp属性

```
var str = 'this is a hotline.';
var reg = /(.)li(..)/i;

if(reg.test(str)){
    // c(RegExp.input);
    c(RegExp['$_']);
    // c(RegExp.lastMatch);
    c(RegExp['$&']);
    // c(RegExp.lastParen);
    c(RegExp['$+']);
    // c(RegExp.leftContext);
    c(RegExp['$`']);
    // c(RegExp.rightContext);
    c(RegExp["$'"]);
    c(RegExp.$1);
    c(RegExp.$2);
}
```

### Function类型

1.声明函数的三种方法

```
// 函数式声明
function funName(){} // 声明提前

// 表达式声明
var funName = function(){}

// 构造函数
var funName = new Function(); // 要解析两次，消耗性能，不推荐
```

2.函数是对象，函数名是指针

3.使用不带圆括号的函数名是访问函数指针，而非调用函数

4.函数声明和函数表达式

```
// 由于函数声明会提前，因此能得到正确的值
alert(sum(1,2)); // 3
function sum(v1, v2) {
    return v1+v2;    
}

// 函数表达式
// 此处仅var sum1;被提前
alert(sum1(1,2)); // 报错TypeError: sum1 is not a function
var sum1 = function(v1, v2){
    return v1+v2;    
}
```

- Function arguments.callee
callee是一个指针，指向拥有这个arguments对象的函数

```
// arguments.callee()
// 阶乘
function f(num) {
    if(num==1) {
        return 1;
    }else {
        return num * arguments.callee(num-1)
    }
}


var tmpf = f;

// f被重写
f = function() {
    return 0;
};

c(tmpf(5)); // 120
c(f(5)); // 0
```

- 函数中的this

this引用的是函数据以执行的环境对象（当在网页的全局作用域调用函数时，this对象是window）

```
window.color = 'red';
var obj = {color: 'blue'};

function sayColor() {
    console.info(this.color);
}

sayColor(); // red

obj.sayColor = sayColor;
obj.sayColor(); // blue
```

- 函数对象的caller

调用当前函数的函数引用

```
function outer() {
    inner();
}

function inner() {
    console.info(arguments.callee.caller); // 返回outer的引用
}

outer();
```

- 函数的属性和方法

length：用于获取函数形参的个数

- Function: call(scope, var1, var2);

- Function: apply(scope, Array);

- Function: bind(scope);
@return 函数的实例

```
var color = 'red';
var obj = {color: 'blue'};

function sayColor() {
    console.info(this.color);
}

sayColor.call();
sayColor.call(window);
sayColor.call(obj);

var s = sayColor().bind(obj);
s();
```

### 基本包装类型

基本类型在调用类似`substring`这类方法时，会临时创建包装类型用于调用方法。

```
var str = 'something';
var s = str.substring(2);
c(s);

// 展示包装对象的原理
var strObj = String(str);
var tmpS = strObj.substring(2);
// 立即销毁包装对象
tmpS = null;

// 包装对象会立即销毁，因此基本类型不能添加属性和方法
str.color = 'blue';
c(str.color); // undefined

// 把字符串传递给Object构造函数，就会创建String的实例
var sObj = new Object('something');
c(sObj instanceof String); // true
```

#### Boolean

```
var o = new Boolean(false)
c(o && true)
c(o.valueOf() && true)
c(o.valueOf())
```

#### Number

- Function: vauleOf()

- Function: toString()

- Function: toFixed()
参数为要保留的小数位数

- Function: toExponential()
e表示法，参数为要保留的小数位数

- Function: toPercision()
参数为要保留的数值位数

#### String

访问字符串

- Function: charAt()

- Function: charCodeAt()

```
var str = "hello word!";

// 访问字符串 参数为字符串的位置值
c(str.charAt(2)); // l
c(str.charCodeAt(2)); // 108 返回相应字符的字符编码

// 获取字符位置
// indexOf
// indexLastOf

// 去掉空格
// tirm

// 截取字符串
// slice
// substr
// substring

// 连接字符串
// concat
// +

// 比较字符串
// localeCompare

// 转换大小写
// toLowerCase
// toLocaleLowerCase
// toUpperCase
// toLocaleUpperCase

// 替换字符串
// replace

// 查找字符串
// match
// search

// 分割字符串
// split

// 静态方法
// fromCharCode
```

## 5.7 单体内置对象

### Math对象

在所有代码执行之前，作用域中就存在两个内置对象：Global、Math

```
// Math

function c(val) {
    console.info(val);
}

var _min = Math.min(1,2,3);
c(_min);

var _max = Math.max(1,2,3);
c(_max);

// 获取数组的最值
var arr = [9,8,7];
var arrMin = Math.min.apply(Math, arr);
c(arrMin);

var arrMax = Math.max.apply(Math, arr);
c(arrMax);

// floor
var num = 1.35;
var numFloor = Math.floor(num);
c(numFloor);

// ceil
var numCeil = Math.ceil(num);
c(numCeil);

// round
var numRound = Math.round(num);
c(numRound);

// random 随机数大于0小于1
var numRandom = Math.random();
c(numRandom);
c(numRandom>0); // true
c(numRandom<1); // true

// 获取两个数之间的随机整数
// 公式：值 = Math.floor(Math.random() * 随机数的总个数 + 最小可能值)
// 随机数的总个数 = 最大数 - 最小数 + 1;
// 根据以上公式取大于等于1小于等于100的整数
var _1to100 = Math.floor(Math.random()*100 + 1);
c(_1to100);

// 获取一个随机的颜色
var color = ["red", "blue", "white", "black", "grey", "pink"];
function randomFrom($min, $max) {
    return randomInt = Math.floor(Math.random() * ($max - $min + 1) + $min);
}
var n = randomFrom(0, color.length-1);
var randomColor = color[n];
c(randomColor);
```

# 第6章 面向对象的程序设计

## 理解对象

### 属性类型

```
function c(val) {
    console.info(val)
}

// Object.defineProperty

// 数据属性
// configurable enumerable writable value
var person = {
    name: 'tom',
    age: 15
};

Object.defineProperty(person, 'name', {
    configurable: true,// 设置为false后 属性不能delete 且无法再置为true
    enumerable: false, // 不能通过for in 枚举
    writable: false,// 不可写
    value: 'tom'
});

person.name = 'jack';
c(person.name); // tom

for(v in person) {
    c(v); // age
}

// 访问类型
// configurable enumerable getter setter

// 特别需要注意的是：以下属性`_year`和`year`
// 由于book._year已经定义，在调用difineProperty时如果再使用`_year`会报栈溢出的错误
// Maximum call stack size exceeded
// https://stackoverflow.com/questions/37502163/getter-setter-maximum-call-stack-size-exceeded-error
var book = {
    _year: 2018,
    edition: 1
};

Object.defineProperty(book, 'year', {
    get: function () {
        return this._year;
    },
    set: function (newYear) {
        if(newYear > 2018) {
            this._year = newYear;
            return this.edition += newYear - 2018;
        }
    }
});

book.year = 2020;
c(book.edition); // 3
```

### 定义多个属性

```
function c(val) {
    console.info(val);
}

// Object.defineProperties
var book = {};

Object.defineProperties(book, {
    _year: {
        value: 2018
    },
    edition: {
        value: 1
    },
    year: {
        get: function () {
            return this._year;
        },
        set: function (newYear) {
            this._year = newYear;
            this.edition += newYear - 2018;
        }
    }
});


// Object.getOwnPropertyDescriptor
var descriptor1 = Object.getOwnPropertyDescriptor(book, 'year');
c(typeof descriptor1.get);

var descriptor2 = Object.getOwnPropertyDescriptor(book, '_year');
c(descriptor2.value);
```

### 6.3继承

#### 借用构造函数

```javascript
function superType(name) {
    this.colors = ['red', 'blue', 'white'];
    this.name = name;
}

function subType() {

    superType.call(this, 'tom');

    this.age = 14;
}

// 子类型subType继承自超级类型superType
var instance1 = new subType();
console.info(instance1.colors);
console.info(instance1.name);
console.info(instance1.age);
```

```javascript
// 理解this
function superType() {
    // 如果函数在全局作用域直接调用，
    // 那么此处的this指代Global，在浏览器环境中即window
    this.colors = ['red', 'blue', 'white'];
}

function subType() {
    // 如果函数在全局作用域直接调用，
    // 那么此处的this指代Global，在浏览器环境中即window
    // ================================================
    // 如果对函数以new操作符产生一个实例对象，
    // 那么this指代实例对象的作用域
    superType.call(this);
}

// 全局作用域调用superType
superType();
console.info(window.colors); // ['red', 'blue', 'white']

// 以new操作符产生一个实例对象，
var ins = new subType();
console.info(ins.colors); // ['red', 'blue', 'white']
```

#### 组合继承

```
// 组合式继承
function SuperType(name) {
    this.name = name;
    this.age = 15;
}

SuperType.prototype.sayName = function () {
    console.info(this.name);
}

function SubType(name) {
    SuperType.call(this, name);
}

SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;
var fun = new SuperType('tom');
fun.sayName();
```

#### 寄生组合式继承

```
// 寄生组合式继承
function inheritPrototype(_sub, _super) {
    var prototype = Object(_super.prototype); // 创建对象
    prototype.constructor = _sub;           // 增强对象
    _sub.prototype = prototype;               // 指定对象
}

function SuperType(age) {
    this.name = 'tom';
    this.age = age;
}

SuperType.prototype.sayAge = function () {
    console.info(this.age);
};

function SubType(age) {
    SuperType.call(this, age);
}

inheritPrototype(SubType, SuperType);

var fun = new SubType('15');
fun.sayAge();
```

## 第7章 函数表达式

### 递归

```
function fac(num) {
    if(num<=1) {
        return 1;
    }else {
        return num * fac(num-1);
    }
}
console.info(ff(5));

function ff(num) {
    if(num<=1) {
        return 1;
    }else {
        return num * arguments.callee(num-1); // 严格模式下不能调用 arguments.callee()
    }
}
console.info(ff(5));

// 严格模式和非严格模式都正确
var fun = (function f(num) {
    if(num<=1) {
        return 1;
    }else {
        return num * f(num-1);
    }
});
console.info(fun(6));
```

### 闭包

闭包是指有权访问一个函数作用域中的变量的函数，创建闭包的常见方式，就是在一个函数内部创建另外一个函数。

```
function fun1(num) {
    return function (name) {
        if(name && num) {
            return name;
        }else {
            return 0;
        }
    }
}

var fun = fun1(15);

console.info(fun('tom'));

// 解除对匿名函数的引用 以释放内存
// 只有匿名函数fun被解除后，fun1才能被垃圾回收机制回收
fun = null;
```

```

function createFun() {
    var res = [];
    for(var i=0; i<10; i++) {
        res[i] = function () {
            return i;
        }
    }

    return res;
}

// 每个匿名函数的作用域琏中都保存着createFun的活动对象，
// 所以所有匿名函数都引用的是同一个变量i
// 当循环执行完，i变为10时，之前的匿名函数的i也都为10了
console.info(createFun()[0]()); // 10
console.info(createFun()[1]()); // 10
console.info(createFun()[9]()); // 10


function createFun1() {
    var res = [];
    for(var i=0; i<10; i++) {
    // 函数的参数是按值传递的，因此num是i的副本，不是引用
        res[i] = function (num) {
            return function () {
                return num;
            };
        }(i);
    }

    return res;
}

console.info(createFun1()[0]()); // 0
console.info(createFun1()[1]()); // 1
console.info(createFun1()[9]()); // 9
```

#### 关于this对象

在全局函数中，this等于window，而当函数被作为某个对象的方法调用时，this等于那个对象。

匿名函数的执行环境具有全局性，因此其this对象通常指向window。

```
var name = 'my window';

var o = {
    name: 'my object',
    getNameFun : function () {
        return function () {
            return this.name;
        }
    }
};

console.info(o.getNameFun()()); // my window
```

```
var name = 'my window';

var o = {
    name: 'my object',
    getNameFun : function () {
        var that = this;
        return function () {
            return that.name;
        }
    }
};

console.info(o.getNameFun()()); // my object
```

```
var name = 'my window';

var o = {
    name: 'my object',
    getNameFun : function () {
        return this.name;
    }
};

console.info(o.getNameFun()); // my object
console.info((o.getNameFun)()); // my object
console.info((o.getNameFun = o.getNameFun)()); // my window
```

#### 模仿块级作用域

```
function(){}(); // 出错！ 因为js会把关键字function视作函数声明的开始，而函数声明后面不能跟圆括号。
(function(){})(); // 正确！因为加上圆括号后就是函数表达式，函数表达式可以接圆括号。

function outp(count) {
    (function () {
        // 模仿块级作用域(私有作用域)
        for(var i=0; i<count; i++) {
            console.info(i); // 可以正常输出
        }
    })();

    console.info(i); // ReferenceError: i is not defined
}

outp(5);
```

## 第8章 BOM

### window对象

window:在浏览器中，window即是访问浏览器窗口的一个接口，又是js的Global对象。
全局变量不能通过delete删除，而定义在window上的属性可以被删除。

```
var age = 28;
window.name = 'tom';

console.info(window.age); // 28
console.info(window.name); // tom

console.info(delete window.age); // false
console.info(delete window.name); // ture

console.info(window.age); // 28
console.info(window.name); // undefined

// 直接访问一个未声明的变量会报错
// 通过window.来访问不会报错，因为这是一次属性查询
window.unkonwnVal
```

### 窗口关系和框架

top:总是指向最外层框架，即浏览器窗口。
parent:总是指向当前框架的直接上层框架，没有框架时等于top。
self:等于window，用于区分top、parent。
top/parent/self都是window的属性。
frame:每个框架都有自己的window对象，可以通过框架集frames进行访问。

```
<!doctype html>
<html lang="zh">
<head>
<meta charset="UTF-8">
<meta name="viewport"
content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>Document</title>
<script>
window.onload = function () {

    console.info(top === window); // true
    console.info(top.frames[0].location.search); // ?s=123
    console.info(top.frames[0].name); // topFrame
}
</script>
</head>
<frameset rows="160, *">
<frame src="topframe.html?s=123" name="topFrame"/>
<frameset cols="50%, 50%">
<frame src="leftframe.html" name="leftFrame"/>
<frame src="rightframe.html" name="rightFrame"/>
</frameset>
</frameset>

</html>
```

## 第10章 DOM

### 节点层次

#### Node类型

1.获取节点Node

通过docuemnt.getElementById()直接获取到节点；
通过document.getElementByClassName()获取到的是节点数组；

```
var tmpNode = document.getElementById('tmpDiv');
```

2.节点下的属性和方法

- nodeType

节点类型共有12种，如`Node.ELEMENT_NODE`其值是1

```
console.log(tmpNode.nodeType == 1); // true
console.log(tmpNode.nodeType == Node.ELEMENT_NODE); // true
```

- nodeName

```
console.info(tmpNode.nodeName); // DIV
```

- childNodes

每个节点都有childNodes，childNodes包含一个对象NodeList，其是一个类数组的对象。

```
tmpNode.childNodes[0];
tmpNode.childNodes.item(0);
// NodeList对象转换为数组
var tmpNodeArr = Array.prototype.slice.call(tmpNode.childNodes, 0);
```

- parentNode

- previousSibling

- nextSibling

- firstChild

- lastChild

3.操作节点

- appendChild(newNode)

- replaceChild(newNode, replaceNode)

- insertBefore(newNode, referNode)

- removeChild(oldNode)

- cloneChild([true|false])

- normalize()

#### Document类型

document是HTMLDocument的实例，也是window对象的属性。

document.documentElement指向html节点

document.body指向body节点

document的其他属性和方法：

- doctype

- title

- URL

- domain

可写，赋予松散值后不能再被赋予紧绷值，所赋值应是document.URL所包含的。

- referrer

- getElementById() 

- getElementsByTagName()

该方法返回的对象是一个HEMLCollection，它类似于NodeList对象，但HTMLCollection可以通过标签的name属性访问节点。

```
<html>
<head></head>
<body>
<ul>
<li name="first"></li>
<li name="second"></li>
<li name="third"></li>
</ul>
</body>
</html>

// 通过name查找节点
var someNodes = document.getElementsByTagName('li');
var fn = someNodes.namedItem('first');
fn == someNodes['first'];
fn == someNodes[0];
// 获取文档全部节点
var allElements = document.getElementsByTagName('*');
```

- getElementsByName()

- anchors

带name属性的a标签

- links

带href属性的a标签

- images

- forms

- write()

- writeln()

#### Element类型

1. 获取Element

   - getElementById()

   - getElementsByName()
...

2. 创建Element

   - createElement()

3. 操作Element的特性

   - getAttribute()

   - setAttribute()

   - removeAttribute()

4. 公认特性如`id`可以通过属性直接操作

```
var element = document.getElementById('myDiv');
element.id = 'otherDiv';
```

#### Text类型

1. 创建文本节点

   - document.createTextNode()

2. 操作文本节点

   - textNode.appendData(text)
将text添加到文本节点textNode的末尾

   - textNode.deleteData(offset, count)
从offset指定的位置开始删除count个字符

   - textNode.insertData(offset, text)
从offset位置插入text

   - textNode.replaceData(offset, count, text)
用text替换从offset位置开始到offset+count结束的文本

   - textNode.spliteText(offset)
从offset位置将当前文本分成两个文本节点

   - textNode.substringData(offset, count)
提前从offset到offset+count为止的字符串

   - normalize()

     ```
     const dv = document.createElement('div');
     const tn1 = document.createTextNode('Hello word!');
     const tn2 = document.createTextNode('Yep!');
     
     dv.appendChild(tn1);
     dv.appendChild(tn2);
     
     console.info(dv.childNodes);
     
     dv.normalize();
     console.info(dv.childNodes);
     dv.firstChild.splitText(5);
     console.info(dv.childNodes);
     ```

#### DocumentFragment类型

文档片段，可以避免浏览器的多次渲染。

```
var fragment = document.createDocumentFragment();
var ul = document.getElementById('myul');
var li = null;
for(var i = 0; i < 3; i++) {
    li = document.createElement('li');
    li.appendChild(document.createTextNode('item'+i));
    fragment.appendChild(li);
}

ul.appendChild(fragment);


```

## 第11章 DOM扩展

### 选择符API

- Selector API Level 1

    - querySelector

    - querySelectorAll

- Selector API Level 2

    - matchesSelector
    各浏览器支持时需要加加前缀 如：webkitMatchSelector
