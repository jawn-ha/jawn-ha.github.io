---
title: Vue指令
date: 2018-07-05 22:08:23
tags: Vue
---

###  常用指令列表

> 1.  v-model
> 2. v-if
> 3. v-else
> 4. v-show
> 5. v-for
> 6. v-bind  —— 可简写为 :class="qq"
> 7. v-on —— 可简写为：@click="qq"
> 8. v-text & v-html
> 9. v-pre & v-cloak
> 10. v-once

<!--more-->

## v-model 双向绑定

```
<input type="text" v-model="message" />
<div> {{ message }} </div>
```

## v-if 条件渲染，根据表达式的真假来删除和插入元素

```
<div v-if="true">当表达式为true，插入该条数据</div>
<div v-if="false">当表达式为flase，删除该条数据</div>
<div v-if="age >  25">当表达式为true，插入该条数据</div>
```

```
<div>
<p v-if="a>18">年龄：{{ data.age }}</p>
<p v-else>年龄：属于未成年</p>
</div>
```

>#### v-if 和v-show的区别：
>
>- v-if： 判断是否加载，可以减轻服务器的压力，在需要时加载。
>
>- v-show：调整css dispaly属性，可以使客户端操作更加流畅。v-else 紧跟着v-if，添加一个else块，否则将不会被识别

## v-show 条件渲染，元素会始终渲染到HTML

```
<div v-show="true">当表达式为true，显示该条数据</div>
<div v-show="false">当表达式为false，隐藏该条数据</div>
<div v-show="a > 18">当表达式为true，显示该条数据</div>
```

### v-for指令基于一个数组渲染一个列表

```
<table>
<tr><td>姓名</td><td>年龄</td><td><性别/td></tr>
<tr v-for="persion in people">
    <td>{{ persion.name }}</td>
    <td>{{ persion.age }}</td>
    <td>{{ persion.sex }}</td>
</tr>
</table>
```

## v-bind指令，带一个参数，参数通常是HTML元素的特性

```
<div v-bind:class="active"></div>
<div :class="active"></div>
```

## v-on指令用于监听DOM事件

```
<a v-on:click=""doSet></a>
<a @:click=""doSet></a>
```

 

###  v-text & v-html

 我们已经用的是{{xxx}},这种情况是有弊端的，就是当我们网速很慢或者javascript出错时，会暴露我们的{{xxx}}。Vue给我们提供的v-text,就是解决这个问题的。我们来看代码： 

 ` <span>{{ message }}</span>=<span v-text="message"></span><br/>` 

如果在javascript中写有html标签，用v-text是输出不出来的，这时候我们就需要用v-html标签了。

`<span v-html="msgHtml"></span>`

双大括号会将数据解释为纯文本，而非HTML。为了输出真正的HTML，你就需要使用v-html 指令。



###  v-pre & v-clak 

在模板中跳过vue的编译，直接输出原始值。就是在标签中加入v-pre就不会输出vue中的data值了。

`<div v-pre>{{message}}</div>`

v-cloak:在vue渲染完指定的整个DOM后才进行显示。它必须和CSS样式一起使用

```css
[v-cloak] {
  display: none;
}
```

 ``` 
<div v-cloak>
  {{ message }}
</div>
 ```

### v-once指令 

在第一次DOM时进行渲染，渲染完成后视为静态内容，跳出以后的渲染过程。

```
<div v-once>第一次绑定的值：{{message}}</div>
<div><input type="text" v-model="message"></div>
```

