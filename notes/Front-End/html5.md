# HTML5

## reflow(回流)和repaint(重绘)

repaint(重绘) ，repaint发生更改时，元素的外观被改变，且在没有改变布局的情况下发生，如改变outline,visibility,background color，不会影响到dom结构渲染。

reflow(渲染)，与repaint区别就是他会影响到dom的结构渲染，同时他会触发repaint，他会改变他本身与所有父辈元素(祖先)，这种开销是非常昂贵的，导致性能下降是必然的，页面元素越多效果越明显。

### 何时发生：

1. DOM元素的添加、修改（内容）、删除( Reflow + Repaint)

2. 仅修改DOM元素的字体颜色（只有Repaint，因为不需要调整布局）

3. 应用新的样式或者修改任何影响元素外观的属性

4. Resize浏览器窗口、滚动页面

5. 读取元素的某些属性（offsetLeft、offsetTop、offsetHeight、offsetWidth、 scrollTop/Left/Width/Height、clientTop/Left/Width/Height、 getComputedStyle()、currentStyle(in IE))

### 如何避免：

1. 先将元素从document中删除，完成修改后再把元素放回原来的位置

2. 将元素的display设置为”none”，完成修改后再把display修改为原来的值

3. 如果需要创建多个DOM节点，可以使用DocumentFragment创建完后一次性的加入document 　　

```
var fragment = document.createDocumentFragment();
fragment.appendChild(document.createTextNode('keenboy test 111'));
fragment.appendChild(document.createElement('br'));
fragment.appendChild(document.createTextNode('keenboy test 222'));
document.body.appendChild(fragment);
```

4. 集中修改样式

  4.1尽可能少的修改元素style上的属性

  4.2尽量通过修改className来修改样式
    4.3通过cssText属性来设置样式值

      ```
      element.style.width=”80px”;  //reflow
      element.style.height=”90px”; //reflow
      element.style.border=”solid 1px red”; //reflow 以上就产生多次reflow，调用的越多产生就越多
      element.style.cssText=”width:80px;height:80px;border:solid 1px red;”; //reflow　
      ```

    4.4缓存Layout属性值

      `var left=elem.offsetLeft; 多次使用left也就产生一次reflow`

    4.5设置元素的position为absolute或fixed

      元素脱离标准流，也从DOM树结构中脱离出来，在需要reflow时只需要reflow自身与下级元素

    4.6尽量不要用table布局

        table元素一旦触发reflow就会导致table里所有的其它元素reflow。在适合用table的场合，可以设置table-layout为auto或fixed，这样可以让table一行一行的渲染，这种做法也是为了限制reflow的影响范围

    4.7避免使用expression,他会每次调用都会重新计算一遍(包括加载页面)

<!-- 参考：Yahoo! 性能工程师 Nicole Sullivan 在最新的文章 《Reflows & Repaints: CSS Performance making your JavaScript slow?》 -->

## API

### Animation Timing

Window.requestAnimationFrame(callback)： 告诉浏览器希望执行一个动画，让浏览器在下一个动画帧安排一次网页重绘（类似于 setTimeout，但是更精确）

* 该函数返回一个唯一标识符，在取消动画请求时使用
* callback： 你希望调用的函数

**requestAnimationFrame**的优势，在于充分利用显示器的刷新机制，比较节省系统资源。显示器有固定的刷新频率，**requestAnimationFrame**的基本思想就是与这个刷新频率保持同步，利用这个刷新频率进行页面重绘。

使用这个API，一旦页面不处于浏览器的当前标签，就会自动停止刷新以节省**CPU**、**GPU**和**电力**。

如果主线程非常繁忙，**requestAnimationFrame**的动画效果会大打折扣。

Window.cancelAnimationFrame(requestID)： 取消对应 requestID 的动画请求

### Fullscreen

Element.[webkit|moz|ms]requestFullscreen()： 发出一个异步请求，使指定元素全屏显示

Document.[webkit|moz|ms]exitFullscreen()： 取消全屏

### Page Visibility

判断页面是否处于浏览器的当前窗口，即是否可见

Document.hidden： 只读属性，返回一个布尔值标识当前页面是否隐藏（hidden）

Document.visibilityState： 只读属性，表示当前页面的状态，可以取三个值，分别是 visibile（页面可见）、hidden（页面不可见）、prerender（页面正处于渲染之中，不可见）

visibilitychange 事件： 当当前页面状态发生变化时触发

### Geolocation

获取设备地理位置的可编程的对象，它可以让Web内容访问到设备的地理位置，这将允许Web应用基于用户的地理位置提供定制的信息

navigator.geolocation： 只读属性，返回一个 Geolocation 对象，通过这个对象可以访问到设备的位置信息

Geolocation.getCurrentPosition(success, error, options)： 获取设备当前位置

Geolocation.watchPosition(success[, error[, options]])： 注册监听器，在设备的地理位置发生改变的时候自动被调用，返回一个唯一标识符

Geolocation.clearWatch(id)： 清除指定 ID 的监听器

### Web Notification

为用户设置和显示桌面通知

#### 实例化

new Notification(title, options)
    * title： 显示通知的标题
    * options： 一个用来设置通知的对象（下面只列出部分参数，全部参数看这里）
        * dir： 文字的方向， auto（自动）, ltr（从左到右）, rtl（从右到左）
        * lang： 指定通知中所使用的语言
        * body： 通知中额外显示的字符串
        * tag： 赋予通知一个 ID，以便在必要的时候对通知进行刷新、替换或移除。
        * icon： 一个图片的URL，将被用于显示通知的图标。

#### 事件

* click 事件： 用户点击通知时触发
* show 事件： 通知显示时触发
* error 事件： 通知遇到错误时触发
* close 事件： 用户关闭通知时触发

#### 静态方法

Notification.permission： 静态只读属性，返回一个状态字符串（granted、denied、default），可以用来判断当前页面是否允许显示通知

Notification.requestPermission()： 静态方法，返回一个 Promise 对象，用于向用户申请当前页面显示通知的权限。这个方法只能被用户行为调用（比如：onclick 事件），并且不能被其他的方式调用

#### 实例方法

close()： 关闭通知

