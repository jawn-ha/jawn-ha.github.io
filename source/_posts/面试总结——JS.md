---
title: 面试总结——JS
date: 2018-07-23 12:06:10
tags: 面试
---

+ 介绍一下 JS 的基本数据类型。

```
Undefined、Null、Boolean、Number、String、Symbol(ES6新增)
```

- 如何控制alert中的换行？

```
\n alert(“p\np”);
```

<!--more-->


- null 和 undefined 有何区别？

> null 表示一个对象被定义了，值为“空值”；
>
>  undefined 表示不存在这个值。
>
>  typeof undefined //"undefined" 
>
> undefined :是一个表示"无"的原始值或者说表示"缺少值"，就是此处应该有一个值，但是还没有定义。当尝试读取时会返回 undefined；  例如变量被声明了，但没有赋值时，就等于undefined。
>
>  typeof null //"object"  null : 是一个对象(空对象, 没有任何属性和方法)；  例如作为函数的参数，表示该函数的参数不是对象； 
>
> 注意： 在验证null时，一定要使用　=== ，因为 == 无法分别 null 和　undefined

- call和apply,bind的作用是什么？区别是什么？

> call和apply的功能基本相同，都是实现继承或者转换对象指针的作用；
> 唯一不通的是前者参数是罗列出来的，后者是存到数组中的；
> call或apply功能就是实现继承的；与面向对象的继承extends功能相似；但写法不同；
> 语法：
> .call(对象[,参数1，参数2,....]);//此地参数是指的是对象的参数，非方法的参数；
> .apply(对象,参数数组)//参数数组的形式:[参数1，参数2,......]
>
>通过bind改变this作用域会返回一个新的函数，这个函数不会马上执行

+ push()-pop()-shift()-unshift()分别是什么功能？

> push 方法 将新元素添加到一个数组中，并返回数组的新长度值。 var a=[1,2,3,4]; a.push(5);
>
>  pop 方法 移除数组中的最后一个元素并返回该元素。 var a=[1,2,3,4]; a.pop(); 
>
> shift 方法 移除数组中的第一个元素并返回该元素。 var a=[1,2]; alert(a.shift());
>
>  unshift 方法 将指定的元素插入数组开始位置并返回该数组。

+ new 操作符具体干了什么呢？

>1、创建了一个空对象
>
>var obj = new object();
>
>2、设置原型链
>
>obj._proto_ = fn.prototype;
>
>3、让fn的this指向obj，并执行fn的函数体
>
>var result = fn.call(obj);
>
>4、判断fn的返回值类型，如果是值类型，返回obj。如果是引用类型，就返回这个引用类型的对象
>
>if (typeof(result) == "object"){  
>
>​    fnObj = result;  
>
>​    } else {  
>
>​    fnObj = obj;
>
>}

+ 闭包

> 一句话可以概括：闭包就是能够读取其他函数内部变量的函数，或者子函数在外调用，子函数所在的父函数的作用域不会被释放。

+ 如何理解 JavaScript 的原型

> 所有的引用类型（数组、对象、函数），都具有对象特性，即可自由扩展属性（null除外）
>
>  所有的引用类型（数组、对象、函数），都有一个__proto__属性，属性值是一个普通的对象 
>
> 所有的函数，都有一个prototype属性，属性值也是一个普通的对象 
>
> 所有的引用类型（数组、对象、函数），__proto__属性值指向它的构造函数的prototype属性值
>
> **当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的__proto__（即它的构造函数的prototype）中寻找**。

- JavaScript 中，有一个函数，执行对象查找时，永远不会去查找原型，这个函数是哪个？

> hasOwnProperty 
>
> // 
>
> JavaScript 中 hasOwnProperty 函数方法是返回一个布尔值，指出一个对象是否具有指定名称的属性。此方法无法检查该对象的原型链中是否具有该属性；该属性必须是对象本身的一个成员。
>
>  // 
>
> 使用方法：object.hasOwnProperty(proName)其中参数object是必选项，一个对象的实例。proName是必选项，一个属性名称的字符串值。
>
>  // 
>
> 如果 object 具有指定名称的属性，那么JavaScript中hasOwnProperty函数方法返回 true，反之则返回 false。

