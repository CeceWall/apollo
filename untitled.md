# Javascript

## 简介

JavaScript是一个[函数先行](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function)的多范式语言，本文不是讲解JS的文档，仅对JS与其他常见语言的异同作比较

我们常说的JavaScript实际上是JavaScript本身和浏览器提供的DOM API共同组成

下面我们分别说明这两部分

## JavaScript

JavaScript本身是一个很简单且不那么严谨的语言（动态且弱类型）

{% hint style="info" %}
从MDN可以查阅JS内置的[标准库和方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)，需要注意的是，不同浏览器对内置标准库的实现程度不太一致，可以通过MDN文档中的兼容性列表部分查看每个方法的兼容性。
{% endhint %}

#### JS是动态弱类型语言

JS的动态体现在，JS的变量在声明时不需要指定类型。解释器会在赋值时在内部记录变量的类型，并在运行到该行时，动态检查变量的类型。

```javascript
var a = 1;
a.getTime() // 只有运行时才会出错
```

{% hint style="info" %}
看起来像Java的推断类型？他与推断类型最大的区别在于，Java会在编译时检查类型错误。
{% endhint %}

弱类型体现在，JS会隐式的转换一些原本无法操作的数据类型

```javascript
var a = 1;
var b = '2';
console.log(a / b); // 输出number类型的0.5
console.log(a + b); // 输入string类型的'11'
```

因此写JS的时候要额外注意，避免因类型不同导致的错误

#### `var`、`let`、`const`

在JS中，var、let、const都是声明变量的关键字，他们的区别如下

在Java等大多数语言中变量的都是块级作用域（在Java中是由`{}`包括的部分）

但是`var`声明的变量其作用域是执行上下文（可以简单的理解成函数体，即每个函数执行的时候都会创建一个上下文），当创建上下文时，解释器会首先做创建的准备工作（这就是JS中函数提升、变量提升的原因）

```javascript
function foo() {
    // 执行foo函数创建了一个上下文，创建时会声明变量a，变量a在整个上下文中都有效
    console.log(a); // 输出undefined，变量提升，a已经声明但未初始化
    for(var a = 0;a < 3; a+=1){ // 为a赋值的语句
        // ...
    }
    console.log(a); // 输出3，a的作用域是当前执行上下文
}
foo();
```

`const`、`let`的作用域都是块级，且不存在变量提升，区别在于`const`用于声明常量，`let`用于声明变量

```javascript
function foo() {
    // 取消下一句的注释会抛出异常，const和let声明的变量不存在变量提升
    // console.log(a); 
    const a = 'a';
    for(let a = 0;a < 3; a+=1){ // 这里声明了块级变量a， 会覆盖上面的a变量
        console.log(a) // 会分别输出1, 2, 3
    }
    // 取消下面的注释会抛出异常，const声明的是常量，不允许重新赋值
    // a = 'b';
    console.log(a); // 输出'a'
}
foo();
```

#### `==` 和`===`比较运算符

在JS中存在两种比较运算符`==`和`===`

{% hint style="warning" %}
建议永远都**不使用**`==`比较运算符，使用`==`可能会产生难以发现的BUG
{% endhint %}

他们的区别是`===`是严格的比较运算符，只有当比较的两个对象类型相同且值相等时返回的结果才是true

`==`会先进行隐式转换，将一些本来不可以比较的类型变得可以比较

```javascript
1 == '1' // true
1 === '1' // false
1 == true // true
1 === true // false
undefined == null // true
undefined === null // false
NaN == NaN // false
NaN === NaN // false
```

{% hint style="warning" %}
NaN（Not a Number）与任何对象比较都`false`，对NaN的比较要使用Math.isNaN方法

需要注意的是JS中存在一个全局的isNaN方法（对，这就是JS中的糟粕），这个方法比较的结果并不可靠，永远都不要使用这个方法比较NaN。

对于不支持Math.isNaN方法的浏览器，可以使用[polyfill](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN#Polyfill)（为旧浏览器提供它不支持的新功能的代码称为polyfill）解决
{% endhint %}

