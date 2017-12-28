# Promise

promise是对异步编程的一种抽象。它是一个代理对象，代表一个必须进行异步处理的函数返回的值或抛出的异常。
也就是说promise对象代表了一个异步操作，可以将异步对象和回调函数脱离开来，通过then方法在这个异步操作上面绑定回调函数。

## promise/A+规范

Promise 表示一个异步操作的最终结果，与之进行交互的方式主要是 then 方法，该方法注册了两个回调函数，用于接收 promise 的终值或本 promise 不能执行的原因。

[promise/A+规范](http://www.ituring.com.cn/article/66566)