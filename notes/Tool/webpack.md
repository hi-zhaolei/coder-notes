# webpack

## 配置

通过你对webpack的使用，有两种方式传递配置对象

1. CLI  如果你使用CLI方式，webpack将会读取一个webpack.config.js的文件，或者通过—config传递对应配置文件

2. node.js API  如果你使用API的形式，需要将配置当作参数传递给webpack方法

### hint

配置文件仅仅是一个模块，不是一个.json的文件

### context

相对于entry配置的基础路径，如果output.pathinfo被设置，它也相对此路径，默认是process.cwd() 文件当前目录路径

### entry 项目入口，如果值是

    * 字符串，它将是项目启动时最先加载的模块；

    * 数组，所有模块都将在启动时被加载，但只有数组最后一个模块被返回

    * 对象，将会创建多个入口，对象的字段名就是是入口的名字，字段值可以是字符串和数组

### output

项目出口，如果你使用任何hash确保有模块的顺序性。需要使用OccurenceOrderPlugin或recordsPath。

    * path 必有！输出的文件目录，绝对地址
    * filename 输出的文件名，相对于output.path的相对路径，不要在这里使用绝对路径。
        * [name]可以用来替换对应的输入口文件r
    * chunkFilename 输出没有入口块文件，output.path的相对路径
        * [name]可以用来替换对应的块名
        * [id]可以用来替换对应的块id
    * sourceMapFilename 生成对应js文件的sourceMap，在output.path目录下，只能相对路径。Default: "[file].map"
        * [file]替代对应js文件名
        * [id]替换块的id
    * devtoolModuleFilenameTemplate sourceMap中模块文件名配置
    * devtoolFallbackModuleFilenameTemplate 类似devtoolModuleFilenameTemplate，用户模块标示重复
    * devtoolLineToLine 允许线对线映射
    * hotUpdateChunkFilename 热更新文件配置，在output.path目录下，Default: "[id].[hash].hot-update.js"
        * [id]替换块的id
    * hotUpdateMainFilename  主热更新文件配置，在output.path目录下，Default: "[hash].hot-update.json"
    * publicPath 网站运行时的访问路径，如果你不清楚这个参数，可以在入口文件设置__webpack_public_path__取代
    * jsonpFunction 异步加载块的时候JSONP的回调函数名，Default: "webpackJsonp"
    * hotUpdateFunction 异步加载热更新文件的时候JSONP的回调函数名，Default: "webpackHotUpdate"
    * pathinfo 加载包含注释的模块名，不要在开发环境使用，Default: false
    * library 如果设置，到处的包作为一个库，值位输出文件名，使用这个配置如果你在写一个库并且希望将其发布成一个单文件
    * libraryTarget 格式化输出的库，Default: “var”，如果output.library没配置，但是此项配置了非默认值，输出的对象属性都会被复制
        * var 输出库文件为一个变量
        * this 输出库文件为this.library的属性
        * commonjs 输出库文件为模块exports.library
        * commonjs2 输出库文件为模块的module.exports
        * amd 输出库文件为一个AMD模块
        * umd 输出库文件为一个AMD模块或module.exports或全局root的属性
    * umdNamedDefine 如果output.libraryTarget设置为mud并且output.library设置，此项设置为true将命名为AMD模块。
    * sourcePrefix 为包文件中的源信息每一行添加前缀，Default: "|t"
    * crossOriginLoading  配置包的跨域加载，Default:false
        * false 关闭跨域加载
        * anonymous 激活跨域，请求是匿名的
        * user-credentials 激活跨域，请求会带上证书
### module

影响常规模块

    * loaders 自动加载所需loader的数组，重要：这里loaders解析相对于资源施加地址，这意味着它们不是解析相对于配置文件。如果你的模块由npm安装，并且node_modules不是源文件的父级目录，webpack是找不到loader的，你需要添加node_modules文件夹的绝对路径resolveLoader.root选项。
        * test 匹配的文件
        * exclude 需要排除的文件夹目录
        * include 需要匹配的文件夹目录
        * loader 一个可以用!分割的loader字符串组合
        * loaders loader数组
    * preLoaders/postLoader 类似loaders
    * noParse 一个或一组正则表达式，不解析对应文件，无视一个大型库文件可以提高性能，这些文件只能使用export和module.exports对外
    * xxxContextXxx

