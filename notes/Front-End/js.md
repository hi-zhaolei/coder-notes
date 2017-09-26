# JavaScript

## prototype



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
