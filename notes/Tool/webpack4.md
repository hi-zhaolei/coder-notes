# Webpack 4.0

## 迁徙

如果是从 webpack4.0 之前的版本迁徙而来，需要指定版本

```bash
npm install webpack@4.2.0 --save-dev
```

否则直接安装 webpack 即可

webpack4.0版本将命令行工具剥离，需要单独安装

```bash
npm install webpack-cli --save-dev
```

升级webpack配置中使用的webpack插件

如果编译失败，更新所有编译所需的 loader

## 0配置

webpack拥有很多强大的功能，但是最大的问题就是需要复杂的配置文件。
虽然对于复杂的大型项目，配置文件是必须的，但是对于小型的项目而言，webpack启动就很不友好。

Parceljs之所以得到那么多关注也是因为拿来即用得特性。

在 4.0 版本中，webpack 默认不再需要配置文件

```bash
<!-- 创建目录 -->
mkdir webpack-4-quickstart && cd $_
<!-- 初始化package.json -->
npm init -y
<!-- 安装webapck -->
npm i webpack --save-dev
<!-- 安装webpack命令行工具 -->
npm i webpack-cli --save-dev
<!-- 执行 -->
webpack
```

在 webpack 4 中不再需要指定入口和输出文件，

* entry 的默认值是 ./src，查找 index.js，output.path 的默认值是 ./dist，输出文件名 main.js

* webpack 默认会按照 .wasm, .mjs, .js 和 .json 的扩展名顺序查找模块。

* output.pathinfo 在开发模式下默认是打开的

* 生产环境下，默认关闭内存缓存

## mode

日常的 webpack 配置需要两个文件，分别处理 development 和 production 两种打包环境

在 webpack 4 中，你可以共用一套配置，只需添加 mode 配置

```json
"scripts": {
  "dev": "webpack --mode development",
  "build": "webpack --mode production"
}
```

### Production mode

默认会开启所有优化方式，包括压缩，作用域提升，tree-shaking等

```js
// webpack 3
module.exports = {
  plugins: [
    new UglifyJsPlugin({}),
    new webpack.DefinePlugin({ "process.env.NODE_ENV": "'production'" }),
    new webpack.optimize.ModuleConcatenationPlugin(),
    new webpack.NoEmitOnErrorsPlugin()
  ]
}

// webpack 4
module.exports = {
  mode: 'production'
}
```

### Development mode

正好相反，针对打包速度，只会提供非压缩输出文件

```js
// webpack 3
module.exports = {
  plugins: [
    new webpack.NamedModulesPlugin(),
    new webpack.DefinePlugin({ "process.env.NODE_ENV": "'development'" })
  ]
}
// webpack 4
module.exports = {
  mode: 'development'
}
```

### none mode

不会开启任何配置

```js
module.exports = {
  mode: 'none'
}
```

## 重写默认 input/output

```json
"scripts": {
  "dev": "webpack --mode development ./foo/src/js/index.js --output ./foo/main.js",
  "build": "webpack --mode production ./foo/src/js/index.js --output ./foo/main.js"
}
```

## loader

webpack 0配置只包括上面3点，loader都需要配置文件声明或者npm scripts中使用--module-bind

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  }
};
```

```bash
"scripts": {
  "dev": "webpack --mode development --module-bind js=babel-loader",
  "build": "webpack --mode production --module-bind js=babel-loader"
}
```

上面的配置形式都自 webpack 3 就有，并且相同

webpack作者回复说不希望为核心添加过多，谋求更多的可扩展性

ps:loader仍然需要配置叫什么0配置[鄙视]，Parceljs默认读取package.json加载plugin赶紧学

## 动态导入

在 webpack 4 中，import() 会返回一个带命名空间(namespace)的对象

```js
<!-- before -->
import('a.js').then(a => { /* ... */})
<!-- before -->
import('a.js').then({default: a} => { /* ... */})
```

## 插件

* 删除 NoEmitOnErrorsPlugin，内置生产模式开启

* 删除 ModuleConcatenationPlugin，内置生产模式开启

* 删除 NamedModulesPlugin，内置生产模式开启

* 删除了 CommonsChunkPlugin，现在使用 optimization.splitChunks 和 optimization.runtimeChunk [具体看这里](https://gist.github.com/sokra/1522d586b8e5c0f5072d7565c2bee693)

## 废弃

* 移除了 module.loaders

* 移除了 loaderContext.options

* 移除了 Compilation.notCacheable

* 移除了 NoErrorsPlugin

* 移除了 Dependency.isEqualResource

* 移除了 NewWatchingPlugin

* 移除了 CommonsChunkPlugin