- JavaScript 有几种类型的值？能否画一下它们的内存图？

> + 栈：原始数据类型（Undefined，Null，Boolean，Number，String）
> + 堆：引用数据类型（对象、数组、函数） 两种类型的区别： 
>
> //存储位置不同
>
> + 原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储； 
> + 引用数据类型存储在堆(heap)中的对象,占据空间大、大小不固定,如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。

+ JavaScript 如何实现继承？

```
ES5
（1）类的创建（es5）：new一个function，在这个function的prototype里面增加属性和方法。
下面来创建一个Animal类：
// 定义一个动物类
function Animal (name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
}
// 原型方法
Animal.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};
复制代码
这样就生成了一个Animal类，实力化生成对象后，有方法和属性。
（2）类的继承——原型链继承
--原型链继承
function Cat(){ }
Cat.prototype = new Animal();
Cat.prototype.name = 'cat';
//&emsp;Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.eat('fish'));
console.log(cat.sleep());
console.log(cat instanceof Animal); //true 
console.log(cat instanceof Cat); //true


介绍：在这里我们可以看到new了一个空对象,这个空对象指向Animal并且Cat.prototype指向了这个空对象，这种就是基于原型链的继承。
特点：基于原型链，既是父类的实例，也是子类的实例
缺点：无法实现多继承

（3）构造继承：使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // false
console.log(cat instanceof Cat); // true

特点：可以实现多继承
缺点：只能继承父类实例的属性和方法，不能继承原型上的属性和方法。

（4）实例继承和拷贝继承
实例继承：为父类实例添加新特性，作为子类实例返回
拷贝继承：拷贝父类元素上的属性和方法
上述两个实用性不强，不一一举例。
（5）组合继承：相当于构造继承和原型链继承的组合体。通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); // true
复制代码

特点：可以继承实例属性/方法，也可以继承原型属性/方法
缺点：调用了两次父类构造函数，生成了两份实例

（6）寄生组合继承：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); //true


ES6
class 是一种新的语法形式，是class Name {...}这种形式，和函数的写法完全不一样
两者对比，构造函数函数体的内容要放在 class 中的constructor函数中，constructor即构造器，初始化实例时默认执行
class 中函数的写法是add() {...}这种形式，并没有function关键字
使用 class 来实现继承就更加简单了，至少比构造函数实现继承简单很多。看下面例子...

class Animal {
    constructor(name) {
        this.name = name
    }
    eat() {
        console.log(`${this.name} eat`)
    }
}

class Dog extends Animal {
    constructor(name) {
        super(name)
        this.name = name
    }
    say() {
        console.log(`${this.name} say`)
    }
}
const dog = new Dog('哈士奇')
dog.say()
dog.eat()
注意以下两点：

使用extends即可实现继承，更加符合经典面向对象语言的写法，如 Java
子类的constructor一定要执行super()，以调用父类的constructor...

```

- Ajax 是什么？如何创建一个 Ajax ？

> ajax的全称：Asynchronous Javascript And XML，异步传输+js+xml。 
>
> 所谓异步，在这里简单地解释就是：向服务器发送请求的时候，我们不必等待结果，而是可以同时做其他的事情，等到有了结果它自己会根据设定进行后续操作，与此同时，页面是不会发生整页刷新的，提高了用户体验。
>
>  // 
>
> (1)创建XMLHttpRequest对象,也就是创建一个异步调用对象 
>
> (2)创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息 
>
> (3)设置响应HTTP请求状态变化的函数 
>
> (4)发送HTTP请求
>
>  (5)获取异步调用返回的数据 
>
> (6)使用JavaScript和DOM实现局部刷新
>
> ```javascript
> var xmlHttp = new XMLHttpRequest();
> xmlHttp.open("GET",demo.php,"true");
> xmlHttp.send();
> xmlHttp.onreadystatechange = function() {
>     if(xmlHttp.readyState === 4 & xmlHttp.status=== 200){
>         ...
>     }
> }
> ```
>

