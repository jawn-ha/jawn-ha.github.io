---
title: Javascript toString()、toLocaleString()、valueOf()三个方法的区别
date: 2018-08-21 22:16:53
tags: JavaScript
---

最近在复习JavaScript 基础，打算把ES5, ES6重过一遍，也算是查漏补缺吧。最近看高程的时候，发现后面几章写的都是精华啊，可是以前图样图森破（好吧，其实主要看不懂）没看，所以决定再从头好好看一下。可能最近的内容都比较简单琐碎，以后抽时间做个系统的整理。今天先总结一哈toString()、toLocaleString()、valueOf()三个方法的区别。

<!--more-->

#### Array

```javascript
var array = new Array("jawn",1,true);
console.log(array.valueOf());//[ 'jawn', 1, true ]
console.log(array.toString());//jawn,1,true
console.log(array.toLocaleString());//jawn,1,true
```

- valueOf：返回数组本身
- toString()：把数组转换为字符串，并返回结果，每一项以逗号分割。
- toLocalString()：把数组转换为本地数组，并返回结果。

#### Boolean

```javascript
var boolean = new Boolean();
console.log(boolean.valueOf()); //false
console.log(boolean.toString()); //false
```

+ valueOf：返回 Boolean 对象的原始值。
+ toString()：根据原始布尔值或者 booleanObject 对象的值返回字符串 "true" 或 "false"。默认为"false"。
+ toLocalString()：Boolean对象没有toLocalString()方法。但是在Boolean对象上使用这个方法也不会报错。

#### Date

```
var date = new Date();
console.log(date.valueOf());//1534862077184
console.log(date.toString());// Tue Aug 21 2018 22:34:37 GMT+0800 (CST)
console.log(date.toLocaleString()); //2018/8/21 下午10:34:37
```

- valueOf：返回 Date 对象的原始值，以毫秒表示。

- toString()：把 Date 对象转换为字符串，并返回结果。使用本地时间表示。

- toLocalString()：可根据本地时间把 Date 对象转换为字符串，并返回结果，返回的字符串根据本地规则格式化。

#### Math

```
console.log(Math.PI.valueOf()); //3.141592653589793
```

+ valueOf：返回 Math 对象的原始值。 

#### Number

```
ar num = new Number(1337);
console.log(num.valueOf());//1337
console.log(num.toString());//1337
console.log(num.toLocaleString());//1,337
```

- valueOf：返回一个 Number 对象的基本数字值。
- toString()：把数字转换为字符串，使用指定的基数。
- toLocalString()：把数字转换为字符串，使用本地数字格式顺序

#### 总结

- toLocalString()是调用每个数组元素的 toLocaleString() 方法，然后使用地区特定的分隔符把生成的字符串连接起来，形成一个字符串。
- toString()方法获取的是String(传统字符串),而toLocaleString()方法获取的是LocaleString(本地环境字符串)。
- 如果你开发的脚本在世界范围都有人使用，那么将对象转换成字符串时请使用toString()方法来完成。
- LocaleString()会根据你机器的本地环境来返回字符串，它和toString()返回的值在不同的本地环境下使用的符号会有微妙的变化。
- 所以使用toString()是保险的，返回唯一值的方法,它不会因为本地环境的改变而发生变化。如果是为了返回时间类型的数据，推荐使用LocaleString()。若是在后台处理字符串，请务必使用toString()。