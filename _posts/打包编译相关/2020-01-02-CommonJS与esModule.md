---
layout: default
title:  "CommonJS 与 ES6 Module"
date:   2020-01-02 17:50:00
des:  ""
motto:  ""
categories: 打包编译
---

#### CommonJS
require 路径可动态指定;

支持表达式动判断加载某个模块

CommonJS 是值拷贝，可编辑;

{% highlight js linenos %}
let csCount = 0;

module.exports = {
  csCount,
  csCountAdd: () => {
    csCount += 10;
  }
};

{% endhighlight %}

#### ES6 Module

导入、导出语句是声明式的；

路径不支持表达式；

导入和导出语句必须位于模块的顶层作用域（不能放在 if 语句中）；

ES6 Module 是址引用，即映射，只读，即不可编辑；

{% highlight js linenos %}
let esCount = 0;
const esCountAdd = () => {
  esCount += 10;
};

export { esCount, esCountAdd };

{% endhighlight %}

{% highlight js linenos %}
// 06/src/index.js
// CommonJS Module
let csCount = require('./commonJs').csCount;
let csCountAdd = require('./commonJs').csCountAdd;

console.log(`----commonjs 初次加载----\n${csCount}`);
csCountAdd();
console.log(`----commonjs 内部自加 10 -----\n${csCount}`);
csCount += 20;
console.log(`----commonjs 启动项自加 20------\n${csCount}`);

// Es6 Module
import { esCount, esCountAdd } from './es6-module.js';
console.log(`----es6 初次加载----\n${esCount}`);
esCountAdd();
console.log(`----es6 内部自加 10 -----\n${esCount}`);
esCount += 20;
console.log(`----es6 启动项自加 20------\n${esCount}`);

{% endhighlight %}

### ES6 Module 优势
僵尸代码检测和排除，减小资源打包体积。

编译器优化，动态模块的导入是一个对象，而 ES6 Module 可直接导入变量，减少引用层级，提高程序效率；

