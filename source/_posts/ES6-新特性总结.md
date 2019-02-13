---
title: ES6 新特性总结(1)
date: 2018-07-08 21:59:51
tags: ES6
---

### 1.新的声明方式

ES6对声明的进行了扩展，现在可以有三种声明方式了。

字面理解ES6的三种声明方式：

1. var：它是variable的简写，可以理解成变量的意思。
2. let：它在英文中是“让”的意思，也可以理解为一种声明的意思。
3. const：它在英文中也是常量的意思，在ES6也是用来声明常量的，常量你可以简单理解为不变的量。

<!-- more -->

var在ES6里是用来升级全局变量的。let是局部变量声明，let声明只在区块内起作用，外部是不可以调用的。在程序开发中，有些变量是希望声明后在业务层就不再发生变化了，简单来说就是从声明开始，这个变量始终不变，就需要用const进行声明。

### 2.解构赋值

ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构。解构赋值在实际开发中可以大量减少我们的代码量，并且让我们的程序结构更清晰。

#### 2.1简单的数组解构

以前，为变量赋值，我们只能直接指定值。比如下面的代码：

```javascript
let a=0;
let b=1;
let c=2;
```

而现在我们可以用数组解构的方式来进行赋值。

`let [a,b,c]=[1,2,3];`

上面的代码表示，可以从数组中提取值，按照位置的对象关系对变量赋值。

#### 2.2数组模式和赋值模式统一：

可以简单的理解为等号左边和等号右边的形式要统一，如果不统一解构将失败。

```javascript
let [a,[b,c],d]=[1,[2,3],4];
```

如果等号两边形式不一样，很可能获得undefined或者直接报错。

#### 2.3 解构的默认值

解构赋值是允许你使用默认值的，先看一个最简单的默认是的例子。

```javascript
let [foo = true] =[];
console.log(foo); //控制台打印出true
```

我们来试试个多个值的数组，并给他一些默认值。

```
let [a,b="Jawnha"]=['单行道']
console.log(a+b); //控制台显示“单行道Jawnha”
```

现在我们对默认值有所了解，但仍需要注意的是**undefined**和**null**的区别。

```
let [a,b="Jawnha"]=['单行道',undefined];
console.log(a+b); //控制台显示“单行道Jawnha”
```

undefined相当于什么都没有，b是默认值。

```
let [a,b="Jawnha"]=['单行道',null];
console.log(a+b); //控制台显示“Jawnhanull”
```

null相当于有值，但值为null。所以b并没有取默认值，而是解构成了null。

#### 2.4对象的解构赋值

**注意：**对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

```
let {foo,bar} = {foo:'Jawnha',bar:'web'};
console.log(foo+bar); //控制台打印出了“Jawnhaweb”
```

#### 2.5圆括号的使用

如果在解构之前就定义了变量，这时候你再解构会出现问题。下面是错误的代码，编译会报错。

```
let foo;
{foo} ={foo:'Jawnha'};
console.log(foo);
```

要解决报错，使程序正常，我们这时候只要在解构的语句外边加一个圆括号就可以了。

```
let foo;
({foo} ={foo:'Jawnha'});
console.log(foo); //控制台输出Jawnha
```

#### 2.6字符串的解构

```
const [a,b,c,d,e,f]="Jawnha";
console.log(a);
console.log(b);
console.log(c);
console.log(d);
console.log(e);
console.log(f);
```

### 3扩展运算符 & rest运算符

展运算符和rest运算符，它们都是…（三个点）。它们可以很好的为我们解决参数和对象数组未知情况下的编程，让我们的代码更健壮和简洁。

#### 3.1对象扩展运算符(...)

展运算符和rest运算符，它们都是…（三个点）。它们可以很好的为我们解决参数和对象数组未知情况下的编程，让我们的代码更健壮和简洁。

```
function jawnha(...arg){
    console.log(arg[0]);
    console.log(arg[1]);
    console.log(arg[2]);
    console.log(arg[3]);
 
}
jawnha(1,2,3);
```

这时我们看到控制台输出了 1,2,3，undefined，这说明是可以传入多个值，并且就算方法中引用多了也不会报错。

**扩展运算符的用处：**

我们先用一个例子说明，我们声明两个数组arr1和arr2，然后我们把arr1赋值给arr2，然后我们改变arr2的值，你会发现arr1的值也改变了，因为我们这是对内存堆栈的引用，而不是真正的赋值。

```
let arr1=['jawnha','github','io'];
let arr2=arr1;
console.log(arr2);
arr2.push('web');
console.log(arr1);
//输出
//["jawnha","github","io"]
//["jawnha","github","io","web"]
```

这是我们不想看到的，可以利用对象扩展运算符简单的解决这个问题，现在我们对代码进行改造。

```
let arr1=['jawnha','github','io'];
//let arr2=arr1;
let arr2=[...arr1];
console.log(arr2);
arr2.push('web');
console.log(arr1);
```

