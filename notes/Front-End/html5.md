# HTML5

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

