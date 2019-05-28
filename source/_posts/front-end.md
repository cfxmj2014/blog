---
title: 前端知识点总结
date: 2016-08-02 02:13:58
tags: 读书笔记
categories: 读书笔记
---

## * javascript *
**闭包（closures)**
> 当某个函数需要调用另一个函数内部变量的时候，你就需要用到闭包。比如最经典的点击每个li显示其index。阮大神说，闭包最简单的理解就是：定义在一个函数内部的函数（[戳这里](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)）。

<!-- more -->

```js
// ＝>3
for(var i = 0;i < 3;i++) {
  $li[i].onclick = function() {
    console.log(i);
  }
}
console.log(i);// i＝>3
// ＝>1,2,3
for(var i = 0;i < 3;i++) {
  $li[i].onclick = (function(x) {
    return function() {
      console.log(x);
    }
  })(i)
}
```

**call和apply**
> call 和 apply 都是为了改变某个函数运行时的 context 即上下文而存在的，即为了改变函数体内部 this 的指向。call 需要把参数按顺序传递进去，而 apply 则是把参数放在数组里。当你的参数是明确知道数量时，用 call，而不确定的时候，用 apply，然后把参数 push 进数组传递进去。

```js
obj.myFunc();
myFunc.call(obj,arg1,arg2);
myFunc.apply(obj,[arg1,arg2..]);

function add(a, b){console.log(this);}
function sub(a, b){console.log(this);}
add(1,2); // Window
sub(1,2); // Window
add.call(sub, 1, 2); // sub(a, b)
sub.apply(add, [1, 2]); // add(a, b)
```

**对象（object）**
> js里面所有东西都是对象。把new命令引入了Javascript，用来从原型对象生成一个实例对象。更好的理解对象可以参考阮大神的[这篇](http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html)。


```js
// 1，Dog是一个构造函数，表示狗对象的原型。
// 2，prototype属性指向Dog的原型对象，即Object对象，实例对象需要共享的属性和方法，都放在这个对象里面，那些不需要共享的属性和方法，就放在构造函数里面。
// 3，构造函数的this关键字表示新创建的实例对象。

function Dog(name){
  this.name = name;
}
var dogA = new Dog('大毛');
```

**原型和原型链**
> 1，每个对象有个prototype属性，指向自己的原型对象，原型对象有个constructor属性指向它的构造函数。

**js链式作用域**
> 1，js中当遇到对变量名或者函数名的使用时，会首先在当前作用域查找变量或者函数，如果没有找到，就会到其上层作用域中寻找，并以此类推。
> 2，js中上层对象中的变量和函数对其子对象都是可见的。

```js
var x = 1;
function test() {
  console.log(x);
}
test(); // ＝>1

// 函数test在执行时，会先在其本身的作用域中寻找，而函数本中是定义了x的，就不会在向上层寻找。
var x = 1;
function test() {
  console.log(x);
  var x = 2;
}
test(); // ＝>undefined
```

**继承**

```js
function Animal() {}
Animal.prototype.type = '动物';
function Dog() {
  this.name = name;
}
Dog.prototype = new Animal();
Dog.prototype.constructor = Dog;
```

**判断Javascript对象是否为空**

```js
var a = {};
object.keys(a).length === 0 //true
```

**Array**
> `slice()`：把数组中一部分的浅复制存入一个新的数组对象中，并返回这个新数组，不改变原数组。（包含begin，不包含end）

```js
var a = [1,2,3,4];
var b = a.slice(1,3);
//b＝>[2,3],a＝>[1,2,3,4]
```

> `fill()`：将一个数组中指定区间的所有元素的值, 都替换成或者说填充成为某个固定的值。(具体要填充的元素区间是 [start, end) , 一个半开半闭区间)

```js
var a = [1,2,3,4];
var b = a.fill(8,1,3);
//a,b＝>[1,8,8,4]
```

> `join()`：将数组中的所有元素连接成一个*字符串*。

```js
var a = [1,2,3,4];
var b = a.join(' ');
//b＝>1 2 3 4,a＝>[1,2,3,4]
```

> `concat()`：将传入的数组或非数组值与原数组合并，组成一个新的数组并返回。

```js
var a = ['a','b'];
var b = [3,4];
var c = a.concat(b);
// c=>['a','b',3,4]
```

> `forEach()`：让数组的每一项都执行一次给定的函数。

```js
var a = [1,2,3];
a.forEach((ele,i)=>{console.log(ele)})
// 1,2,3
```

> `indexOf()`：返回给定元素能找在数组中找到的第一个索引值，否则返回-1

```js
var a = [1,2,3];
a.indexOf(2) // 1
a.indexOf(4) // -1
```

**String**
> `split()`：把字符串分割成子字符串*数组*。

```js
var a = 'hello';
var b = a.split(''); // ＝>["h", "e", "l", "l", "o"]
```

**charAt()**

> `charAt()`：返回字符串中指定位置的字符

```js
var a = 'hello world';
a.charAt(3);  // l
```

**indexof()**
> `indexof()`：与数组类似

