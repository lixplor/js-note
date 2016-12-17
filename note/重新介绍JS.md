# 重新介绍JS

* 1995年, Netscape的Brendan Eich创造了LiveScript
* 后由于Java的流行, 为了推广考虑, 改名为Javascript
* 1997年, Netscape将Javascript提交至Ecma International, 版本为ES3
* ES4由于一些分歧未推出便废除
* 2009年ES5发布
* 2015年ES6发布

## 类型

* `Number`数字
    - 不区分整数和浮点数, 所有数字均用浮点数表示
    - 整数值通常被视为32位整形变量, 便于进行位操作
* `String`字符串
* `Boolean`布尔
* `Symbol`符号(ES6)
* `Object`对象
    - `Function`函数
    - `Array`数组
    - `Date`日期
    - `RegExp`正则表达式
* `null`空
* `undefined`未定义
* `Error`错误

### 数字 (Number)

* `0.1 + 0.2 = 0.30000000000000004`
* 运算方法
    - 加, 减, 乘, 除, 模, 余
* `Math`对象可以进行数学函数操作
* 字符串转数字
    - 内置函数`parseInt("123", 10);`
        - 解析整数
        - 可以指定进制
    - 内置函数`parseFloat("123.12")`
        - 解析浮点数
        - 可以指定进制
    - 加号操作符`+"42"`
* 是否为Number的判断
    - 内置函数`isNaN()`: 不是数字
    - 非数字为`NaN`: Not a Number
* 正负无穷
    - `Infinity`: 正无穷
        - `1 / 0`
    - `-Infinity`: 负无穷
        - `-1 / 0`
    - `isFinite()`: 是否为有限数字

```javascript
// Number类型
console.log(0.1 + 0.2); // 0.30000000000000004

// 字符串转数字
console.log(parseInt("123")); // 123
console.log(parseFloat("123.123")); // 123.123
console.log(+ "123"); // 123

// 正负无穷
console.log(1/0); // Infinity
console.log(-1/0); // -Infinity
console.log(isFinite(5)); // true
console.log(isFinite(1/0)); // false
console.log(isFinite(-Infinity)); // false
console.log(isFinite(NaN)); // false
```

### 字符串

* JS中字符串是一串 **Unicode字符** 序列, 使用UTF-16编码
* `length`属性
获取字符串长度
* `charAt(索引)`方法
获取索引位置的字符
* `replace(原字符串, 新字符串)`方法
替换字符串
* `toUpperCase()`方法
全部转为大写字母

```javascript
// length
console.log("hello".length); // 5
```

### 其他类型

* `null`空值
* `undefined`未初始化
* 布尔类型
    - 可以被转换为`false`的值:
        - `false`
        - `0`数字
        - `""`空字符串
        - `NaN`
        - `null`
        - `undefined`
    - 所有其他值都可以转换为`true`
    - 可以使用`Boolean()`显式转换

## 变量

* `var`关键字
* 声明但不赋值, 则该变量值为`undefined`
* 作用域
    - 以前的JS只有函数有作用域, 其余地方没有作用域
    - ES6开始, `let`和`const`关键字允许创建块作用域的变量

## 运算符

* `+`加
* `-`减
* `*`乘
* `/`除
* `%`余
* `+=`
* `-=`
* `*=`
* `/=`
* `%=`
* `=`
* `==`
* `!=`
* `===`
* `!==`
* `? :`三元运算符

## 控制流程

* 条件判断
    - `if...else if...else`
    - `switch case`
* 循环
    - `while`
    - `do...while`
    - `for`
    - `for...in...`遍历数组和对象
    - `for...of...`
    - `forEach()`遍历数组

```javascript
// for in
var a = [1, 2, 3];
for(var i in a) {
    console.log(a[i]);
}

// forEach()
var a = [1, 2, 3];
a.forEach(function(value, index){
    console.log(value);
});
```

## 对象

* `键-值`对
* 创建对象的2种方式
    - `var obj = new Object();`
    - `var obj = {};`
* 属性的调用
    - `obj.property`
    - `obj["property"]`

## 数组

* 数组是一种特殊的对象
    - 只能通过`[]`访问元素
    - 有特殊属性`length`