+  前端中的事件流
> HTML中与javascript交互是通过事件驱动来实现的，例如鼠标点击事件onclick、页面的滚动事件onscroll等等，可以向文档或者文档中的元素添加事件侦听器来预订事件。想要知道这些事件是在什么时候进行调用的，就需要了解一下“事件流”的概念。
>
> 什么是事件流：事件流描述的是从页面中接收事件的顺序,DOM2级事件流包括下面几个阶段。
>
> - 事件捕获阶段
> - 处于目标阶段
> - 事件冒泡阶段
>
> **addEventListener**：**addEventListener** 是DOM2 级事件新增的指定事件处理程序的操作，这个方法接收3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。最后这个布尔值参数如果是true，表示在捕获阶段调用事件处理程序；如果是false，表示在冒泡阶段调用事件处理程序。
>
> **IE只支持事件冒泡**。

+ 如何让事件先冒泡后捕获

> 在DOM标准事件模型中，是先捕获后冒泡。但是如果要实现先冒泡后捕获的效果，对于同一个事件，监听捕获和冒泡，分别对应相应的处理函数，监听到捕获事件，先暂缓执行，直到冒泡事件被捕获后再执行捕获之间。

+ 事件委托

> - 简介：事件委托指的是，不在事件的发生地（直接dom）上设置监听函数，而是在其父元素上设置监听函数，通过事件冒泡，父元素可以监听到子元素上事件的触发，通过判断事件发生元素DOM的类型，来做出不同的响应。
> - 举例：最经典的就是ul和li标签的事件监听，比如我们在添加事件时候，采用事件委托机制，不会在li标签上直接添加，而是在ul父元素上添加。
> - 好处：比较合适动态元素的绑定，新添加的子元素也会有监听函数，也可以有事件触发机制。

+ 如何实现sleep的效果（es5或者es6）
> (1)while循环的方式
>
> ```
> function sleep(ms){
>    var start=Date.now(),expire=start+ms;
>    while(Date.now()<expire);
>    console.log('1111');
>    return;
> }
> ```
>
> 执行sleep(1000)之后，休眠了1000ms之后输出了1111。上述循环的方式缺点很明显，容易造成死循环。
>
>  (2)通过promise来实现
>
> ```
> function sleep(ms){
>   var temple=new Promise(
>   (resolve)=>{
>   console.log(111);setTimeout(resolve,ms)
>   });
>   return temple
> }
> sleep(500).then(function(){
>    //console.log(222)
> })
> //先输出了111，延迟500ms后输出222
> 复制代码
> ```
>
>  (3)通过async封装
>
> ```
> function sleep(ms){
>   return new Promise((resolve)=>setTimeout(resolve,ms));
> }
> async function test(){
>   var temple=await sleep(1000);
>   console.log(1111)
>   return temple
> }
> test();
> //延迟1000ms输出了1111
> 复制代码
> ```
>
> (4).通过generate来实现
>
> ```
> function* sleep(ms){
>    yield new Promise(function(resolve,reject){
>              console.log(111);
>              setTimeout(resolve,ms);
>         })  
> }
> sleep(500).next().value.then(function(){console.log(2222)})
> ```
>