**match()**
> `match()`：当字符串匹配到正则表达式时，返回一个包含匹配结果的*数组*，如果没有匹配项，则返回 null

```js
var str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";
var regexp = /[A-E]/gi;
var matches_array = str.match(regexp);

console.log(matches_array);
// ['A', 'B', 'C', 'D', 'E', 'a', 'b', 'c', 'd', 'e']
```

**substring()**
> `substring(indexStart,indexEnd)`：返回字符串中从指定位置开始到指定长度的子字符串,[indexStart,indexEnd)

```js
var a = 'abcdefg';
var b = a.substring(1,3);
// a=>'abcdefg' b=>'bc'
```

**slice()**
> `slice()`：类似数组

**js异步编程**
> 1，callback回调函数

```js
fs.readFile('test.txt', (err, data) => {
  if (err) throw err;
  fs.writeFile('message.txt', data, (err) => {
    if (err) throw err;
    console.log('It\'s saved!');
  });
});
```
> 2，Promise对象

> 用来传递异步操作的消息，每一个异步任务返回一个Promise对象，该对象有一个then方法，允许指定回调函数。把你的异步操作用Promise对象包装一下，这样就规范化了异步的处理流程，然后通过 then 和 catch 来分别注册成功回调函数和失败回调函数。

> 对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：Pending（进行中）、Resolved（已完成，又称Fulfilled）和Rejected（已失败）, 只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。

> 状态一旦改变，再改变就不起作用了。Promise对象的状态改变，只有两种可能：从Pending变为Resolved和从Pending变为Rejected。

> Promise 无法取消，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，Promise内部抛出的错误的不会向上传递。

```js
// Promise对象是一个构造函数，用来生成Promise实例。
new Promise((resolve, reject) => {
  console.log("promise start...");

  let age = 10;
  if (age > 18) {
    resolve(age);
  } else {
    reject("this is error");
  }
})
.then(
  (age)=>{console.log(age)}
)
.catch(
  (errMsg)=>{
    setTimeout(()=>{
      console.log(errMsg);
    }, 1000)
  }
)
.then(
  ()=>{console.log('hello-world');}
)
```

> 3，Generator + yield
> 整个 Generator 函数就是一个封装的异步任务，或者说是异步任务的容器。异步操作需要暂停的地方，都用 yield 语句注明。返回一个 Promise 对象

> yield命令是异步两个阶段的分界线，协程遇到 yield 命令就暂停，等到执行权返回，再从暂停的地方继续往后执行。

> 4，async + await
> async 函数返回一个 Promise 对象，可以使用 then 方法添加回调函数。当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再接着执行函数体内后面的语句。


## * es6/7 *
> 1，let声明的变量只在代码块内有效，块级作用域，for循环的计数器就适合let，let声明变量没有变量提升，不允许重复声明。

> 2，::this.fn ＝> this.fn.bind(this)

```js
// 属性方法的简写
var birth = '2000/01/01';
var Person = {
name: '张三',
//等同于birth: birth
birth,
// 等同于hello: function ()...
hello() { console.log('我的名字是', this.name); }
};
```
> 3，Object.assign方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。

```js
var target = { a: 1 };
var source1 = { b: 2 };
var source2 = { c: 3 };
Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

> 4，数据结构Set类似于数组，但是成员的值都是唯一的，没有重复的值。是一组key的集合，但不存储value

```js
var set = new Set([1, 2, 3, 4, 4]);
[...set];
// [1, 2, 3, 4]
```
> 5，Map数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object结构提供了“字符串—值”的对应，Map结构提供了“值—值”的对应。

> 6，对于数组来说，for...in循环读取键名，for...of循环读取键值。for...in循环主要是为遍历对象而设计的，不适用于遍历数组。对于普通的对象，for...of结构不能直接使用，会报错。

```js
var arr = ['a', 'b', 'c', 'd'];
for (let a in arr) {
  console.log(a); // 0 1 2 3
}
for (let a of arr) {
  console.log(a); // a b c d
}
```

> 7，遍历器（Iterator），Array、Map和Set都属于iterable类型。具有iterable类型的集合可以通过新的for ... of循环来遍历。

> 8，箭头函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象, 原因是箭头函数没有自己的 this。

> 9，类（Class），继承

```js

