# Babel

Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行

## .babelrc

Babel的配置文件是.babelrc，存放在项目的根目录下

```
{
  "presets": [],
  "plugins": []
}
```

**presets**有以下选项

* ES2015转码规则babel-preset-es2015
* react转码规则babel-preset-react
* ES7不同阶段语法提案的转码规则（共有4个阶段）
    * babel-preset-stage-0
    * babel-preset-stage-1
    * babel-preset-stage-2
    * babel-preset-stage-3

## babel-cli

命令行转码工具

### 转码结果输出到标准输出
$ babel example.js

### 转码结果写入一个文件 --out-file 或 -o 参数指定输出文件
$ babel example.js --out-file compiled.js

### 整个目录转码 --out-dir 或 -d 参数指定输出目录
$ babel src --out-dir lib

### -s 参数生成source map文件
$ babel src -d lib -s

## babel-node

babel-cli工具自带一个babel-node命令，提供一个支持ES6的REPL环境。它支持Node的REPL环境的所有功能，而且可以直接运行ES6代码

## babel-register

babel-register模块改写require命令，为它加上一个钩子。此后，每当使用require加载.js、.jsx、.es和.es6后缀名的文件，就会先用Babel进行转码

使用时先加载babel-register，并只会对require命令加载的文件转码，而不会对当前文件转码

由于它是实时转码，所以只适合在开发环境使用

## babel-core

Babel的API，调用方法进行转码

## babel-polyfill

Babel默认只转换新的JavaScript句法（syntax），而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。

## 在线转换

[BABEL REPL](https://babeljs.io/repl/)
