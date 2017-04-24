# Nodejs

## util工具模块

引用：require(‘util’)

util.debug(string) 将string实时输出到stderr标准错误。阻塞当前进程知道输出完成。

util.log(string) 输出当前时间+string

util.inherits(constructor, superConstructor)将superConstructor的原型方法继承到contructor中，内部处理是constructor.prototype是superConstructor的实例，constructor的super_为superConstructor

## events事件模块

引用：require(‘events’)

events.EventEmitter 事件触发类

emitter.addListener/emitter.on(event,listener) 将一个监听器添加到指定事件的监听器数组的末尾。

emitter.once(event,listener) 为事件添加一次性监听器

emitter.removeListener(event,listener) 将监听器从制定事件的监听器数组移除

emitter.removeAllListeners(event) 将指定事件的所有监听器移除

emitter.setMaxListeners(n) 默认超过10个触发器会警告，设置数量值，0为没有限制

emitter.emit(event,arg1,arg2) 顺序执行监听器列表中的函数

event: newListener 有新的监听器触发事件

## Buffer

Buffer类是专门用来存放二进制数据的缓存区

Buffer是一个JS与C++结合的模块,性能部分用C++实现,非性能部分用JS

Buffer是一个全局类,无需加载就可使用.

### 创建

* `new Buffer(size)`

```
var b1 = new Buffer(100);
console.log(b1); //<Buffer c0 f6 15 00 00 00 00 00 01 00 00 00 02 00 00 00 04 00 00 00 03 00 00 00 03 00 00 00 04 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 f0 00 ... >
console.log(b1.length); //100
```

* `new Buffer(array)`

```
var b1 = new Buffer([1,2,3,4]);
console.log(b1); // <Buffer 01 02 03 04>
console.log(b1.length); // 4
```

* `new Buffer(str, [encoding])`

```
// encoding字符编码默认为'utf8'
var b1 = new Buffer('你好 node');
console.log(b1); // <Buffer e4 bd a0 e5 a5 bd 20 6e 6f 64 65>
console.log(b1.length); // 11

// 使用不同的字符编码，缓存区诗句也不同
var b1 = new Buffer('你好 node', 'ascii');
console.log(b1); //<Buffer 60 7d 20 6e 6f 64 65>
console.log(b1.length); //7
```

### Buffer对象下标

```
\> buff = new Buffer(20)
<Buffer b0 1c 3d 00 88 f9 27 00 00 ff ff ff ff ff ff ff ff ff ff ff>
\> buff[2]
61
\> buff[2] = 80
80
\> buff[2]
80
\> buff[2] = -100  ## 赋值负数情况
-100
\> buff[2]
156
\> buff[2] = 300   ## 赋值正数
300
\> buff[2]
44
\> buff[2] = 3.14156  ## 赋值小数
3.14156
\> buff[2]
3
```

给元素赋值如果<0,就把该值依次+256,直到得到介于0-255之间的整数.如果赋值>256,就依次-256,直到得到介于0-255之间的数值.如果是小数,舍去小数部分,保留整数.

### 字符串和缓存区Buffer

字符串长度: 以文字为单位; 缓存区长度(Buffer): 以字节为单位

字符串创建后不可修改,Buffer对象是可以被修改的.

Buffer也具备slice方法, 但修改slice取出来的数据,会影响到缓存区里面的数据

Buffer对象转为字符串

Buffer与JSON的转换

### Buffer的两个方法

* `Buffer.fill(value,[offset],[end])`

```
//value: 需要被写入的数值,offset: 开始写入指定值的起始位置（默认0）,结束位置(默认到结尾)

\> buff = new Buffer('我爱前端')
<Buffer e6 88 91 e7 88 b1 e5 89 8d e7 ab af>
\> buff.fill(1,3,6)
<Buffer e6 88 91 01 01 01 e5 89 8d e7 ab af>
\> buff
<Buffer e6 88 91 01 01 01 e5 89 8d e7 ab af>
\> buff.toString()
'我\u0001\u0001\u0001前端'
```

