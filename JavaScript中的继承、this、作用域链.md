[TOC]

# JavaScript中的继承、this、作用域链

## 开始之前
- `JavaScript`使用场景
    - Web前端
    - 服务器
    - PC跨平台应用
    - 移动端：`React Native` `Weex` `微信小程序`    
> Jeff Atwood: Any application that can be written in JavaScript, will eventually be written in JavaScript.
- `JavaScript`的世界一切都是对象,函数也是对象!对象是**一等公民**！
- `JavaScript`是**动态**语言：可以方便的在运行时给对象添加或删除属性、方法
> 动态性比较：JavaScript > Objective-C > Java > Swit > C++
- `JavaScript`**多范式**语言
    - 过程式
    - 面向对象
    - 函数式
    - 响应式
    - ....
- `JavaScript`调试
    - 浏览器环境下运行：Chrome浏览器开发者工具
    - Node环境运行：https://nodejs.org/zh-cn/

- `Hello world`
```js
    var jsObject = {name:'Andrew'}
    
    console.log(jsObject) //{name: "Andrew"}
    console.log(jsObject.name) //Andrew
    console.log(typeof jsObject) //object
    console.log(jsObject.age) //undefined
    jsObject.age = 30
    console.log(jsObject.age) //30

    function foo(){
        return 'I am foo.'
    }
    
    jsObject.speak = foo
    jsObject.speak() //"I am foo."
    delete jsObject.speak
    console.log(jsObject.speak) //undefined
    
    console.log(typeof foo) //function
    foo.customTag = 'HomeCredit';
    console.log(foo.customTag) //HomeCredit
    
    function foo2(){ 
        return 2018
    }
    foo.customFunc = foo2
    foo.customFunc() //2018
```

## 继承
- `JavaScript`基于原型继承的语言:  
    - `prototype`(**原型**):只有函数对象拥有此字段，创建一个函数就有一个对应的函数原型对象，`prototype`指向函数原型对象
    - `__proto__`(**原型链**):任何对象都拥有`__proto__`这个字段，用来在运行时查找调用的`属性`、`方法`,`__proto__`也指向某个对象，以此构成查找链。
    - 通过函数创建的对象的`__proto__`字段，自动指向函数的原型对象    
    ```js
    function Foo(){};
    var foo = new Foo(); //只有函数才能使用new关键字
    foo.__proto__ == Foo.prototype
    ```
    > tips:`JavaScript`的世界一切都是对象   
- 代码
```js
function Base(){
    
}; //声明Base类，同时也是构造函数constructor，
console.log(Base) //ƒ Base(){}

console.log(Base.prototype)
/*
Base.prototype
    constructor:ƒ Base()
    __proto__:Object
*/
console.log(Base.__proto__) //ƒ () { [native code] }, Function.prototype
Base.__proto__ == Function.prototype //true

var base = new Base();
base.constructor //ƒ Base(){}
base.constructor == Base //true
base.__proto__ == Base.prototype //true

Object.prototype
/*
constructor:ƒ Object()
hasOwnProperty:ƒ hasOwnProperty()
isPrototypeOf:ƒ isPrototypeOf()
propertyIsEnumerable:ƒ propertyIsEnumerable()
toLocaleString:ƒ toLocaleString()
toString:ƒ toString()
valueOf:ƒ valueOf()
__defineGetter__:ƒ __defineGetter__()
__defineSetter__:ƒ __defineSetter__()
__lookupGetter__:ƒ __lookupGetter__()
__lookupSetter__:ƒ __lookupSetter__()
get __proto__:ƒ __proto__()
set __proto__:ƒ __proto__()
*/

Function.prototype // ƒ () { [native code] }
Object.__proto__ == Function.prototype //true
Function.prototype.__proto__ == Object.prototype //true
Base.__proto__ == Function.prototype //true

base.toString(); //"[object Object]" 

Base.prototype = {
    method1:function(){
        return "Base method1";
    },
    
    method2:function(){
        return "Base method2";
    }
};
base.method1(); //undefined
base.method2(); //undefined

var base_new = new Base();
base_new.method1(); //"Base method1"
base_new.method2(); //"Base method2"

function Derived(){}
Derived.prototype = {
    method1:function(){return "Derived method1";},
    method3:function(){return "Derived method3";}
}
Derived.prototype.__proto__ = Base.prototype;
var derived = new Derived();
derived.method1(); //"Derived method1"
derived.method2(); //"Base method2" 
derived.method3(); //"Derived method3"
```
- 图例  

![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fsqocmi49pj30gz0l4q4h.jpg)

- `ES6`
    - 新一代的js版本
    - 添加了各种语法和语法糖
    - [文档](http://es6.ruanyifeng.com/#docs/class-extends)：阮一峰教程
    - [Babel](https://babeljs.io/en/repl): ES6代码转ES5
```js
class Base {
  foo(){
    return "i am base"
  }
}

class Derived extends Base {
  foo(){
    return "i am Derived"
  }
}
```
转码后
```js
"use strict";

var _createClass = function () { function defineProperties(target, props) { for (var i = 0; i < props.length; i++) { var descriptor = props[i]; descriptor.enumerable = descriptor.enumerable || false; descriptor.configurable = true; if ("value" in descriptor) descriptor.writable = true; Object.defineProperty(target, descriptor.key, descriptor); } } return function (Constructor, protoProps, staticProps) { if (protoProps) defineProperties(Constructor.prototype, protoProps); if (staticProps) defineProperties(Constructor, staticProps); return Constructor; }; }();

function _possibleConstructorReturn(self, call) { if (!self) { throw new ReferenceError("this hasn't been initialised - super() hasn't been called"); } return call && (typeof call === "object" || typeof call === "function") ? call : self; }

function _inherits(subClass, superClass) { if (typeof superClass !== "function" && superClass !== null) { throw new TypeError("Super expression must either be null or a function, not " + typeof superClass); } subClass.prototype = Object.create(superClass && superClass.prototype, { constructor: { value: subClass, enumerable: false, writable: true, configurable: true } }); if (superClass) Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass; }

function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

var Base = function () {
  function Base() {
    _classCallCheck(this, Base);
  }

  _createClass(Base, [{
    key: "foo",
    value: function foo() {
      return "i am base";
    }
  }]);

  return Base;
}();

var Derived = function (_Base) {
  _inherits(Derived, _Base);

  function Derived() {
    _classCallCheck(this, Derived);

    return _possibleConstructorReturn(this, (Derived.__proto__ || Object.getPrototypeOf(Derived)).apply(this, arguments));
  }

  _createClass(Derived, [{
    key: "foo",
    value: function foo() {
      return "i am Derived";
    }
  }]);

  return Derived;
}(Base);
```
## this
TODO
## 作用域链
TODO