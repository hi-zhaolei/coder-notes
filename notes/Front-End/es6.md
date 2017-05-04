# JavaScript ES6

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

