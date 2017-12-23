# JavaScript ES6

## let和const

### let

let 是更完美的 var

1.let声明的变量拥有块级作用域。let声明的变量的作用域只是外层块，而不是整个外层函数。

2.let声明的全局变量不是全局对象的属性。

3.形如for (let x...)的循环在每次迭代时都为x创建新的绑定。

4.let声明的变量直到控制流到达该变量被定义的代码行时才会被装载，所以在到达之前使用该变量会触发错误。
(因为let的特性关系，在闭包内，JavaScript引擎将会多做一个运行时检查，所以let相对var而言比较慢)

5.用let重定义变量会抛出一个语法错误（SyntaxError）。

### const

const声明的变量只可以在声明时赋值，不可随意修改，否则会导致SyntaxError（语法错误），用const声明变量后必须要赋值，否则也抛出语法错误。。

## 集合

### Set

Set是一群值的集合。它是可变的，不支持索引，相比 Array 针对包含性检测(indexOf)做了优化。

```js
new Set // 创建一个新的、空的Set。
new Set(iterable) // 从任何可遍历数据中提取元素，构造出一个新的集合。
set.size // 获取集合的大小，即其中元素的个数。
set.has(value) // 判定集合中是否含有指定元素，返回一个布尔值。
set.add(value) // 添加元素。如果与已有重复，则不产生效果。
set.delete(value) // 删除元素。如果并不存在，则不产生效果。.add()和.delete()都会返回集合自身，所以我们可以用链式语法。
set[Symbol.iterator]() // 返回一个新的遍历整个集合的迭代器。一般这个方法不会被直接调用，因为实际上就是它使集合能够被遍历，也就是说，我们可以直接写for (v of set) {...}等等。
set.forEach(f) // 直接用代码来解释好了，它就像是for (let value of set) { f(value, value, set); }的简写，类似于数组的.forEach()方法。
set.clear() // 清空集合。
set.keys()、set.values()和set.entries()返回各种迭代器，是为了兼容Map而提供的。
```

### Map

Map对象由若干键值对组成

```js
new Map // 返回一个新的、空的Map。
new Map(pairs) // 根据所含元素形如[key, value]的数组pairs来创建一个新的Map。
              //这里提供的pairs可以是一个已有的Map 对象，可以是一个由二元数组组成的数组，
              //也可以是逐个生成二元数组的一个生成器，等等。
map.size // 返回Map中项目的个数。
map.has(key) // 测试一个键名是否存在，类似key in obj。
map.get(key) // 返回一个键名对应的值，若键名不存在则返回undefined，类似obj[key]。
map.set(key, value) // 添加一对新的键值对，如果键名已存在就覆盖。
map.delete(key) // 按键名删除一项，类似delete obj[key]。
map.clear() // 清空Map。
map[Symbol.iterator]() // 返回遍历所有项的迭代器，每项用一个键和值组成的二元数组表示。
map.forEach(f) // 类似for (let [key, value] of map) { f(value, key, map); }。
map.keys() // 返回遍历所有键的迭代器。
map.values() // 返回遍历所有值的迭代器。
map.entries() // 返回遍历所有项的迭代器，就像map[Symbol.iterator]()。实际上，它们就是同一个方法，不同名字。
```

## Symbol

Javascript第七种原始类型

symbol 被创建后就不可变更，你不能为它设置属性，但可以用作属性名称。

每一个 symbol 都独一无二，不与其它 symbol 等同，即使二者有相同的描述也不相等。

symbol 不能被自动转换为字符串，这和语言中的其它类型不同。尝试拼接 symbol 与字符串将得到 TypeError 错误。

只有通过 String() 或 sym.toString() 可以显示地将 symbol 转换为一个字符串

## 迭代器和for-of循环

ES6引入了新的for-of循环语法

```js
for (var value of myArray) {
  console.log(value);
}
```

1.这是最简洁、最直接的遍历数组元素的语法
2.这个方法避开了for-in循环的所有缺陷
3.与forEach()不同的是，它可以正确响应break、continue和return语句

for-in循环用来遍历对象属性。
for-of循环用来遍历数组，大多数类数组对象，字符串，Map和Set对象。

事实上，for-of 循环语句是通过迭代器方法调用来遍历的，各种数据集合只有是存在迭代器方法，任意类型的数据结构都可以被 for-of 遍历。

