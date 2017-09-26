# OpenGL

## 深度测试

在绘制3D场景的时候，我们需要决定哪些部分对观察者是可见的，或者说哪些部分对观察者不可见，对于不可见的部分，我们应该及早的丢弃，例如在一个不透明的墙壁后的物体就不应该渲染。这种问题称之为[隐藏面消除](https://en.wikipedia.org/wiki/Hidden_surface_determination)（Hidden surface elimination）,或者称之为找出可见面(Visible surface detemination)。

解决这一问题比较简单的做法是[画家算法](https://en.wikipedia.org/wiki/Painter%27s_algorithm)(painter’s algorithm)。画家算法的基本思路是，先绘制场景中离观察者较远的物体，再绘制较近的物体。画家算法很简单，但另一方面也存在缺陷，例如下面的图中，三个三角形互相重叠的情况，画家算法将无法处理.

解决隐藏面消除问题的算法有很多，具体可以参考[Visible Surface Detection](http://www.tutorialspoint.com/computer_graphics/visible_surface_detection.htm)。结合OpenGL，我们使用的是Z-buffer方法，也叫深度缓冲区Depth-buffer。

深度缓冲区(Detph buffer)同颜色缓冲区(color buffer)是对应的，颜色缓冲区存储的像素的颜色信息，而深度缓冲区存储像素的深度信息。在决定是否绘制一个物体的表面时，首先将表面对应像素的深度值与当前深度缓冲区中的值进行比较，如果大于等于深度缓冲区中值，则丢弃这部分;否则利用这个像素对应的深度值和颜色值，分别更新深度缓冲区和颜色缓冲区。这一过程称之为深度测试(Depth Testing)。在OpenGL中执行深度测试时，我们可以根据需要指定深度值的比较函数，后面会详细介绍具体使用。

## 使用深度测试

深度缓冲区一般由窗口管理系统，深度值一般由16位，24位或者32位值表示，通常是24位。位数越高的话，深度的精确度越好。

首先我们需要开启深度测试，默认是关闭的：

`glEnable(GL_DEPTH_TEST);`

再绘制场景前，清除颜色缓冲区同时清除深度缓冲区

```opengl
glClearColor(0.18f, 0.04f, 0.14f, 1.0f);
glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
```

清除深度缓冲区的默认值是1.0，表示最大的深度值，深度值的范围在[0,1]之间，值越小表示越靠近观察者，值越大表示远离观察者。

开启深度测试后，当前深度值和深度缓冲区中的深度值进行比较的函数，是通过glDepthFunc指定的

`glDepthFunc(GL_ALWAYS);` 这个函数有一个参数，具体值如下表:

| 函数 | 说明 |
|----|-----|
| GL_ALWAYS | 总是通过测试 |
| GL_NEVER | 总是不通过测试 |
| GL_LESS | 在当前深度值 < 存储的深度值时通过 |
| GL_EQUAL | 在当前深度值 = 存储的深度值时通过 |
| GL_LEQUAL | 在当前深度值 <= 存储的深度值时通过 |
| GL_GREATER | 在当前深度值 > 存储的深度值时通过 |
| GL_NOTEQUAL | 在当前深度值 不等于 存储的深度值时通过 |
| GL_GEQUAL | 在当前深度值 >= 存储的深度值时通过 |

使用深度测试，最常见的错误时没有开启深度测试，或者没有清除深度缓冲区。

与深度缓冲区相关的另一个函数是glDepthMask,它的参数是布尔类型，*GL_FALSE*将关闭缓冲区写入，默认是*GL_TRUE*，开启了深度缓冲区写入。

## 可视化深度值

在可视化深度值之前，首先我们要明白，这里的深度值，实际上是屏幕坐标系下的zwin坐标，屏幕坐标系下的(x,y)坐标分别表示屏幕坐标系下以左下角(0,0)为起始点的坐标。zwin我们如何获取呢？ 可以通过着色器的输入变量*gl_FragCoord.z*来获取，这个*gl_FragCoord*的z坐标表示的就是深度值。