### resolve

    * alias 模块别名， 如果是相对路径则会相对于加载该模块的路径寻找
    * root 包含你的模块的绝对路径，或者路径数组
    * moduleDirectories 一个目录数组被解析当前和父级目录并进行模块搜索，这个功能类似于node查找node_module目录
    * fallback 一个目录或目录数组，当web pack在resolve.root和resolve.modulesDirectories没找到对应模块的时候使用
    * extensions 可以被解析成模块的文件的扩展数组
    * packageMains 包含package.json的宿主目录数组 Default: ["webpack", "browser", "web", "browserify", ["jam", "main"], "main"]
    * packageAlias
    * unsafeCache 解析文件时遇到符合预期的代码进行缓存，一个正则数组

### resolveLoader 类似resolve，但是用于loader

    * alias loader别名
    * moduleTemplate 可以被忽略的loader字符串

### externals 指定不需要被webpack解析的bundle依赖，依赖关系取决于output.libraryTarget

    * 字符串 精确匹配外部依赖，相同的字符串都会被认为是外部依赖
    * 对象 如果依赖匹配对象中的属性，则属性值会被当做依赖，属性值包含依赖类型前缀并用空格分割。如果属性值为true会使用属性替代，相反则不是外部依赖
    * 函数 function(context, request, callback(err, result))函数会对每一个依赖执行，如果传递的回调函数的返回值会被指定到一个对象的属性上
    * 正则 每一个匹配到的模块都将成为外部依赖，将会请求外部依赖
    * 数组

### target 打包的目标

    * web 打包用于浏览器
    * webworker webworker
    * node 打包用于node环境，require
    * async-node  打包用于node环境，fs vm
    * node-webkit 浏览器和node环境兼容
    * atom 用语atom编辑器

### bail

报告第一个错误

### profile

捕获模块的时间信息

### cache

产生模块和数据块缓存，watch模式下默认开启，可以传递一个对象让webpack缓存到这个对象内

### watch

进入监听模式，文件修改后会rebuild代码

    * aggregateTimeout 文件修改后多久rebuild，默认300
    * poll 定时器rebuild

### debug

开启loader的debug模式

### devtool

选择一个开发工具提高调试能力

    * eval 每一个模块都通过eval之行，后面添加 //@ sourceURL，
    * source-map 原始的source-map实现方式
    * hidden-source-map 和source-map类似，不会给bundle文件添加注释，没注释怎么找文件，找同名结尾.map的对应文件
    * inline-source-map为每一个文件添加source map的dataUrl，对每一个文件包含一个完整的sourceMap信息的Base64个实话字符串
    * eval-source-map 把eval的sourceURL换成完整的sourceMap
    * cheap-source-map 不包含列信息，不包含loader的sourceMap
    * cheap-module-source-map 不包含列信息，同时loader的sourceMap被简化。最终的sourceMap只有一份，他是web pack对loader生成的sourceMap进行简化，然后再次生成。
### devServer
### node

包含一些node方法

### and

设置require.amd和define.amd的值

### loader

自定义一些再loader中可以使用的值

### recordsPath/recordsInputPath/recordsOutputPath

### plugins

添加额外插件

## 运行

webpack [ config ]
     --display-error-details 显示错误详细信息
     --config 使用另一份配置文件来打包
     --watch 监听变动并自动打包
     --p 压缩混淆版本
     --d 生成map映射文件，告知哪些模块被打包到哪里

### 模块引入

html直接引入打包好的js文件，css会自动提出生成style标签放到head里

js直接使用commonJS书写，并可以直接引入需要编译的模块（JSX scss coffee），需要再config配置中配置好对应的加载器

### loader

loader允许在加载模块的时候对文件进行预编译，loader在其他构建工具中类似于任务但是提供了非常强大而简便的方式进行替代。

可以在模块请求中加入需要对文件进行处理的loader，用!分割