调用普通函数后会立即开始运行，直到遇到return或抛出异常时才退出执行。

调用生成器函数并非立即执行，而是返回一个已暂停的生成器对象。每当调用生成器对象的.next()方法时，函数调用将其自身解冻并一直运行到下一个yield表达式，再次暂停。

## 生成器(Generator)

普通函数使用function声明，而生成器函数使用function*声明。

在生成器函数内部，有一种类似return的语法：关键字yield。
普通函数只可以return一次，而生成器函数可以yield多次。在生成器的执行过程中，遇到yield表达式立即暂停，后续可恢复执行状态。

利用 Generator 可以实现 for-of 循环需要的迭代器。
编写生成器函数遍历这个对象，运行时yield每一个值。然后将这个生成器函数作为这个对象的[Symbol.iterator]方法。

```js
generator.return() // 跳出循环
generator.next(args)  // 可选参数，传入的参数为上一个yield的语句的返回值
generator.throw(error) // 报错
yield* 表达式 // yield*后面的表达式可以通过迭代器进行迭代生成所有的值
```

### generator.next

generator.next工作流程

1.generator暂停在yield操作符

2.发送x给这个yield

3.继续执行到下一个yield，return或者throw

    *yield x 导致 next() 返回 {value: x, done: false}

    *return x 导致 next() 返回 {value:x, done:true}

    *throw err 导致 next() 抛出err

### generator能扮演的角色

#### 迭代器(数据生产者)

每一个yield可以通过next()返回一个值，这意味着generators可以通过循环或递归生产一系列的值，
因为generator对象实现了Iterable接口，generator生产的一系列值可以被ES6中任意支持可迭代对象的结构处理，并且大部分和可迭代对象一起工作的结构会忽略done属性是true的对象的value值

```js
function* genFunc(){
    yield 'a';
    yield 'b';
    return 'c';
}
// for of
for (const x of genFunc()) {
    console.log(x);
}
// output
// a
// b

// 扩展操作符(...)
const arr = [...genFunc()]; 
// output
// ['a', 'b']

// 解构赋值
const [x, y] = genFunc();
// output
// x => 'a', y => 'b'
```

generator.next可以生产三种类型的值

1.对于可迭代序列中的一项x，它返回 {value:x,done:false}

2.对于可迭代序列的最后一项,明确是return返回的z，它返回{value:z,done:true}

3.对于异常，它抛出这个异常

#### 观察者(数据消费者)

yield可以通过next()接受一个值，这意味着generator变成了一个暂停执行的数据消费者直到通过next()给generator传递了一个新值

作为数据的消费者，generator函数返回的对象也实现了接口Observer

```js
interface Observer {
    next(value? : any) : void;
    return(value? : any) : void;
    throw(error) : void;
}
```

generator暂停执行直到它接受到输入值，这有三种类型的输入，通过以下三种observer方法

##### 通过next()发送值

```js
function* dataConsumer() {
    console.log('Started');
    console.log(`1. ${yield}`); // (A)
    console.log(`2. ${yield}`);
    return 'result';
}
// create instace
const genObj = dataConsumer();
// call genObj.next
genObj.next()
// OUTPUT
// Started
// { value: undefined, done: false }
genObj.next('a')
// OUTPUT
// 1. a
//{ value: undefined, done: false }
genObj.next('b')
// OUTPUT
// 2. b
//{ value: 'result', done: true }
```

##### return() 和 throw()

return(x) 在 yield的位置执行 return x

throw(x) 在yield的位置执行throw x

return()终止generator

#### 协作程序(数据生产者和消费者)

考虑到generators是可以暂停的并且可以同时作为数据生产者和消费者，不会做太多的工作就可以把generator转变成协作程序(合作进行的多任务)

## 模版字符串

一种新型的字符串字面量语法，使用反撇号字符 ` 代替普通字符串的引号 ' 或 " 。
它为JavaScript提供了一种简单的、更加美观、更加方便的字符串插值功能。

1.模板占位符中的代码可以是任意JavaScript表达式，甚至是嵌套另一个模版。
2.占位符中的代码如果不是字符串，将会被 toString() 。
3.模版字符串中的 $ 、 { 、 ` 需要 \ 转义才能正确显示。
4.不会自动转义特殊字符，为了避免跨站脚本漏洞，应当对非置信数据进行特殊处理。
5.模板字符串可以多行书写，所有的空格、新行、缩进，都会原样输出在生成的字符串中。