// ES6 定义方法属性时，可以省略function
class Vehicle {
  constructor (name, type) {
    this.name = name;
    this.type = type;
  }
  getName () {
    return this.name;
  }
  getType () {
    return this.type;
  }
}
class Car extends Vehicle {
constructor (name) {
  super(name, 'car');
  this.name = "BMW";
}
getName () {
  return 'It is a car: ' + super.getName();
}
}
let car = new Car('Tesla');
console.log(car.getName()); // It is a car: BMW
console.log(car.getType()); // car
```

> 10，模块

> es6之前，服务器端Node.js 使用CommonJS, 浏览器端主要使用AMD, AMD最流行的实现是RequireJS.

> es6模块是动态引用，遇到模块加载命令import时，不会去执行模块，只是生成一个指向被加载模块的引用，需要开发者自己保证，真正取值的时候能够取到值。

## * react *
> 1，ReactDOM.render 是 React 的最基本方法，用于将模板转为 HTML 语言，并插入指定的 DOM 节点。
> 2，React.createClass 方法就用于生成一个组件类。组件类的第一个字母必须大写，组件类只能包含一个顶层标签。
> 3，组件类的PropTypes属性，就是用来验证组件实例的属性是否符合要求。
> 4，DOM diff：所有的 DOM 变动，都先在虚拟 DOM 上发生，然后再将实际发生变动的部分，反映在真实 DOM上。

## * redux *
**reducer**
> 把一个component想干的事情弄成action，dispatcher会为不同的action调用不用的store中的reducer，store真正管理着component的状态。

## * npm scripts *

```json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "build:css": "npm run scss && npm run autoprefixer",
  "build:js": "npm run lint && npm run uglify",
  "build:images": "npm run imagemin",
  "build:all": "npm run build:css && npm run build:js && npm run build:images",
  /*编译scss为css*/
  "scss": "node-sass --output-style compressed -o dist/css src/style",
  /*使用PostCSS自动给CSS加前缀*/
  "autoprefixer": "postcss -u autoprefixer -r dist/css/*",
  /*JavaScript 代码检查*/
  "lint": "eslint src/js",
  /*混淆压缩JavaScript文件*/
  "uglify": "mkdir -p dist/js && uglifyjs src/js/*.js -m -o dist/js/app.js",
  /*压缩图片*/
  "imagemin": "imagemin src/images dist/images -p",
  /*通过BrowserSync提供服务、自动监测并注入变更，启动一个本地服务，向连接的浏览器自动注入更新的文件，并同步浏览器的点击和滚动*/
  "serve": "browser-sync start --server --files 'dist/css/*.css, dist/js/*.js'",
  /*变更监控*/
  "watch:css": "onchange 'src/style/*.scss' -- npm run build:css",
  "watch:js": "onchange 'src/js/*.js' -- npm run build:js",
}
```

## * JSON *

> JSON.parse() 方法可以将一个 JSON 字符串解析成为一个 JavaScript 值。

```js
// 单引号写在{}外，每个属性名都必须用双引号，否则会抛出异常
var str = '{"name":"chen","age":"25"}';
JSON.parse(str);  // {name: "chen", age: "25"}
```
> JSON.stringify() 方法可以将任意的 JavaScript 值序列化成 JSON 字符串。

```js
var a = {a:1,b:2};
JSON.stringify(a);  // "{"a":1,"b":2}"
```

> eval() 函数可计算某个字符串，并执行其中的的 JavaScript 代码。

```js
eval("x=10;y=20;console.log(x*y)"); // 200
var str = '{"name":"chen","age":"25"}';
JSON.eval('(' + str + ')');  // {name: "chen", age: "25"}
```

## * 选择器优先级 *
> !important > 内联样式 > ID 选择器 > 伪类(:hover li:nth-child) > 属性选择器([type="radio"]) > 类选择器 > 元素(类型)选择器(p) > 通用选择器


## * js 数据类型 *
> 基本类型：String、Number、Boolean
> 引用类型：Object、Array
> 特殊类型：Null、Undefined


## * 事件捕获，事件冒泡，事件委托 *
> 一个元素1中又嵌套了另一个元素2，并且两者都有一个onClick事件处理函数(event handler)，如果用户单击元素2，则元素1和元素2的单击事件都会被触发。
> 当你使用捕获型事件时，元素1的事件处理函数首先被触发，元素2的事件处理函数最后被触发。
> 当你使用冒泡型事件时，元素2 的处理函数首先被触发，元素1其次。
> W3C 模型，首是进入捕获阶段，直到达到目标元素，再进入冒泡阶段。
> addEventListener()：捕获阶段绑定函数:true，在冒泡阶段绑定函数:false
> 在支持w3c dom(文档对象模型) 的浏览器中，默认被视为在绑定于冒泡阶段

```js
// doSomething1 doSomething2
element1.addEventListener('click', doSomething1, true)
element2.addEventListener('click', doSomething2, false)

// doSomething2 doSomething1
element1.addEventListener('click', doSomething1, false)
element2.addEventListener('click', doSomething2, false)
```

> 事件委托
> 当鼠标点击某一li时，alert输出该li的内容。

```html
<ul id="list">
  <li id="item1">item1</li>
  <li id="item2">item2</li>
  <li id="item3">item3</li>
  <li id="item4">item4</li>
</ul>
```

```js
window.onload=function(){
  var ulNode=document.getElementById("list");
  var liNodes=ulNode.childNodes||ulNode.children;
  for(var i=0;i<liNodes.length;i++){
    liNodes[i].addEventListener('click',function(e){
      alert(e.target.innerHTML);
    },false);
  }
}

window.onload=function(){
  var ulNode=document.getElementById("list");
  ulNode.addEventListener('click',function(e){
    if(e.target&&e.target.nodeName.toUpperCase()=="LI"){/*判断目标事件是否为li*/
      alert(e.target.innerHTML);
    }
  },false);
};
```