`var moduleWithOneLoader = require("my-loader!./my-awesome-module");`

也可以使用外部的lodaer，一样用!分割

`require("./loaders/my-loader!./my-awesome-module");`

并且loader可以进行串联，以!分割，可以对文件的内容通过一个管道进行多重处理，类似于gulp，loader顺序为自右向左

`require("style-loader!css-loader!less-loader!./my-styles.less");`

loader是可以接受query形态的参数，具体参看对应loader的文档

`require("loader?with=parameter!./file");`

require加载模块的方式可以覆盖config中配置loader

在请求前加!会阻止preloders的执行，eg `require("!raw!./script.coffee")`

在请求前加!!会阻止所有预设loader的执行，eg `require(“!!raw!./script.coffee")`

在请求前加-!会阻止preloader和loader的执行，eg `require(“-!raw!./script.coffee")`

#### 编写loader

loader仅仅是一个输出方法的文件，编译器只是执行方法并传入文件内容或者上一个loader的结果，方法的上下文有编译器以及一些可以让loader加载，一步改变调用样式以及获取query参数。第一个loader会接受源文件内容，最后期望的结果会由最后一个加载器输出。结果应该是一个字符串或者buffer，表示对应模块的js源代码，对应的map文件包含在内。

### 插件使用

#### config

#### NormalModuleReplacementPlugin

`new webpack.NormalModuleReplacementPlugin(resourceRegExp, newResource)`

#### ContextReplacementPlugin

替换默认资源，通过解析newContentResource生成递归标志和正则，如果newContentResource是相对路径，它是相对于之前的资源去解析。如果newContentResource是一个函数，它将覆盖提供的对象的“请求”属性。
```
new webpack.ContextReplacementPlugin(
                resourceRegExp,
                [newContentResource],
                [newContentRecursive],
                [newContentRegExp])
```

#### IgnorePlugin

如果请求的模块匹配提供的正则，则不会生成模块

* requestRegExp 一个需要匹配的正则

* contextRegExp 可选，一个需要测试上下文的正则

`new webpack.IgnorePlugin(requestRegExp, [contextRegExp])`

#### PrefetchPlugin

需要在require之前被解析和处理的模块，可以提高性能，聪明的预加载可以尝试提前构建

* context 目录的绝对路径

* request 一个模块的请求路径

`new webpack.PrefetchPlugin([context], request)`

#### ResolverPlugin

对一个或多个解析器（指定类型）应用一个或多个插件

* plugins 需要解析器应用插件