## 不定参数和默认参数值

```js
function (str='abc', ...args) {
  // variable`s default value is 'abc'
  // old time use args = Array.prototype.slice.call(arguments, 1)
  // now just use args immediately
}
```

## 解构赋值(Destructuring)

### 数组解构

允许你使用类似数组或对象字面量的语法将数组和对象的属性赋给各种变量。

```js
// old time
const first = array[0];
const second = array[1];
const third = array[2];
// now!
const [first, second, third] = someArray;
// use grammar
// [keyword] [ variable1, variable2, ..., variableN ] = array;
```

可以对任意深度的嵌套数组进行解构

```js
const [ a, [ b, [c]]] = [1, [2, [3]]];
// a -> 1
// b -> 2
// c -> 3
```

可以在对应位留空来跳过被解构数组中的某些元素

```js
const [,,third] = ["foo", "bar", "baz"];
// third -> baz
```

可以通过“不定参数”模式捕获数组中的所有尾随元素

```js
const [head, ...tail] = [1, 2, 3, 4];
// head -> 1
// tail -> [2,3,4]
```

当访问空数组或越界访问数组时，最终得到的结果都是：undefined。

```js
const [ missing ] = [];
// missing -> undefined
```

数组解构赋值的模式同样适用于任意迭代器

```js
function* iterator () {
  for(let i = 0; i < 5; i++)
    yield i + 1;
}
const [ first, second ] = iterator()
// first -> 1
// second -> 2
```

### 对象解构

通过解构对象可以把它的每个属性与不同的变量绑定，首先指定被绑定的属性，然后紧跟 : 和一个要解构的变量。

```js
const person = { name: "leo" };
const { name: nameA } = person;
// nameA -> 'leo'
```

当属性名与变量名一致时，可以省略变量名

```js
const person = { name: "leo" };
const { name } = person;
// name -> 'leo'
```

可以随意嵌套并进一步组合对象解构

```js
const person = { name: ["leo"] };
const { name: [nameA] } = person;
// nameA -> 'leo'
```

解构一个未定义的属性时，得到的值为undefined

```js
const { missing } = {}
// missing -> undefined
```

解构对象并赋值给变量时，如果你已经声明或不打算声明这些变量，需要将整个表达式用一对小括号包裹阻止报错。

```js
let name;
{ name } = { name: 'leo' };
// Syntax error
({ name } = { name: 'leo' });
// name -> 'leo'
```

无法解构 null 或 undefined ，因为被解构的值需要被强制转换为对象，而 null 和 undefined 是不可以的。

解构的属性未定义时你可以提供一个默认值

```js
const { name = 'leo', name1: nameA = 'zhao' } = {};
// name -> 'leo'
// nameA -> 'zhao'
```

## 箭头函数(=>)

通过箭头函数进行函数声明无需输入function和return，一些小括号、大括号以及分号也可以省略。

新标准中的箭头函数语法：标识符=>表达式。

函数需要多重参数、没有参数、不定参数、默认参数、参数解构时，需要用小括号包裹参数list。

```js
const fn = (a, b = 'leo', { c, d }) => a + b + c + d
const fn = () => 1
```

根据需要，表达式可以替换为用 {} 包裹的块状语句，但块状语句不会有自动返回值。当需要返回普通对象时，为了避免被解析成块状语句，需要用 () 包裹该对象。

箭头函数没有它自己的this值，箭头函数内的this值继承自外围作用域。

## Proxy

全局构造函数，接受两个参数：目标对象（target）与句柄对象（handler）。

### 目标对象（target）

代理的所有内部方法转发至目标对象。

eg: 调用proxy.[[Enumerate]]()，就会返回target.[[Enumerate]]()。

### 句柄对象（handler）

句柄对象的方法可以覆写任意代理的内部方法，句柄方法都是可选的，而没被句柄拦截的内部方法会直接指向目标。

## 类(class)

FIXME

## 模块

## Promise

## 修饰器

修饰器是一个对类和类方法进行处理的函数。只能用于类和类的方法，不能用于函数，因为存在函数提升。

### 类的修饰

许多面向对象的语言都有修饰器（Decorator）函数，用来修改类的行为。

```js
@decorator
class A {}

// 等同于