现在控制台预览时，你可以看到我们的arr1并没有改变，简单的扩展运算符就解决了这个问题。

#### 3.2 rest运算符

rest运算符和扩展运算符有很多呢相似之处，甚至很多时候你不用特意去区分。它也用…（三个点）来表示，我们先来看一个例子。

```
function func(first,...arg){
    console.log(arg.length);
}
 
func(0,1,2,3,4,5,6,7);
```

这时候控制台打印出了7，说明我们arg里有7个数组元素，这就是rest运算符的最简单用法。

如何循环输出rest运算符

这里我们用for…of循环来进行打印出arg的值

```javascript
function func(first,...arg){
    for(let val of arg){
        console.log(val);
    }
}
 
func(0,1,2,3,4,5,6,7);
```

### 4字符串模板

ES6对字符串新增的操作，最重要的就是字符串模版，字符串模版的出现让我们再也不用拼接变量了，而且支持在模板里有简单计算操作。

#### 4.1字符串模版

先来看一个在ES5下我们的字符串拼接案例:

```javascript
let jawnha='jawnha';
let welcome = '非常高兴你能看我博客,我是'+jawnha+'。';
document.write(welcome);
```



ES5下必须用**+jawnha+**这样的形式进行拼接，这样很麻烦而且很容易出错。ES6新增了字符串模版，可以很好的解决这个问题。字符串模版不再使用**‘xxx’**这样的单引号，而是换成了连接号。这时我们再引用jspang变量就需要用${jspang}这种形式了，我们对上边的代码进行改造。

```javascript
let jawnha='jawnha';
let welcome = `非常高兴你能看我博客,我是${jawnha}。`;
document.write(welcome);
```

可以看到浏览器出现了和上边代码一样的结果。而且这里边支持**html标签和运算符**可以试着输入一些。

```
let jawnha='jawnha';
let a=10;
let b=11;
let welcome = `<b>非常高兴你能看我博客</b>,<br/>我是${jawnha}。我今年${a+b}岁`;
document.write(welcome);
```

#### 4.2字符串查找

ES6还增加了字符串的查找功能，而且支持中文。

**查找是否存在:**

先来看一下ES5的写法，其实这种方法并不实用，给我们的索引位置，我们自己还要确定位置。

```
let blog='博客';
let welcome= '非常高兴你能看我博客，我是jawnha。';
document.write(welcome.indexOf(blog));
```

上述代码会返回一个索引值

ES6直接用includes就可以判断，不再返回索引值，这样的结果我们更喜欢，更直接。

```
let blog='博客';
let welcome= '非常高兴你能看我博客，我是jawnha。';
document.write(welcome.includes(blog));
```

**判断开头是否存在：**

```welcome.startsWith(blog);
blog.startsWith(jspang);
```

**判断结尾是否存在：**

```
blog.endsWith(jspang);
```

需要注意的是：starts和ends 后边都要加s，我开始时经常写错，希望小伙伴们不要采坑。

#### 4.3复制字符串

我们有时候是需要字符串重复的，比如分隔符和特殊符号，这时候复制字符串就派上用场了，语法很简单。

```
document.write('jawnha|'.repeat(3));
```

当然ES6对字符串还有一些其它操作，这里就不作太多的介绍了。

### 5 ES6数字操作

####  5.1数字判断和转换

**数字验证Number.isFinite( xx )**

可以使用Number.isFinite( )来进行数字验证，只要是数字，不论是浮点型还是整形都会返回true，其他时候会返回false。

```
let a= 11/4;
console.log(Number.isFinite(a));//true
console.log(Number.isFinite('jspang'));//false
console.log(Number.isFinite(NaN));//false
console.log(Number.isFinite(undefined));//false
```

**NaN验证**

NaN是特殊的非数字，可以使用Number.isNaN()来进行验证。下边的代码控制台返回了true。

```
console.log(Number.isNaN(NaN));
```

**判断是否为整数Number.isInteger(xx)**

```
let a=123.1;
console.log(Number.isInteger(a)); //false
```

**整数转换Number.parseInt(xxx)和浮点型转换Number.parseFloat(xxx)**

```
let a='9.18';
console.log(Number.parseInt(a)); 
console.log(Number.parseFloat(a));
```

#### 5.2整数取值范围操作

整数的操作是有一个取值范围的，它的取值范围就是2的53次方。我们先用程序来看一下这个数字是什么.

```
let a = Math.pow(2,53)-1;
console.log(a); //9007199254740991
```

在我们计算时会经常超出这个值，所以我们要进行判断，ES6提供了一个常数，叫做最大安全整数，以后就不需要我们计算了。

**最大安全整数**

```
console.log(Number.MAX_SAFE_INTEGER);
```

**最小安全整数**

```
console.log(Number.MIN_SAFE_INTEGER);
```

**安全整数判断isSafeInteger( )**

```
let a= Math.pow(2,53)-1;
console.log(Number.isSafeInteger(a));//false
```