* types 一个或多个解析器类型 default: ["normal”]，解析器类型有 normal, context, loader

`new webpack.ResolverPlugin(plugins, [types])`

#### ResolverPlugin.FileAppendPlugin

如果你有一个模块的package.json/bower.json有错误的"main”入口，你可以使用这个插件来加载正确的入口文件，这个插件将会传递一个路径到模块目录寻找匹配文件

```
new webpack.ResolverPlugin([
  new webpack.ResolverPlugin.FileAppendPlugin(['/dist/compiled-moduled.js'])
])
```

#### BannerPlugin

对每个生成的块文件头部添加一个标题

* banner 标题字符串

* options.raw 如果是true，标志不会被注释

* options.entryOnly 如果是true，标题只会被添加到入口块文件

`new webpack.BannerPlugin(banner, options)`

<!-- optimize -->
#### DedupePlugin

在output删除相同或类似的文件，将会开销生成入口文件性能，但是能减少文件大小。目前实验阶段

`new webpack.optimize.DedupePlugin()`

#### LimitChunkCountPlugin

`new webpack.optimize.LimitChunkCountPlugin(options)`

#### MinChunkSizePlugin

`new webpack.optimize.MinChunkSizePlugin(options)`

#### OccurenceOrderPlugin

`new webpack.optimize.OccurenceOrderPlugin(preferEntry)`

#### UglifyJsPlugin

`new webpack.optimize.UglifyJsPlugin([options])`

#### ngAnnotatePlugin

`new ngAnnotatePlugin([options]);`

#### CommonsChunkPlugin

* name 共用代码块的名字，如果值和已存在的chunk名字一样，该chunk将被选择，如果值是一个数组，将执行多次；如果未设置并且async或children已设置，所有代码块都会被使用，否则filename被用作代码块名字

* filename 共用代码块的文件名模版，可以和output.filename使用一样的占位符

* minChunks

共用代码块需要被包含在一个模块中的最小次数，值必须大于等于2，低于等于代码块的数量，设置为Infinity之生成空的共用代码块文件，通过提供一个函数你可以添加自定义的逻辑。
* chunks 选择源代码块名
* children 如果为true，共用代码的依赖也会被打包
* async 如果为true，创建异步的代码块，作为name和chunks的依赖，和chunks同时加载
* minsize 共用代码块
`new webpack.optimize.CommonsChunkPlugin(options)`

#### AggressiveMergingPlugin

`new webpack.optimize.AggressiveMergingPlugin(options)`

#### AppCachePlugin

生成一个html5应用缓存

#### OfflinePlugin

提供项目离线支持的插件。它根据输出文件自动生成ServiceWorker并选择更新策略。 与AppCache冲突

<!-- module styles -->
#### LabeledModulesPlugin

支持labeled模块

`new webpack.dependencies.LabeledModulesPlugin()`

#### ComponentPlugin

`在webpack使用component`

#### AngularPlugin

在webpack使用angular.js模块

<!-- dependency injection -->
#### DefinePlugin

`new webpack.DefinePlugin(definitions)`

#### ProvidePlugin

`new webpack.ProvidePlugin(definitions)`

#### RewirePlugin

在webpack里使用rewire

#### NgRequirePlugin
自动加载angularjs模块
{
  plugins: [
    new ngRequirePlugin(['file path list for your angular modules. eg: src/**/*.js'])
  ]
}
<!-- localization -->
#### I18nPlugin

`new I18nPlugin(translations: Object, fnName = "__": String)`

<!-- debugging -->
#### SourceMapDevToolPlugin
为assets添加map文件
* test, include 和excluder用来确定哪个自选需要被处理，值可以是正则（文件名需要匹配），字符串（文件名有此开头），数组（每一项都需配匹配）如果省略后缀，默认是.js
* filename 定义输出map文件的文件名，如果没有值sourcemap会输出到行间
*
```
new webpack.SourceMapDevToolPlugin({
  // asset matching
  test: string | RegExp | Array,
  include: string | RegExp | Array,
  exclude: string | RegExp | Array,

  // file and reference
  filename: string,
  append: bool | string,

  // sources naming
  moduleFilenameTemplate: string,
  fallbackModuleFilenameTemplate: string,

  // quality/performance
  module: bool,
  columns: bool,
  lineToLine: bool | object
})
```
<!-- other -->
#### HotModuleReplacementPlugin

激活热替换模块，如果没有在开发模式中则需要record数据。对每一个记录的块生成热更新块文件，并且激活API以及 __webpack_hash__可被访问

`new webpack.HotModuleReplacementPlugin()`

#### ExtendAPIPlugin

在bundle添加变量

`new webpack.ExtendedAPIPlugin()`

#### NoErrorsPlugin

当编译时出现错误，这个插件将跳过

`new webpack.NoErrorsPlugin()`

#### WatchIgnorePlugin

通过路径或正则匹配指定文件修改监听

`new webpack.WatchIgnorePlugin(paths)`


#### 技巧

##### shimming

对不符合要求的模块进行shim处理，需要使用exports-loader{ test: require.resolve("./src/js/tool/swipe.js"),  loader: "exports?swipe"}

##### 自定义公共模块

使用CommonsChunkPlugin来提取多个页面之间的公共模块，可以分组提取

##### 独立打包样式

有时候可能希望项目的样式能不要被打包到脚本中，而是独立出来作为.css，然后在页面中以<link>标签引入。这时候我们需要 extract-text-webpack-plugin

##### 使用CDN/远程文件

有时候我们希望某些模块走CDN并以<script>的形式挂载到页面上来加载，但又希望能在 webpack 的模块中使用上。可以在配置文件里使用 externals