class A {}
A = decorator(A) || A;
```

修饰器函数的第一个参数，就是所要修饰的目标类。

修饰器对类的行为的改变，是代码编译时发生的，而不是在运行时。这意味着，修饰器能在编译阶段运行代码。也就是说，修饰器本质就是编译时执行的函数。

### 方法的修饰

```js
class Person {
    @readonly
    name() {  }
}
```

修饰器参数:

第一个参数是类的原型对象，上例是Person.prototype，修饰器的本意是要“修饰”类的实例，但是这个时候实例还没生成，所以只能去修饰原型（这不同于类的修饰，那种情况时target参数指的是类本身）；

第二个参数是所要修饰的属性名，

第三个参数是该属性的描述对象。

```js
// descriptor对象原来的值如下
{
    value: specifiedFunction, //该属性值
    enumerable: false, //该属性能否被枚举
    configurable: true, //该属性能否被配置
    writable: true //该属性能否被修改
};
```

如果同一个方法有多个修饰器，会像剥洋葱一样，先从外到内进入，然后由内向外执行

```js
function dec(id){
  console.log('evaluated', id);
  return (target, property, descriptor) => console.log('executed', id);
}

class Example {
    @dec(1)
    @dec(2)
    method(){}
}
// evaluated 1
// evaluated 2
// executed 2
// executed 1
```

## ArrayBuffer

## DataView

## File

继承自**Blob**对象, 在 Blob 对象基础上增加了和 File 相关的属性.
**Blob**对象代表浏览器所能读取的一组原始二进制流.

### 实例化

`new File( binarydata, filename, options)`
实例化一个File类，需要三个参数，**binarydata**文件二进制数据，**filename**文件名，**options**文件配置，比如文件类型

## FileReader

### 方法

#### readAsBinaryString(file)

读取文件内容，读取结果为一个 binary string。文件每一个 byte 会被表示为一个 [0..255] 区间内的整数。函数接受一个 File 对象作为参数。

#### readAsText(file)

读取文件内容，读取结果为一串代表文件内容的文本。函数接受一个 File 对象以及文本编码名称作为参数。

#### readAsDataURL(file)

读取文件内容，读取结果为一个 data: 的 URL。DataURL 由 RFC2397 定义，具体可以参考 http://www.ietf.org/rfc/rfc2397.txt。

### 事件

#### onloadstart

文件读取开始时触发。

#### onprogress

当读取进行中时定时触发。事件参数中会含有已读取总数据量。

#### onabort

当读取被中止时触发。

#### onerror

当读取出错时触发。

#### onload

当读取成功完成时触发。

#### onloadend

当读取完成时，无论成功或者失败都会触发。

## FormData

## Array

### 静态方法

#### Array.isArray()

判断一个对象是不是数组

### 实例方法

#### indexOf()

indexOf()方法返回在该数组中第一个找到的元素位置，如果它不存在则返回-1

#### lastindexOf()

与**indexOf**相反，返回在该数组中最后一个找到的元素位置

#### every()

检测数组中的每一项是否符合条件，参数为一个**callback**
**callback**需要**return**检测条件判断结果，都为**true**则最终返回**true**

#### some()

检测数组中是否有某一项符合条件，参数为一个**callback**
**callback**需要**return**检测条件判断结果，有一个为**true**则最终返回**true**

#### forEach()

为每个元素执行对应的方法，参数为**callback**和**context**
**callback**参数为**item**（每一项），**index**（索引）和**list**（便利数组对象），需要返回结果，如果没有**return**，生成的数组该项为**undefined**
**context**为**callback**回调上下文

#### map()

对数组的每个元素进行一定操作（映射）后，会返回一个新的数组，参数同上

#### filter()

创建一个新的匹配过滤条件的数组，参数为一个**callback**
**callback**参数为**item**（每一项），**index**（索引）和**list**（便利数组对象），需要返回判断结果

#### reduce()

实现一个累加器的功能，将数组的每个值（从左到右）将其降低到一个值，参数为**callback**和**initialValue**
**callback**参数为**prev**（上一项），**next**（下一项），**index**（索引）和**list**（便利数组对象），需要返回结果，如果没有**return**，生成的数组该项为**undefined**
**initialValue**prev的初始值，如果为空，则prev是数组第一项

#### reduceRight()

实现一个累加器的功能，将数组的每个值（从右到左）将其降低到一个值