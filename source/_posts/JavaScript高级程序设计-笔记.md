---
title: JavaScript高级程序设计-笔记
date: 2018-12-03 16:03:39
tags: 读书笔记
categories: 读书笔记
---
* JavaScript:ECMAScript、DOM、BOM  
* script标签:async、defer属性  
* 语法:区分大小写、标识符、严格模式
* 数据类型:Undefined、Null、Boolean、Number、String、Object  
* typeof:确定一个值是哪种基本类型
* null:值表示一个空对象指针，typeof null === 'object'  
* undefined:值是派生自null值的，null == undefined  

<!-- more -->

* 0.1+0.2=0.30000000000000004    
* NaN:非数值，是一个特殊的数值，NaN == NaN false，isNaN   
* Constructor:保存着用于创建当前对象的函数
* hasOwnProperty:检查给定属性在当前对象实例中是否存在，而不是在实例原型中   
* for-in   
* 基本类型、引用类型  
* instanceof:确定一个值是哪种引用类型
* person instanceof Object  
* 没有块级作用域  
* 垃圾自动回收  
* Array  
  push:返回修改后数组的长度，pop返回移除的项  
  concat:创建当前数组一个副本，然后将接收到的参数添加到这个副本末尾，最后返回新构建的数组  
  slice:返回项的起始和结束为止，两个参数时，返回所有项不包括结束位置的项，不影响原始数组  
  splice  
  indexOf:返回要查找的项在数组中的位置，未找到为-1  
  filter:利用指定函数返回符合条件的所有项  
  map有返回值，forEach没有  
  reduce: 返回的任何值都会作为第一个参数自动传给下一项  
```js
var values = [1,2,3,4,5]
var sum = values.reduce(function(prev, cur, index, array) {
  return prev + cur  // 15
})
```
* RegExp:/pattern/flags  
  flags:  
  /g-全局模式，非在发现第一个匹配项时就立即停止  
  /i-不区分大小写  
  /m-多行  
* 函数是对象，函数名是指针  
* 函数声明和函数表达式  
```js
// 一个对象数组，按照某个对象属性对数组排序
function test(propertyName) {
  return function(object1, object2) {
    var value1 = object1[propertyName]
    var value2 = object2[propertyName]

    if (value1 < value2) {
      return -1
    } else if (value1 > value2) {
      return 1
    } else {
      return 0
    }
  }
}

var data = [{name: 'zhang', age: 28}, {name: 'chen', age: 29}]

data.sort(test('name'))
console.log(data[0].name)  // zhang
```
* arguments
* this
* prototype
* apply、call:在特定的作用域中调用函数，等同于设置函数体内this对象的值  
  call:必须明确的传入每一个参数  
  bind:创建一个函数的实例，this值会被绑定到传给bind函数的值
```js
function sum(num1, num2) {
  return num1 + num2
}
function callSum1(num1, num2) {
  return sum.apply(this, arguments)
  return sum.apply(this, [num1, num2])
  return sum.call(this, num1, num2)
}
callSum1(10, 10)  // 20
```