* `Buffer.copy(targetBuffer,[targetStart],[sourceStart],[sourceEnd])`

Buffer复制数据到targetBuffer。

```
// copy()复制缓存区数据
// targetBuffer: 目标Buffer,targetStart:从第几个字节写入数据(默认0),sourceStart:指定复制源获取数据开始位置(默认0),sourceEnd:指定复制源获取数据结束位置(默认复制源buffer长度)

\>  buff1 = new Buffer('我爱前端')
<Buffer e6 88 91 e7 88 b1 e5 89 8d e7 ab af>
\> buff2 = new Buffer(12)
<Buffer 00 00 00 00 00 00 00 00 00 00 00 00>
\> buff2.fill(0)
<Buffer 00 00 00 00 00 00 00 00 00 00 00 00>
\> buff1.copy(buff2,0)
12
\> buff2
<Buffer e6 88 91 e7 88 b1 e5 89 8d e7 ab af>
\> buff2.toString()
'我爱前端'
```

### Buffer类方法

* isBuffer(obj)
  判断一个对象是否是Buffer对象

* byteLength
  计算一个指定字符串的字节数

* concat()
  把几个Buffer对象结合成新的Buffer对象.

* sEncoding()
  检测一个字符串是否是有效的编码格式字符串

### Buffer的内存分配

```
//V8的内存机制

1. 受内存的限制情况
在Node中使用内存，只能使用到系统的一部分内存，64位系统下约为1.4GB，32位系统下约为0.7GB。这归咎于Node使用了本来运行在浏览器的V8引擎。
V8引擎的设计之初只是运行在浏览器中，而在浏览器的一般应用场景下使用起来绰绰有余，足以胜任前端页面中的所有需求。
在启动node程序的时候，可以传递两个参数来调整内存限制的大小。
node --max-nex-space-size=1024 app.js // 单位为KB Node内存堆中的「新生代」
node --max-old-space-size=2000 app.js // 单位为MB Node内存堆中的「老生代」

2. 不受内存限制的情况
在Node中，使用Buffer可以读取超过V8内存限制的大文件。原因是Buffer对象不同于其他对象，它不经过V8的内存分配机制。这在于Node并不同于浏览器的应用场景。
在浏览器中，JavaScript直接处理字符串即可满足绝大多数的业务需求，而Node则需要处理网络流和文件I/O流，操作字符串远远不能满足传输的性能需求。
```

```
Buffer的内存分配是在C++层面申请内存,在javascript中分配内存.

Node采用的slab(动态内存管理)分配机制.
- slab: 已经申请好的固定大小的一块内存区域,具备三种状态
1. full: 完全分配状态
2. partial: 部分分配状态
3. empty: 没有被分配状态
```

在 lib/buffer.js 模块中，有个模块私有变量 pool， 它指向当前的一个8K 的slab ：Node以8 kb 为界限来区别Buffer对象是大对象还是小对象.

#### 分配小的Buffer对象

```
//小于8kb,使用局部变量pool,让处于分配状态的slab单元指向它.

Buffer.poolSize = 8 * 1024;
var pool;

function allocPool() {
  pool = new SlowBuffer(Buffer.poolSize);
  pool.used = 0;
}
//新建小buffer时，如果还没pool,就创建一个slab指向它，当前的buffer对象的parent指向slab

this.parent = pool; //当前的buffer对象的parent指向slab
this.offset = pool.used;//slab开始使用的位置
pool.used += this.length;//slab已使用量
if (pool.used & 7) pool.used = (pool.used + 8) & ~7;

//此时slab状态为partial,在创建buffer时会判断次slab是否够用，如果不够，就构建新的slab,原来的slab剩余的空间浪费，如果不释放就占据8kb

```

#### 分配大的Buffer对象

```
//直接分配一个SlowBuffer对象作为slab单元，并且独占

// Big buffer,just alloc one  SlowBuffer由c++定义,勿直接操作它
this.parent=new SlowBuffer(this.length);
this.offset=0;
```

