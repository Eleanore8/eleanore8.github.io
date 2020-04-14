---
layout: default
title:  "call、apply、bind"
date:   2020-03-10 17:50:00
des:  ""
motto:  ""
categories: js
---

call: 使用一个指定的 this 值和单独给出的一个或多个参数来调用一个函数。

apply: 调用一个具有给定this值的函数，以及作为一个数组（或类似数组对象）提供的参数

bind: 创建一个新的函数，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

    function Person(name) {
      this.name = name;
    }
    
    Person.prototype.showName = function () {
      console.log(this.name)
    }

    let obj = {
      name: 'herry',
    }

    Person.prototype.showName.call(obj) //herry
    Person.prototype.showName.apply(obj) //herry
    Person.prototype.showName.bind(obj)() //herry

区别：

1）call和apply 改变了函数的this上下文之后便立即执行函数，
bind则是返回改变了上下文后的一个函数。

2）apply传入数组，call为多个参数

手写apply：

    Function.prototype.myApply = function (context) {
      //如果没有参数context指向得是window
      context = context || window
      //传输obj的对象上添加调用的方法，这里this得指向是max
      context.fn = this
      let result
      if (arguments[1]) {
        result = context.fn(...arguments[1])
      } else {
        result = context.fn()
      }
      delete context.fn
      return result
    }
    
    console.log(Math.max.myApply(Math, [34, 5, 3, 6, 54, 6, -67, 5, 7, 6, -8, 687])) //687

call：

    Function.prototype.myCall = function (context) {
      //如果没有参数context指向得是window
      context = context || window
      //传输obj的对象上添加调用的方法，这里this得指向是max
      context.fn = this
      //处理参数 去除第一个参数this 其它传入fn函数
      let arg = [...arguments].slice(1)
      //执行fn
      let result = context.fn(...arg)
      //删除fn
      delete context.fn
      //返回执行结果
      return result
    }
    console.log(Math.max.myCall(Math, 34, 5, 3, 6, 54, 6, -67, 5, 7, 6, -8, 687)) //687

bind：

    Function.prototype.myBind = function (context) {
      //返回一个绑定得this，保存this
      let _this = this
      let arg = [...arguments].slice(1)
      //返回一个函数
      return function F() {
        // 处理函数使用new的情况
        if (this instanceof F) {
          return new _this(...arg, ...arguments)
        } else {
          // 返回函数绑定this，传入两次保存的参数
          //考虑返回函数有返回值做了return
          return _this.apply(context, arg.concat(...arguments))
        }
      }
    }
    console.log(Math.max.myBind(Math, 34, 5, 3, 6, 54, 6, -67, 5, 7, 6, -8, 687)())
