# JavaScript

## 数据类型

Javascript提供五种简单的数据类型，与一种较为复杂的数据类型。

### undefined

在使用var声明变量，但未对其加以初始化时，这个变量的类型就是undefined，且其默认初始化值为undefined。

对未声明也未初始化的变量，直接使用，那么这个变量的类型也是undefined，但是没有默认初始化值。

### Null

null类型的默认值是null，表示一个空对象指针，类型为object。

与undefined不同: undefined类型，被用来形容未经初始化的变量，null类型被用来形容空对象指针。

### Number

两种表示形式，整数和浮点数

#### 整数

可以通过十进制，八进制，十六进制的字面值来表示

```js
var int = 12; //十进制
var octal = 056; //八进制数，第一位必须是0，后跟8进制数字（0~8）
var hex = 0xFA; //十六进制数，前两位必须是0x，后跟16进制数字（0~9及A~F）
```

#### 浮点数

必须包含一个小数点，且小数点后必须有一位数字。
浮点数所占据的内存空间是整数的两倍。
多位的浮点数可以采用e表示法

```js
var float = 1e7; // 10000000
var float = 1e-7; // 0.0000001
```

#### NaN(Not a Number)

用于表示一个本来要返回数值的操作数，未返回数值的情况。

任何涉及NaN的操作都会返回NaN，NaN值与任何值都不相等，包括本身。

isNaN()函数判断参数是否是NaN类型

### String

在js中字符串用单引号和双引号没有差别的。

### Boolean

布尔类型，该类型有两个值：true false

### Symbol(es5)

[详见ES6](./es6.md)

#### 获取方法

调用Symbol()，每次调用都会返回一个新的唯一symbol。

调用Symbol.for(string)。这种方式会访问symbol注册表，其中存储了已经存在的一系列symbol，注册表中的symbol是共享的，多次调用都会返回相同的symbol。

使用标准定义的symbol，例如：Symbol.iterator。标准根据一些特殊用途定义了少许的几个symbol。

### Object

object类型是最基本的类型，我们可以在其基础上继承出更多的类型(eg: Array、Date、Function)

实例化对象的过程有两种，通过new操作符或通过对象字面量表示法。

实例化结果又称做引用类型，引用类型是堆内存中一个对象的别称，本身并不占用内存。

## 作用域

作用域是指程序源代码中定义变量的区域，它规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。

### 静态作用域与动态作用域

静态作用域是指函数的作用域在函数定义的时候就决定了。

动态作用域是指函数的作用域是在函数调用的时候才决定的。

JavaScript 采用词法作用域(lexical scoping)，也叫做静态作用域。

JavaScript 函数的执行用到了作用域链，这个作用域链是在函数定义的时候创建的。

## 执行上下文

JavaScript 引擎并非一行一行地分析和执行程序，而是一段一段地分析执行。当执行一段代码的时候，会进行一个“准备工作”。

JavaScript 的可执行代码(executable code)的类型有三种，全局代码、函数代码、eval代码。

为了管理创建的那么多执行上下文，JavaScript 引擎创建了执行上下文栈（Execution context stack，ECS）

当 JavaScript 开始要解释执行代码的时候，最先遇到的就是全局代码，所以初始化的时候首先就会向执行上下文栈压入一个全局执行上下文，并且只有当整个应用程序结束的时候才会被清空。

当执行一个函数的时候，就会创建一个执行上下文，并且压入执行上下文栈，当函数执行完毕的时候，就会将函数的执行上下文从栈中弹出。

每个执行上下文，都有三个重要属性：变量对象(Variable object，VO)、作用域链(Scope chain)、this

### 变量对象(Variable object，VO)

变量对象是与执行上下文相关的数据作用域，存储了在上下文中定义的变量和函数声明。

变量对象是规范上的或者说是引擎实现上的，不可在 JavaScript 环境中访问，只有到当进入一个执行上下文中，这个执行上下文的变量对象才会被激活

#### 全局上下文

全局上下文中的变量对象就是全局对象

1.可以通过 this 引用，在客户端 JavaScript 中，全局对象就是 Window 对象。

2.全局对象是由 Object 构造函数实例化的一个对象。

3.预定义一些函数和属性。

4.作为全局变量的宿主。

5.客户端 JavaScript 中，全局对象有 window 属性指向自身。

#### 函数上下文

##### 进入执行上下文

当进入执行上下文时，这时候还没有执行代码，引擎会先准备下面三种变量对象

1.函数形参

创建一个由名称和对应值组成的变量对象属性，没有实参的，属性值设为 undefined

2.函数声明

创建一个由名称和对应值（函数对象(function-object)）组成的变量对象属性，如果变量对象已经存在相同名称的属性，则完全替换这个属性

