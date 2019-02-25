# shell

## local

local一般用于局部变量声明，多在在函数内部使用。

1.shell 脚本中定义的变量是 global 的，其作用域从被定义的地方开始，到 shell 结束或被显示删除的地方为止。

2.shell 函数定义的变量默认是 global 的，其作用域从“函数被调用时执行变量定义的地方”开始，到 shell 结束或被显示删除处为止。函数定义的变量可以被显示定义成 local 的，其作用域局限于函数内。但请注意，函数的参数是local的。

3.如果同名，Shell 函数定义的 local 变量会屏蔽脚本定义的 global 变量。

## $()和 ``

在 bash shell 中，$( ) 与 `` (反引号) 都是用来做命令替换用(commandsubstitution)的。

所谓命令替换，就是 shell 命令预执行，在 shell 脚本执行时，会先扫描一遍命令行，发现了命令替换结构，便将结构中的 command 执行一次，得到其标准输出，再将此输出放到原来命令。

## 特殊变量

Shell特殊变量：Shell $0, $#, $*, $@, $?, $$和命令行参数

### 特殊变量列表

变量 | 含义
---------|----------
$0 | 当前脚本的文件名
$n | 传递给脚本或函数的参数。n 是一个数字，表示第几个参数。例如，第一个参数是$1，第二个参数是$2。
$# | 传递给脚本或函数的参数个数。
$* | 传递给脚本或函数的所有参数。
$@ | 传递给脚本或函数的所有参数。被双引号(" ")包含时，与 $* 稍有不同，下面将会讲到。
$? | 上个命令的退出状态，或函数的返回值。一般情况下，大部分命令执行成功会返回 0，失败返回 1。

### $* 和 $@ 的区别

$* 和 $@ 都表示传递给函数或脚本的所有参数，不被双引号(" ")包含时，都以"$1" "$2" … "$n" 的形式输出所有参数。

但是当它们被双引号(" ")包含时，"$*" 会将所有的参数作为一个整体，以"$1 $2 … $n"的形式输出所有参数；"$@" 会将各个参数分开，以"$1" "$2" … "$n" 的形式输出所有参数。

## 函数

Shell 函数必须先定义后使用

```shell
function_name () {
    list of commands
    [ return value ]
}
```

or

```shell
function function_name () {
    list of commands
    [ return value ]
}
```

函数返回值，可以显式增加 return 语句；如果不加，会将最后一条命令运行结果作为返回值。

Shell 函数返回值只能是整数，一般用来表示函数执行成功与否，0表示成功，其他值表示失败。如果 return 其他数据，比如一个字符串，往往会得到错误提示：“numeric argument required”。

调用函数只需要给出函数名，不需要加括号。

## 流程控制

### if

```shell
if list then
  # do something here
elif list then
  # do another thing here
else
  # do something else here
fi
```

### 判断

#### 字符串判断

str1 = str2   当两个串有相同内容、长度时为真
str1 != str2    当串str1和str2不等时为真
-n str1   当串的长度大于0时为真(串非空)
-z str1   当串的长度为0时为真(空串)
str1    当串str1为非空时为真

#### 数字的判断

int1 -eq int2   两数相等为真
int1 -ne int2   两数不等为真
int1 -gt int2   int1大于int2为真
int1 -ge int2   int1大于等于int2为真
int1 -lt int2   int1小于int2为真
int1 -le int2   int1小于等于int2为真

#### 文件的判断

-r file   用户可读为真
-w file   用户可写为真
-x file   用户可执行为真
-f file   文件为正规文件为真
-d file   文件为目录为真
-c file   文件为字符特殊文件为真
-b file   文件为块特殊文件为真
-s file   文件大小非0时为真
-t file   当文件描述符(默认为1)指定的设备为终端时为真

#### 复杂逻辑判断

-a    与
-o    或
!   非