* 扩充函数赖以运行的作用域，对象不需要与方法有任何耦合关系
```js
window.color = 'red'
var o = {color: 'blue'}
function sayColor() {
  console.log(this.color)
}
sayColor.call(this) // red
sayColor.call(o) // blue
sayColor.bind(o)  // blue
```
* toFixed:按照指定小数位返回数值的字符串  
* String:charAt、slice、substring、trim、replace、split   
* Math:ceil、floor、round、random  
* Math.floor(Math.random() * 可能值的总数 + 第一个可能的值)
```js
// 随机1到10之间的数值
Math.floor(Math.random() * 10 + 1)
```
* 数据属性:默认是指直接在对象上定义属性
configurable:delete 默认true  
enumerable:for-in 默认true  
writable:默认true  
value  
Object.defineProperty:修改对象属性默认的特性  
* 访问器属性
```js
var person = {
  _year: 2018,
  age: 18
}
// 数据属性
Object.defineProperty(person, 'name', {
  writable: false,
  value: 'chen'
})
// 访问器属性
Object.defineProperty(person, 'year', {
  get: function() {
    return this._year
  },
  set: function(newValue) {
    if (newValue > 2018) {
      this._year = newValue
      this.age += newValue + 1
    }
  }
})
person.name  //chen
person.name = 'fa'
person.name //chen
person.year = 2019
person.age // 19
```
* 设计模式:js面向对象，创建对象
```js
// 工厂模式:使用简单的函数创建对象，为对象添加属性和方法，然后返回对象。抽象了创建具体对象的过程，但不知道创建对象的类型
function createPerson(name, age, job) {
  var o = new Object()
  o.name = name
  o.age = age
  o.job = job
  o.sayName = function() {
    console.log(this.name)
  }
  return o
}

var person = createPerson('chen', 18, 'hahaha')

// 构造函数模式:每个方法都要在每个实例上重新创建一遍
function Person(name, age, job) {
  this.name = name
  this.age = age
  this.job = job
  this.sayName = function() {
    console.log(this.name)
  }
}

var person1 = new Person('chen', 18, 'hahaha')
var person2 = new Person('fa', 18, 'hahaha')
person.constructor == Person // true
person instanceof Object // true
person instanceof Person // true
person1.sayName === person2.sayName // false
```
* prototype:指向一个对象，包含所有实例共享的属性和方法，多个对象实例共享原型所保存的属性和方法
```js
// 原型模式:使用构造函数的prototype属性来指定那些应该共享的属性和方法，引用类型共享问题
function Person() {}
Person.prototype.name = 'chen'
Person.prototype.age = 18
Person.prototype.sayName = function() {
  console.log(this.name)
}

var person1 = new Person()
var person2 = new Person()
person.sayName() // chen
person1.sayName === person2.sayName // true

// 简洁的原型模式
Person.prototype = {
  name: 'chen',
  age: 18,
  sayName: function() {
    console.log(this.name)
  }
}
person.constructor == Person // false
person.constructor == Object // true

Person.prototype = {
  // 方法一
  constructor: Person,  // enumerable: true
}

// 方法二
Object.defineProperty(Person.prototype, 'constructor', {
  enumerable: false,
  value: Person
})
person.constructor == Person // true
```
* delete:删除实例属性
* hasOwnProperty:检测一个属性是存在实例中，还是原型中，对象实例中为true
* 实例中的指针仅指向原型，而不指向构造函数
```js
// 构造函数模式和原型模式混合:构造函数定义示例属性，原型定义共享属性和方法
function Person(name, age) {
  // 实例属性
  this.name = name
  this.age = age
  this.friends = ['li', 'wang']
}
Person.prototype = {
  // 定义方法和共享的属性
  constructor: Person,
  sayName: function() {
    console.log(this.name)
  }
}
var person1 = new Person('chen', 18)
var person2 = new Person('fa', 18)

person1.friends.push('zhang')
person1.friends // li, wang, zhang
person2.friends // li, wang
person1.friends == person2.friends // false
person1.sayName == person2.sayName // true
```
* 动态原型模式、寄生构造函数模式、稳妥构造函数模式
* 原型链:通过一个类型的实例赋值给另一个构造函数的原型实现继承
```js
// 重写原型对象实现继承
function SuperType() {
  this.property = true
}
SuperType.prototype.getSuperValue = function() {
  return this.property
}
function SubType() {
  this.subproperty = false
}
SubType.prototype = new SuperType()

var instance = new SubType()
instance.getSuperTypeValue() // true
```
* 组合继承
```js
function SuperType(name) {
  this.name = name
  this.colors = ['red', 'blue']
}
SuperType.prototype.sayNmae = function() {
  console.log(this.name)
}
function SubType(name, age) {
  // 继承属性
  SuperType.call(this, name)
  this.age = age
}
// 继承方法
SubType.prototype = new SuperType()

SubType.prototype.sayAage = function() {
  console.log(this.age)
}
var instance1 = new SubType('chen', 18)
instance1.colors.push('black')
instance1.colors // red,blue,black
instance1.sayName() // chen
instance1.sayAage() // 18

var instance2 = new SubType('haha', 20)
instance2.colors // red,blue
instance2.sayName() // haha
instance2.sayAage() // 20 
```
* 递归
```js
function factorial(num) {
  if (num <= 1) {
    return 1
  } else {
    return num * factorial(num - 1)
  }
}
```
* 闭包  
  函数内部定义了其它函数，闭包有权访问包含函数内部的所有变量
  作用域链  
  函数的作用域及其所有变量都会在函数执行结束后被销毁
  闭包的作用域链包含自己的作用域，包含函数的作用域和全局作用域
  闭包只能取得包含函数中任何变量的最后一个值
  当函数返回一个闭包时，这个函数的作用域将会一直在内存中保存到闭包不存在为止