3.变量声明

创建一个由名称和对应值（undefined）组成的一个变量对象属性，如果变量名称跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性

##### 代码执行

在代码执行阶段，会顺序执行代码，根据代码，修改变量对象的值

### 作用域链

作用域链创建需要两个步骤

#### 函数创建

函数有一个内部属性 [[scope]]，当函数创建的时候，就会保存所有父变量对象到其中

ps:可以认为 [[scope]] 是所有父变量对象的层级链，但是并不代表完整的作用域链！

```js
fucntion.[[scope]] = [
    functionContext.AO,
    globalContext.VO
];
```

#### 函数激活

当函数激活时，进入函数上下文，创建 VO/AO 后，就会将活动对象添加到作用链的前端。

```js
scope= [
  AO,
  ...[[scope]]
];
```

## 原型(prototype)

## URL编码

网络标准[RFC 1738](http://www.ietf.org/rfc/rfc1738.txt)规定:

> "...Only alphanumerics [0-9a-zA-Z], the special characters "$-_.+!*'()," [not including the quotes - ed], and reserved characters used for their reserved purposes may be used unencoded within a URL."
> "只有字母和数字[0-9a-zA-Z]、一些特殊符号"$-_.+!*'(),"[不包括双引号]、以及某些保留字，才可以不经过编码直接用于URL。"

javascript语言用于编码的函数，一共有三个

### escape

返回一个字符的Unicode

除了ASCII字母、数字、标点符号"@ * _ + - . /"以外，对其他所有字符进行编码。在\u0000到\u00ff之间的符号被转成%xx的形式，其余符号被转成%uxxxx的形式。对应的解码函数是unescape()。

已不提倡使用，并不能直接用于URL编码

无论网页的原始编码是什么，一旦被Javascript编码，就都变为unicode字符。也就是说，Javascipt函数的输入和输出，默认都是Unicode字符。

escape()不对"+"编码。但是我们知道，网页在提交表单的时候，如果有空格，则会被转化为+字符。服务器处理数据的时候，会把+号处理成空格。所以，使用的时候要小心。

### encodeURI

Javascript中真正用来对URL编码的函数。对应的解码函数是decodeURI()。

除了常见的符号以外，对其他一些在网址中有特殊含义的符号"; / ? : @ & = + $ , #"，也不进行编码。编码后，它输出符号的utf-8形式，并且在每个字节前加上%。不对单引号'编码

## encodeURIComponent

与encodeURI()的区别是，它用于对URL的组成部分进行个别编码，而不用于对整个URL进行编码。"; / ? : @ & = + $ , #"，这些在encodeURI()中不被编码的符号，在encodeURIComponent()中统统会被编码。

解码函数是decodeURIComponent()。

## 浮点数

Javascript采用了IEEE-745浮点数表示法
这是一种二进制表示法，可以精确地表示分数1/2, 1/8
我们常用的分数都是十进制分数1/10，1/100等，二进制浮点数表示法并不能精确的表示类似这样的简单的数字

## 典型错误

### Chrome中的错误

`Uncaught TypeError: undefined is not a function`

错误的结构如下：

1.`Uncaught TypeError：`这部分信息通常不是很有用。`Uncaught`表示错误没有被`catch`语句捕获，`TypeError`是错误的名字。
2.`undefined is not a function:`这部分信息，你必须逐字阅读。比如这里表示代码尝试使用`undefined`，把它当做一个函数。

其它基于 webkit 的浏览器，比如 Safari ，给出的错误格式跟 Chrome 很类似。Firefox 也类似，但是不总包含第一部分，最新版本的 IE 也给出比 Chrome 简单的错误 - 但是在这里，简单并不总代表好。

###`Uncaught TypeError: undefined is not a function`

相关错误：

`number is not a function, object is not a function, string is not a function, Unhandled Error: ‘foo’ is not a function, Function Expected`

当尝试调用一个像方法的值时，这个值并不是一个方法。比如：
```
var foo = undefined;
foo();
```

如果你尝试调用一个对象的方法时，你输错了名字，这个典型的错误很容易发生。

```
var x = document.getElementByID('foo');
```

由于对象的属性不存在，默认是`undefined`，以上代码将导致这个错误。尝试调用一个像方法的数字，“number is not a function” 错误出现。

**如何修复错误：**确保方法名正确。这个错误的行号将指出正确的位置。


### `Uncaught ReferenceError: Invalid left-hand side in assignment`

相关错误：

`Uncaught exception: ReferenceError: Cannot assign to ‘functionCall()’, Uncaught exception: ReferenceError: Cannot assign to ‘this’`

尝试给不能赋值的东西赋值，引起这个错误。

这个错误最常见的例子出现在 if 语句使用：

`if(doSomething() = 'somevalue')`

此例中，程序员意外地使用了单个等号，而不是双等号。“left-hand side in assignment” 提及了等号左手边的部分，因此你可以看到以上例子，左手边包含不能赋值的东西，导致这个错误。

**如何修复错误：**确保没有给函数结果赋值，或者给 this 关键字赋值。


### `Uncaught TypeError: Converting circular structure to JSON`

相关错误：

`Uncaught exception: TypeError: JSON.stringify: Not an acyclic Object, TypeError: cyclic object value, Circular reference in value argument not supported`

把循环引用的对象，传给`JSON.stringify`总会引起错误。

```
var a = { };
var b = { a: a };
a.b = b;
JSON.stringify(a);
```

由于以上的`a`和`b`循环引用彼此，结果对象无法转换成`JSON`。

**如何修复错误：** 移除任何想转换成`JSON`的对象中的循环引用。


### `Unexpected token ;`

相关错误：

`Expected ), missing ) after argument list`

JavaScript 解释器预期的东西没有被包含。不匹配的圆括号或方括号通常引起这个错误，错误信息可能有所不同 -`“Unexpected token ]”`或者`“Expected {”`等。

**如何修复错误：** 有时错误出现的行号并不准确，因此很难修复。

* `[ ]` `{ }` `( )` 这几个符号不配对常常导致出错。检查所有的圆括号和方括号是否配对。行号指出的不仅是问题字符。
* `Unexpected / `跟正则表达式有关。此时行号通常是正确的。
* `Unexpected ; `对象或者数组字面量里面有个；通常引起这个错误，或者函数调用的参数列表里有个分号。此时的行号通常也是正确的。

### `Uncaught SyntaxError: Unexpected token ILLEGAL`

相关错误：

`Unterminated String Literal, Invalid Line Terminator`

一个字符串字面量少了结尾的引号。

**如何修复错误：** 确保所有的字符串都有结束的引号。


### `Uncaught TypeError: Cannot read property ‘foo’ of null, Uncaught TypeError: Cannot read property ‘foo’ of undefined`

相关错误：

`TypeError: someVal is null, Unable to get property ‘foo’ of undefined or null reference`

尝试读取`null`或者`undefined`，把它当成了对象。例如：

```
var someVal = null;
console.log(someVal.foo);
```

**如何修复错误：** 通常由于拼写错误导致。检查错误指出的行号附近使用的变量名是否正确。


### `Uncaught TypeError: Cannot set property ‘foo’ of null, Uncaught TypeError: Cannot set property ‘foo’ of undefined`

相关错误：

`TypeError: someVal is undefined, Unable to set property ‘foo’ of undefined or null reference`

尝试写入`null`或者`undefined`，把它当成了一个对象。例如：

```
var someVal = null;
someVal.foo = 1;
```
**如何修复错误：** 也是由于拼写错误所致。检查错误指出的行号附近的变量名。


### `Uncaught RangeError: Maximum call stack size exceeded`

相关错误：

`Related errors: Uncaught exception: RangeError: Maximum recursion depth exceeded, too much recursion, Stack overflow`

通常由程序逻辑 bug 引起，导致函数的无限递归调用。

**如何修复错误：** 检查递归函数中可能导致无限循环 的`bug`。


### `Uncaught URIError: URI malformed`

相关错误：

`URIError: malformed URI sequence`

无效的`decodeURIComponent`调用所致。

**如何修复错误：** 按照错误指出的行号，检查`decodeURIComponent`调用，它是正确的。


### `XMLHttpRequest cannot load [http://some/url/](http://some/url/). No ‘Access-Control-Allow-Origin’ header is present on the requested resource`

相关错误：

`Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at [http://some/url/](http://some/url/)`

错误肯定是使用`XMLHttpRequest`引起的。

**如何修复：**确保请求的`URL`是正确的，它遵循同源策略 。最好的方法是从代码中找到错误信息指出的`URL`。


### `InvalidStateError: An attempt was made to use an object that is not, or is no longer, usable`

相关错误：

`InvalidStateError, DOMException code 11`

代码调用的方法在当前状态无法调用。通常由`XMLHttpRequest`引起，在方法准备完毕之前调用它会引起错误。

```
var xhr = new XMLHttpRequest();
xhr.setRequestHeader('Some-Header', 'val');
```

这时就会出错，因为`setRequestHeader`方法只能在`xhr.open`方法之后调用。

**如何修复：**查看错误指出的行号，确保代码运行的时机正确，或者在它（例如 xhr.open）之前添加了不必要的调用