+ 简单的实现一个promise
> 首先明确什么是promiseA+规范，参考规范的地址：
>
> [primiseA+规范](https://link.juejin.im/?target=https%3A%2F%2Fpromisesaplus.com%2F)
>
> 如何实现一个promise，参考文章：
>
> [实现一个完美符合Promise/A+规范的Promise](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fforthealllight%2Fblog%2Fissues%2F4)

```javascript
function myPromise(constructor){
    let self=this;
    self.status="pending" //定义状态改变前的初始状态
    self.value=undefined;//定义状态为resolved的时候的状态
    self.reason=undefined;//定义状态为rejected的时候的状态
    function resolve(value){
        //两个==="pending"，保证了状态的改变是不可逆的
       if(self.status==="pending"){
          self.value=value;
          self.status="resolved";
       }
    }
    function reject(reason){
        //两个==="pending"，保证了状态的改变是不可逆的
       if(self.status==="pending"){
          self.reason=reason;
          self.status="rejected";
       }
    }
    //捕获构造异常
    try{
       constructor(resolve,reject);
    }catch(e){
       reject(e);
    }
}

//同时，需要在myPromise的原型上定义链式调用的then方法：

myPromise.prototype.then=function(onFullfilled,onRejected){
   let self=this;
   switch(self.status){
      case "resolved":
        onFullfilled(self.value);
        break;
      case "rejected":
        onRejected(self.reason);
        break;
      default:       
   }
}
```

- 请编写一个JavaScript函数 parseQueryString，它的用途是把URL参数解析为一个对象，如：
  var url = “[http://witmax.cn/index.php?key0=0&key1=1&key2=2](https://link.jianshu.com/?t=http://witmax.cn/index.php?key0=0&key1=1&key2=2)″;

```
function parseQueryString(url){
    var params = {},
        arr = url.split("?");
    if (arr.length <= 1)
        return params;
    arr = arr[1].split("&");
    for(var i=0, l=arr.length; i<l; i++){
        var a = arr[i].split("=");
        params[a[0]] = a[1];
    }
    return params;
}
var url = "http://witmax.cn/index.php?key0=0&key1=1&key2=2",
    ps = parseQueryString(url);
console.log(ps["key1"]);
```

- 如何获取光标的水平位置？

```
function getX(e){
    e = e || window.event;
    //先检查非IE浏览器，在检查IE的位置
    return e.pageX || e.clentX + document.body.scrollLeft;
} 
```

- 如何规避javascript多人开发函数重名问题？

```
(1) 可以开发前规定命名规范，根据不同开发人员开发的功能在函数前加前缀
(2) 将每个开发人员的函数封装到类中，调用的时候就调用类的函数，即使函数重名只要类名不重复就行
```



- AJAX请求总共有多少种CALLBACK

```
Ajax请求总共有八种Callback
onSuccess
onFailure
onUninitialized
onLoading
onLoaded
onInteractive
onComplete
onException
```

- 数组去重

```
方法一:
var arr1=[1,2,3,2,1,2];
    function repeat1(arr){
      for(var i=0,arr2=[];i<arr.length;i++){
        if(arr2.indexOf(arr[i])==-1){
          arr2.push(arr[i]);
        }
      }//(遍历结束)
      return arr2;
}
方法二:hash
function repeat2(arr){
      //遍历arr中每个元素，同时声明hash
      for(var i=0,hash={};i<arr.length;i++){
       //hash中是否包含当前元素值的建
    //如果不包含,就hash添加一个新元素，以当前元素值为key，value默认为1
        if(hash[arr[i]]===undefined){
          hash[arr[i]]=1;
        }
      }//(遍历结束)
      //将hash转为索引:
      var i=0;
      var arr2=[];
      for(arr2[i++] in hash);
      return arr2;
}
方法三:
function repeat3(arr){
      return arr.sort()
               .join(",,")
               .replace(
                /(^|,,)([^,]+)(,,\2)*/g,
                "$1$2")
               .split(",,");
    }
 console.log(repeat3(arr1));
```

- 快速排序:

```
    function quickSort(arr){
      //如果arr的length<=1
      if(arr.length<=1){
        return arr;//就直接返回arr
      }
      //计算参照位p
      var p=Math.floor(arr.length/2);
      var left=[];
      var right=[];
      //删除p位置的元素
      var center=arr.splice(p,1)[0];
      //遍历arr中每个元素
      for(var i=0;i<arr.length;i++){
        if(arr[i]>=center){
          right.push(arr[i]);
        }else{
          left.push(arr[i]);
        }
      }
      return quickSort(left) .concat(center,quickSort(right))
    }
    var sortedArr=quickSort(arr);
```

- 深克隆原理

```javascript
Object.clone=function(obj){//深克隆
if(typeof(obj)=="object"){//如果obj是对象
    var o=//有必要区分数组和普通对象
    Object.prototype.toString.call(obj)=="[object Array]"?[]:{};
        for(var key in obj){//遍历obj的自有属性
            //如果key是obj的自有属性
            if(obj.hasOwnProperty(key)){
                o[key]=arguments.callee(obj[key]);//arguments.callee调的是当前的Object.clone函数
                }
        }
        return o;
        }else{//如果obj是原始类型的值，就直接返回副本
            return obj;
        }
    }
```

 bind函数的实现原理

```javascript
Function.prototype.bind2=function(obj/*，参数列表*/){
        var fun=this;//留住this
                //*****将类数组对象，转化为普通数组
        var args=Array.prototype.slice.call(arguments,1);
        //args保存的就是提前绑定的参数列表
        return function(){
                   //将后传入的参数值，转为普通数组      
           var innerArgs=Array.prototype.slice.call(arguments);//将之前绑定的参数值和新传入的参数值，拼接为完整参数之列表
           var allArgs=args.concat(innerArgs)
          //调用原始函数fun，替换this为obj，传入所有参数
          fun.apply(obj,allArgs);
        }
       }
    }

```

+ 字符串转数字的方法

> 1. js提供了parseInt()和parseFloat()两个转换函数
>
> 2. 强制类型转换
>
>    ECMAScript中可用的3种强制类型转换如下： Boolean(value)——把给定的值转换成Boolean型； Number(value)——把给定的值转换成数字（可以是整数或浮点数）； String(value)——把给定的值转换成字符串。
>
> 3.  利用js变量弱类型转换

+ 重绘和回流（考察频率：中）

> - 重绘：当页面中元素样式的改变并不影响它在文档流中的位置时（例如：color、background-color、visibility等），浏览器会将新样式赋予给元素并重新绘制它，这个过程称为重绘。
> - 回流：当Render Tree（DOM）中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为回流。
> - 回流要比重绘消耗性能开支更大。
> - 回流必将引起重绘，重绘不一定会引起回流。
- 请给出异步加载js方案，不少于两种

```
异步加载方式：
defer，只支持IE
async：
创建script，插入到DOM中，加载完毕后callBack，见代码：
function loadScript(url, callback){
      var script = document.createElement("script")
      script.type = "text/javascript";
      if(script.readyState){ //IE
          script.onreadystatechange = function(){
               if (script.readyState == "loaded" ||script.readyState == "complete"){
                      script.onreadystatechange = null;
                      callback();
               }
         };
     } else {
//Others: Firefox, Safari, Chrome, and Opera
         script.onload = function(){
            callback();
         };
    }
    script.src = url;
    document.body.appendChild(script);
}
```

Promise Promise是 CommonJS 提出来的这一种规范，有多个版本，在 ES6 当中已经纳入规范，原生支持 Promise 对象，非 ES6 环境可以用类似 Bluebird、Q 这类库来支持。 Promise 可以将回调变成链式调用写法，流程更加清晰，代码更加优雅。 简单归纳下 Promise：三个状态、两个过程、一个方法，快速记忆方法：3-2-1 三个状态：pending、fulfilled、rejected 两个过程： pending→fulfilled（resolve） pending→rejected（reject） 一个方法：then 当然还有其他概念，如catch、 Promise.all/race，这里就不展开了。

 

 

 

 

 

 

 

 

 

 

 

 