```js
function demo() {
  var result = []

  for (var i = 0;i < 10;i++) {
    result[i] = function() {
      return i
    }
  }

  return result
}

function demo() {
  var result = []

  for (var i = 0;i < 10;i++) {
    result[i] = function(num) {
      return function() {
        return num
      }
    }
  }

  return result
}
```
* 模仿块级作用域:立即执行的匿名函数，创建私有作用域，避免全局污染
```js
(function() {
  // 这里是块级作用域

})()
```
* 单例:只有一个实例的对象，以对象字面量方式来创建
```js
var singleton = {
  name: value,
  method: function() {

  }
}
```
* 模式模块:为单例创建私有变量和特权方法
```js
// 如果创建一个对象并以某些数据对其进行初始化，同时还要公开一些能够访问这些私有数据的方法
// 以这种模式创建的每个单例都是object的实例
var application = function() {
  // 私有变量和函数
  var components = new Array()
  // 初始化
  components.push(new BaseComponent())
  // 公共
  return {
    getComponentCount: function() {
      return components.length
    },
    registerComponent: function(component) {
      if (typeof component === 'object') {
        components.push(component)
      }
    }
  }
}()

// 增强的模式模块
var application = function() {
  // 私有变量和函数
  var components = new Array()
  components.push(new BaseComponent())

  var app = new BaseComponent()

  app.getComponentCount = function() {
    return components.length
  }
  app.registerComponent = function(component) {
    if (typeof component === 'object') {
      components.push(component)
    }
  }
  return app
}()
```
* BOM:window、location、navigator、history
* DOM
```js
function loadScript(url) {
  var script = document.createElement('script')
  script.type = 'text/javascript'
  script.src = url
  document.body.appendChild(script)
}
```
* 事件冒泡:逐级向上
* 事件捕获:逐级向下（特殊时使用）
* addEventListener、removeEventListener
```js
var btn = document.getElementById('myBtn')
btn.onclick = function(event) {
  console.log(event)
}
// 在冒泡阶段调用事件
btn.addEventListener('click', function(event) {
  console.log(event)
}, false)
```
* event.preventDefault():阻止特定事件的默认行为
* event.stopPropageation():取消事件的捕获或冒泡
```js
var btn = document.getElementById('myBtn')
btn.onclick = function(event) {
  console.log(event)
  event.stopPropageation()
}
// 不会触发
document.body.onclick = function(event) {
  console.log(event)
}
```
* load事件:当页面完全加载后（包括图像，js，css等外部资源），就会触发window上面的load事件
* canvas:context 2D
* webGL 3D
* postMessage
* video audio
* 任何没有通过try-catch处理的错误都会触发window对象的error事件
* 序列化:JSON.stringify  
  反序列化:JSON.parse
* XMLHttpRequest
* 200、304
* Referer:发出请求的页面URI
* GET、POST
* FormData:序列化表单以及创建与表单格式相同的数据
* 超时:timeout
* CORS:跨域  
  Origin(request)/Access-Control-Allow-Origin(response):请求页面的原信息（协议、域名和端口）  
  Access-Control-Max-Age:缓存多长时间（秒）
  withCredentials/Access-Control-Allow-Credentials:跨源请求提供凭据（cookie、HTTP认证及客户端SSL证明等，默认不提供）
* JSONP:回调函数和数据，不安全
```js
// 被包含在函数调用中的JSON
callback({"name", "chen"})

function handleResponse(response) {
  console.log(response)
}

var script = document.createElement('script')
script.src = 'http://xxxx?callback=handleResponse'

document.body.insertBefore(script, docement.body.fristChild)
```
* Web Sockets
* 函数绑定
```js
function bind(fn ,context) {
  return function() {
    return fn.apply(context, arguments)
  }
}

var handler = {
  message: 'event handled',
  handleClick: function(event) {
    console.log(this.message)
  }
}
btn.bind(handler.handleClick, handler)
```
* 函数柯里化
* cookie
* sessionStorage:浏览器关闭
* localStorage:主动删除或者用户清楚浏览器缓存
* IndexedDB
* 命名空间
```js
var Application = {}

Application.Util = {}

Application.Util.Event = {}
Application.Util.Dom = {}
```
* requestAnimationFrame