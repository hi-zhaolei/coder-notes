# 函数式编程

## Arity

函数参数的个数。来自于单词 unary, binary, ternary 等等。这个单词是由 -ary 与 -ity 两个后缀拼接而成。例如，一个带有两个参数的函数被称为二元函数或者它的 arity 是2。它也被那些更喜欢希腊词根而非拉丁词根的人称为 dyadic。同样地，带有可变数量参数的函数被称为 variadic，而二元函数只能带两个参数。

```
const sum = (a, b) => a + b

const arity = sum.length
console.log(arity)        // 2
```

## 高阶函数 (Higher-Order Function / HOF)

以函数为参数或/和返回值

```
const filter = (predicate, xs) => xs.filter(predicate)

const is = (type) => (x) => Object(x) instanceof type

filter(is(Number), [0, '1', 2, null]) // 0, 2
```

## 偏函数 (Partial Function)

针对原始函数的参数，固定一部分并生成一个新的函数

```
const sum = (a, b, c) => a + b + c
const partial = (a, c) => sum(a, 2, c)
partial(1, 3)
```

## 柯里化 (Currying)

将一个多元函数转变为一元函数的过程。 每当函数被调用时，它仅仅接收一个参数并且返回带有一个参数的函数，直到传递完所有的参数

```

```

## 自动柯里化 (Auto Currying)

lodash，understore 和 ramda 有 curry 函数可以自动完成柯里化

## 反柯里化 (Reverse Currying)

## 函数组合 (Function Composing)

接收多个函数作为参数，从右到左，一个函数的输入为另一个函数的输出

## Continuation

在一个程序执行的任意时刻，尚未执行的代码称为 Continuation

## 纯函数 (Purity)

输出仅由输入决定，且不产生副作用

## 副作用 (Side effects)

如果函数与外部可变状态进行交互，则它是有副作用的

## 幂等性 (Idempotent)

如果一个函数执行多次皆返回相同的结果，则它是幂等性的

## Point-Free 风格 (Point-Free Style)

定义函数时，不显式地指出函数所带参数。这种风格通常需要柯里化或者高阶函数。也叫 Tacit programming

## 谓词 (Predicate)

根据输入返回 true 或 false。通常用在 Array.prototype.filter 的回调函数中

## 契约 (Contracts)

契约保证了函数或者表达式在运行时的行为。当违反契约时，将抛出一个错误

## 范畴 (Category)

在范畴论中，范畴是指对象集合及它们之间的态射 (morphism)。在编程中，数据类型作为对象，函数作为态射

## 值 (Value)

赋值给变量的值称作 Value

## 常量 (Constant)

一旦定义不可重新赋值

## 函子 (Functor)

一个实现了map 函数的对象，map 会遍历对象中的每个值并生成一个新的对象

## 用透明性 (Referential Transparency)

一个表达式能够被它的值替代而不改变程序的行为成为引用透明

## 匿名函数 (Lambda)

## 惰性求值 (Lazy evaluation)

按需求值机制，只有当需要计算所得值时才会计算

##