* 创建数组的2种方式
    - `var a = [1, 2, 3];`
    - `var a = new Array();`
* 常用方法
    - `toString()`
    - `toLocaleString()`
    - `concat()`
    - `join()`
    - `pop()`
    - `push()`
    - `reverse()`
    - `shift()`
    - `slice()`
    - `sort()`
    - `splice()`
    - `unshift()`
* 坑
    - `length`并不是元素的个数, 而是数组的长度, 数组的长度并不是元素的个数

```javascript
// 数组
var arr = new Array();
a[0] = 1;
a[1] = 2;
a[2] = 3;
console.log(a.length);

// 数组长度和元素个数无关
var a = new Array();
a[0] = 0;
a[100] = 100;
console.log(a.length); // 101
```

## 函数

* `return`返回一个值, 并结束函数
    - 如果没有return, 则返回`undefined`
* `arguments`对象是函数的内部对象, 表示参数的数组
    - 所以即使函数定义时没有给定参数, 也可以在调用函数时传入参数, 并通过遍历`arguments`获取到参数值
* 匿名函数可以使用变量接收: `var f = function(){}`

```javascript
// arguments
function noArg(){
    for(var i in arguments){
        console.log(arguments[i]);
    }
}
noArg(1, 2, 3); // 1 2 3
```

## 自定义对象

* JS基于原型, 没有类, 而是使用函数模拟类, 使用`prototype`实现继承
* `this`在函数中指代当前对象
    - 当在对象上使用`.`或`[]`访问属性或方法时, this指向该对象
    - 当没有使用`.`或`[]`调用某个对象时, this将指向全局对象
* 创建对象的方式
    - 函数生成对象
    - 函数作为对象定义, 并使用this
* `类.prototype`
    - 引用外部函数作为对象的方法
    - 实现继承: `子类.prototype = Object.create(父类.prototype);`
    - 可以通过prototype给JS内部函数增加额外属性或方法

```javascript
// 使用函数方式定义对象
function makePerson(name){
    return {
        name:name,
        age:12,
        say:function(word){
            console.log(word);
        }
    }
}
var p = makePerson("Jack");
p.say("hoo"); // hoo

// 使用this定义
function Person(name){
    this.name = name;
    this.age = 12;
    this.say = function(word){
        console.log(name + " says " + word);
    }
}
function Student(subject){
    this.subject = subject;
}
Student.prototype = Object.create(Person.prototype);
var p = new Person("Jack");
p.say("haa"); // Jack says haa
var s = new Student("CS");
console.log(s instanceof Person); // true
s.say("hoho"); // ???todo

// 对象引用外部定义的函数
function eat(){
    console.log("eating");
}
function Person(){
    this.name = "hi";
    this.eat = eat;
}
var p = new Person();
p.eat(); // eating

// 使用prototype引用外部函数作为方法
function Person(){
    this.name = "hi";
}
Person.prototype.eat = function(){
    console.log("eating");
}
var p = new Person();
p.eat(); // eating
```

## 内部函数和闭包

* 函数中可以嵌套函数
* 内部函数可以访问外部函数变量或参数, 而外部不能访问内部
* `闭包`: 一个函数和被创建的函数中的作用域对象的集合
* 闭包的隐患
    - 内存泄漏: JS对象和本地对象之间形成循环引用

```javascript
// 闭包
function makeAdder(a) {
    return function(b) {
        return a + b;
    }
}
var x = makeAdder(5);
var y = makeAdder(20);
console.log(x(6)); // 11, 5 + 6
console.log(y(7)); // 27, 20 + 7

//❌ 闭包导致内存泄漏的错误示例
// IE不会释放el和o使用的内存, 知道浏览器彻底关闭
function addHandler() {
    var el = document.getElementById('el');
    el.onclick = function() {
        el.style.backgroundColor = 'red';
    }
}
// ✅ 解决方法是不要在内部函数中使用外部的引用
function addHandler(){
    document.getElementById('el').onclick = function(){
        this.style.backgroundColor = 'red';
    };
}
// 或者通过另一个闭包解决循环引用
function addHandler() {
    var clickHandler = function() {
        this.style.backgroundColor = 'red';
    };
    (function() {
        var el = document.getElementById('el');
        el.onclick = clickHandler;
    })();
}
```
