# CSS3

## 盒模型(Box Model)

盒模型范围包括margin, border, padding, content, 根据 W3C 的规范，元素内容占据的空间是由 width 属性设置的，而内容周围的 padding 和 border 值是另外计算的。

标准盒模型width只包含content部分，不包含其他

![标准盒模型](http://image79.360doc.com/DownloadImg/2014/10/1001/45984178_1.jpeg)

一旦为页面设置了恰当的 DTD，大多数浏览器都会按照上面的图示来呈现内容。然而 IE 5 和 6 的呈现却是不正确的。

IE5.X 和 6 在怪异模式中使用自己的非标准模型。width包含content, padding和border

![](http://image79.360doc.com/DownloadImg/2014/10/1001/45984178_2.jpeg)

### box-sizing

box-sizing 属性允许您以特定的方式定义匹配某个区域的特定元素。

|||
|------|----------|
|默认值：|  content-box |
|继承性：|  no |
|版本：| CSS3 |
|JavaScript 语法：|  object.style.boxSizing="border-box" |

`box-sizing: content-box|border-box|inherit;`

| 值 | 描述 |
|----|-----|
|content-box |这是由 CSS2.1 规定的宽度高度行为。宽度和高度分别应用到元素的内容框。|
|border-box  |元素设定的宽度和高度决定了元素的边框盒。为元素指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制。|
|inherit | 规定应从父元素继承 box-sizing 属性的值。|

## flex box

Flex 为"弹性布局"，用来为盒状模型提供最大的灵活性。任何一个容器都可以指定为 Flex 布局。

```css
<!-- 块状 -->
.box{
  display: flex;
}
<!-- 行内 -->
.box{
  display: inline-flex;
}
```

设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效。

### 容器属性

#### flex-direction

flex-direction属性决定主轴的方向（即项目的排列方向）。

```css
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

*row（默认值）：主轴为水平方向，起点在左端。
*row-reverse：主轴为水平方向，起点在右端。
*column：主轴为垂直方向，起点在上沿。
*column-reverse：主轴为垂直方向，起点在下沿。

#### flex-wrap

默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。

```css
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

nowrap（默认）：不换行。

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071007.png)

wrap：换行，第一行在上方。

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071008.jpg)

wrap-reverse：换行，第一行在下方。

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071009.jpg)

#### flex-flow

flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。

```css
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

#### justify-content

justify-content属性定义了项目在主轴上的对齐方式。

```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

具体对齐方式与轴的方向有关。下面假设主轴为从左到右。

*flex-start（默认值）：左对齐
*flex-end：右对齐
*center： 居中
*space-between：两端对齐，项目之间的间隔都相等。
*space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

#### align-items

align-items属性定义项目在交叉轴上如何对齐。

具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下。

*flex-start：交叉轴的起点对齐。
*flex-end：交叉轴的终点对齐。
*center：交叉轴的中点对齐。
*baseline: 项目的第一行文字的基线对齐。
*stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

#### align-content

align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

```css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

*flex-start：与交叉轴的起点对齐。
*flex-end：与交叉轴的终点对齐。
*center：与交叉轴的中点对齐。
*space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
*space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
*stretch（默认值）：轴线占满整个交叉轴。

### 项目属性

#### order

order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

```css
.item {
  order: <integer>;
}
```

#### flex-grow

flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

```css
.item {
  flex-grow: <number>; /* default 0 */
}
```

#### flex-shrink

flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

```css
.item {
  flex-shrink: <number>; /* default 1 负值无效*/
}
```

#### flex-basis

flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

```css
.item {
  flex-basis: <length> | auto; /* default auto */
}
```

#### flex

flex属性是flex-grow, flex-shrink 和 flex-basis的简写，后两个属性可选。建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]  /* default 0 1 auto */
}
```

该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。

#### align-self

align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch; /*default auto*/
}
```

## 选择器

element1~element2 选择前面有element1元素的每个element2元素。

[attribute^=value] 选择某元素attribute属性是以value开头的。

[attribute$=value] 选择某元素attribute属性是以value结尾的。

[attribute*=value] 选择某元素attribute属性包含value字符串的。

E:first-of-type 选择属于其父元素的首个E元素的每个E元素。

E:last-of-type 选择属于其父元素的最后E元素的每个E元素。

E:only-of-type 选择属于其父元素唯一的E元素的每个E元素。

E:only-child 选择属于其父元素的唯一子元素的每个E元素。

E:nth-child(n) 选择属于其父元素的第n个子元素的每个E元素。

E:nth-last-child(n) 选择属于其父元素的倒数第n个子元素的每个E元素。

E:nth-of-type(n) 选择属于其父元素第n个E元素的每个E元素。

E:nth-last-of-type(n) 选择属于其父元素倒数第n个E元素的每个E元素。

E:last-child 选择属于其父元素最后一个子元素每个E元素。

:root 选择文档的根元素。

E:empty 选择没有子元素的每个E元素（包括文本节点)。

E:target 选择当前活动的E元素。

E:enabled 选择每个启用的E元素。

E:disabled 选择每个禁用的E元素。

E:checked 选择每个被选中的E元素。

E:not(selector) 选择非selector元素的每个元素。

E::selection 选择被用户选取的元素部分。

## 动画

### Transition

Transition可以在当元素从一种样式变换为另一种样式时为元素添加效果，而不用使用Flash动画或JavaScript。

#### transition-property

规定应用过渡的CSS属性的名称。

#### transition-duration

规定完成过渡效果需要多长时间。

#### transition-delay

规定过渡效果何时开始，默认是0。

#### transition-timing-function

规定过渡效果的时间曲线，默认是”ease”，还有linear、ease-in、ease-out、ease-in-out和cubic-bezier等过渡类型。

transition简写属性，用于在一个属性中设置四个过渡属性。

### Transform

Transform用来向元素应用各种2D和3D转换，该属性允许我们对元素进行旋转、缩放、移动或倾斜等操作。

transform可以有各种变换类型:

none: 定义不进行转换。

matrix(n,n,n,n,n,n): 定义2D转换，使用六个值的矩阵。

matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n): 定义3D转换，使用16个值的4x4矩阵。

translate(x,y): 定义2D位移转换。

translate3d(x,y,z): 定义3D位移转换。

translateX(x): 定义位移转换，只是用X轴的值。

translateY(y): 定义位移转换，只是用Y轴的值。

translateZ(z): 定义3D位移转换，只是用Z轴的值。

scale(x,y): 定义2D缩放转换。

scale3d(x,y,z): 定义3D缩放转换。

scaleX(x): 通过设置X轴的值来定义缩放转换。

scaleY(y): 通过设置Y轴的值来定义缩放转换。

scaleZ(z): 通过设置Z轴的值来定义3D缩放转换。

rotate(angle): 定义2D旋转，在参数中规定角度。

rotate3d(x,y,z,angle): 定义3D旋转。

rotateX(angle): 定义沿着X轴的3D旋转。

rotateY(angle): 定义沿着Y轴的3D旋转。

rotateZ(angle): 定义沿着Z轴的3D旋转。

skew(x-angle,y-angle): 定义沿着X和Y轴的2D倾斜转换。

skewX(angle): 定义沿着X轴的2D倾斜转换。

skewY(angle): 定义沿着Y轴的2D倾斜转换。

perspective(n): 为3D转换元素定义透视视图。

#### 浏览器支持

Internet Explorer 10、Firefox、Opera 支持 transform 属性。
Internet Explorer 9 支持替代的 -ms-transform 属性（仅适用于 2D 转换）。
Safari 和 Chrome 支持替代的 -webkit-transform 属性（3D 和 2D 转换）。
Opera 只支持 2D 转换。

### Animation

Animation让CSS拥有了可以制作动画的功能。使用CSS3的Animation制作动画我们可以省去复杂的js代码。

## 边框

### border-radius

border-radius可以创建圆角边框，border-radius 属性是一个简写属性，用于设置四个 border-*-radius 属性。

|                |
| -------------- |  |
| 默认值：           | 0 |
| 继承性：           | no |
| 版本：            | CSS3 |
| JavaScript 语法： | object.style.borderRadius="5px" |

#### 语法

`border-radius: 1-4 length|% / 1-4 length|%;`

按此顺序设置每个 radii 的四个值。如果省略 bottom-left，则与 top-right 相同。如果省略 bottom-right，则与 top-left 相同。如果省略 top-right，则与 top-left 相同。

#### 浏览器支持

IE9+、Firefox 4+、Chrome、Safari 5+ 以及 Opera 支持 border-radius 属性。

### box-shadow

box-shadow可以为元素添加阴影

|                |
| -------------- |  |
| 默认值：| none |
| 继承性：| no |
| 版本：| CSS3 |
| JavaScript 语法：| object.style.boxShadow="10px 10px 5px #888888" |

#### 浏览器支持

IE9+、Firefox 4、Chrome、Opera 以及 Safari 5.1.1 支持 box-shadow 属性。

### border-image

border-image可以使用图片来绘制边框。

值 | 描述
--|---
border-image-source | 用在边框的图片的路径。
border-image-slice | 图片边框向内偏移。
border-image-width | 图片边框的宽度。
border-image-outset | 边框图像区域超出边框的量。
border-image-repeat | 图像边框是否应平铺(repeated)、铺满(rounded)或拉伸(stretched)。

#### 浏览器支持

Internet Explorer 11, Firefox, Opera 15, Chrome 以及 Safari 6 支持 border-image 属性。
Safari 5 支持替代的 -webkit-border-image 属性。

## 背景

### background-clip

background-clip属性用于确定背景画区，有以下几种可能的属性：

| | |
|--- | --- |
|border-box | 背景从border开始显示 |
|padding-box | 背景从padding开始显示 |
|content-box | 背景显content区域开始显示 |
|no-clip | 默认属性，等同于border-box |

通常情况，背景都是覆盖整个元素的，利用这个属性可以设定背景颜色或图片的覆盖范围。

#### 浏览器支持

IE9+、Firefox、Opera、Chrome 以及 Safari 支持 background-clip 属性。
注释：Internet Explorer 8 以及更早的版本不支持 background-clip 属性。

### background-origin

|||
-----|-----------
默认值： | border-box
继承性： | no
版本： | CSS3
JavaScript 语法： | object.style.backgroundClip="content-box"
|

background-clip属性用于确定背景的位置，它通常与background-position联合使用，可以从 border、padding、content来计算background-position（就像background-clip）。


值 | 描述
--|---
border-box | 背景被裁剪到边框盒。
padding-box | 背景被裁剪到内边距框。
content-box | 背景被裁剪到内容框。

#### 浏览器支持

IE9+、Firefox、Opera、Chrome 以及 Safari 支持 background-clip 属性。

### background-size

|||
-----|-----
默认值： | auto
继承性： | no
版本： | CSS3
JavaScript 语法： | object.style.backgroundSize="60px 80px"

#### 语法

`background-size: length|percentage|cover|contain;`

background-size属性常用来调整背景图片的大小，主要用于设定图片本身。有以下可能的属性：

值 | 描述
--|---
length |
设置背景图像的高度和宽度。
第一个值设置宽度，第二个值设置高度。
如果只设置一个值，则第二个值会被设置为 "auto"。
percentage |
以父元素的百分比来设置背景图像的宽度和高度。
第一个值设置宽度，第二个值设置高度。
如果只设置一个值，则第二个值会被设置为 "auto"。
cover |
把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。
背景图像的某些部分也许无法显示在背景定位区域中。
contain | 把图像图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域。

#### 浏览器支持

IE9+、Firefox 4+、Opera、Chrome 以及 Safari 5+ 支持 background-size 属性。

### background-break

CSS3中，元素可以被分成几个独立的盒子（如使内联元素span跨越多行），background-break 属性用来控制背景怎样在这些不同的盒子中显示。

|||
-----------|---------------------------------------
continuous | 默认值。忽略盒之间的距离（也就是像元素没有分成多个盒子，依然是一个整体一样）
bounding-box | 把盒之间的距离计算在内；
each-box | 为每个盒子单独重绘背景。

## 文字

### word-wrap

CSS3中，word-wrap属性允许您允许文本强制文本进行换行，即这意味着会对单词进行拆分。

|||
-----|-------
默认值： | normal
继承性： | yes
版本： | CSS3
JavaScript 语法： | object.style.wordWrap="break-word"

`word-wrap: normal|break-word;`

值 | 描述
--|---
normal | 只在允许的断字点换行（浏览器保持默认处理）。
break-word | 在长单词或 URL 地址内部进行换行。

#### 浏览器支持

所有主流浏览器都支持 word-wrap 属性。

### text-overflow

它与word-wrap是协同工作的。word-wrap设置或检索当当前行超过指定容器的边界时是否断开转行，而 text-overflow则设置或检索当当前行超过指定容器的边界时如何显示。

|||
-----|-----
默认值： | clip
继承性： | no
版本： | CSS3
JavaScript 语法： | object.style.textOverflow="ellipsis"

`text-overflow: clip|ellipsis|string;`

对于“text-overflow”属性，有“clip”和“ellipsis”两种可供选择。

值 | 描述
--|---
clip | 修剪文本。
ellipsis | 显示省略符号来代表被修剪的文本。
string | 使用给定的字符串来代表被修剪的文本。

#### 浏览器支持

所有主流浏览器都支持 text-overflow 属性。

### text-shadow

CSS3中，text-shadow可向文本应用阴影。能够规定水平阴影、垂直阴影、模糊距离，以及阴影的颜色。

|||
-----|-----
默认值： | none
继承性： | yes
版本： | CSS3
JavaScript 语法： | object.style.textShadow="2px 2px #ff0000"

`text-shadow: h-shadow v-shadow blur color;`

text-shadow 属性向文本添加一个或多个阴影。该属性是逗号分隔的阴影列表，每个阴影有两个或三个长度值和一个可选的颜色值进行规定。省略的长度是 0。

值 | 描述
--|---
h-shadow | 必需。水平阴影的位置。允许负值。
v-shadow | 必需。垂直阴影的位置。允许负值。
blur | 可选。模糊的距离。
color | 可选。阴影的颜色。

### text-fill-color

设置文字内部填充颜色

`text-fill-color：<color>`

### text-stroke

设置文字边界

`text-stroke：<' text-stroke-width '> || <' text-stroke-color '>`

|||
------------------|-----------------
text-stroke-width | 设置或检索对象中的文字的描边厚度
text-stroke-color | 设置或检索对象中的文字的描边颜色

## 渐变

### linear-gradient(线性渐变)

`<linear-gradient> = linear-gradient([ [ <angle> | to <side-or-corner> ] ,]? <color-stop>[, <color-stop>]+)`


`<side-or-corner> = [left | right] || [top | bottom]`

|||
--------|------------------
<angle> | 用角度值指定渐变的方向（或角度）。
to left | 设置渐变为从右到左。相当于: 270deg
to right | 设置渐变从左到右。相当于: 90deg
to top | 设置渐变从下到上。相当于: 0deg
to bottom | 设置渐变从上到下。相当于: 180deg。这是默认值，等同于留空不写。

`<color-stop> = <color> [ <length> | <percentage> ]?`

|||
--------|------
<color> | 指定颜色。
<length> | 用长度值指定起止色位置。不允许负值
<percentage> | 用百分比指定起止色位置。

### radial-gradient(径向渐变)

#### 语法

`<radial-gradient> = radial-gradient([ [ <shape> || <size> ] [ at <position> ]? , | at <position>, ]?<color-stop>[ , <color-stop> ]+)`

`<position> = [ <length>① | <percentage>① | left | center① | right ]? [ <length>② | <percentage>② | top | center② | bottom ]?`

<position> 确定圆心的位置。如果提供2个参数，第一个表示横坐标，第二个表示纵坐标；如果只提供一个，第二值默认为50%，即center

|||
-----------|-------------------------------------------------------------
<percentage①> | 用百分比指定径向渐变圆心的横坐标值。可以为负值。
<length①> | 用长度值指定径向渐变圆心的横坐标值。可以为负值。
left | 设置左边为径向渐变圆心的横坐标值。
center① | 设置中间为径向渐变圆心的横坐标值。
right | 设置右边为径向渐变圆心的横坐标值。
<percentage>② | 用百分比指定径向渐变圆心的纵坐标值。可以为负值。
<length>② | 用长度值指定径向渐变圆心的纵坐标值。可以为负值。
top | 设置顶部为径向渐变圆心的纵坐标值。
center② | 设置中间为径向渐变圆心的纵坐标值。
bottom | 设置底部为径向渐变圆心的纵坐标值。

`<shape> = circle | ellipse`

<shape> 确定圆的类型

|||
--------|-------
circle | 指定圆形的径向渐变
ellipse | 指定椭圆形的径向渐变。

`<size> = <extent-keyword> | [ <circle-size> || <ellipse-size> ]`

|||
------------------------|---------------------
<extent-keyword> | circle ellipse 都接受该值作为 size
closest-side | 指定径向渐变的半径长度为从圆心到离圆心最近的边
closest-corner | 指定径向渐变的半径长度为从圆心到离圆心最近的角
farthest-side | 指定径向渐变的半径长度为从圆心到离圆心最远的边
farthest-corner | 指定径向渐变的半径长度为从圆心到离圆心最远的角

`<extent-keyword> = closest-side | closest-corner | farthest-side | farthest-corner`

`<circle-size> = <length>`

<circle-size> circle 接受该值作为 size
<length> 用长度值指定正圆径向渐变的半径长度。不允许负值。

`<ellipse-size> = [ <length> | <percentage> ]{2}`

<ellipse-size> ellipse 接受该值作为 size

`<shape-size> = <length> | <percentage>`

|||
---------|------------------------------
<length> | 用长度值指定椭圆径向渐变的横向或纵向半径长度。不允许负值。
<percentage> | 用百分比指定椭圆径向渐变的横向或纵向半径长度。不允许负值。


`<color-stop> = <color> [ <length> | <percentage> ]?`

<color-stop> 用于指定渐变的起止颜色

|||
--------|------
<color> | 指定颜色。
<length> | 用长度值指定起止色位置。不允许负值
<percentage> | 用百分比指定起止色位置。不允许负值

## @font-face特性

在CSS3之前，web设计师必须使用已在用户计算机上安装好的字体。通过CSS3，web设计师可以使用他们喜欢的任意字体。当您您找到或购买到希望使用的字体时，可将该字体文件存放到web服务器上，它会在需要时被自动下载到用户的计算机上。字体是在 CSS3 @font-face 规则中定义的。Firefox、Chrome、Safari以及Opera支持 .ttf(True Type Fonts)和 .otf(OpenType Fonts)类型的字体。IE9+ 支持新的@font-face规则，但是仅支持 .eot类型的字体(Embedded OpenType)。

在新的@font-face规则中，必须首先定义字体的名称（比如myFont），然后指向该字体文件。

如需为HTML元素使用字体，请通过font-family属性来引用字体的名称 (myFont)

```
@font-face {
    font-family: myFirstFont;
    src: url('Sansation_Light.ttf'),
         url('Sansation_Light.eot'); /* IE9+ */
}
div{
    font-family:myFirstFont;
}
```

## 多列布局

通过CSS3，能够创建多个列来对文本进行布局，IE10和Opera支持多列属性。Firefox 需要前缀-moz-，Chrome和Safari需要前缀-webkit-。

主要有如下三个属性

### column-count

规定元素应该被分隔的列数。

### column-gap

规定列之间的间隔。

### column-rule

设置列之间的宽度、样式和颜色规则

## 用户界面

CSS3中，新的用户界面特性包括重设元素尺寸、盒尺寸以及轮廓等。Firefox、Chrome以及Safari 支持resize属性。IE、Chrome、Safari以及Opera支持box-sizing属性。Firefox需要前缀-moz-。
所有主流浏览器都支持outline-offset属性，除了IE。

### resize

resize 属性规定是否可由用户调整元素尺寸。如果希望此属性生效，需要设置元素的 overflow 属性，值可以是 auto、hidden 或 scroll。

### box-sizing

box-sizing属性可设置的值有content-box、border-box和inherit。

#### content-box

padding和border不被包含在定义的width和height之内。对象的实际宽度等于设置的width值和border、padding之和，即 (Element width = width + border + padding)，此属性表现为标准模式下的盒模型。

#### border-box

padding和border被包含在定义的width和height之内。对象的实际宽度就等于设置的width值，即使定义有border和padding也不会改变对象的实际宽度，即 (Element width = width)，此属性表现为怪异模式下的盒模型。

### outline-offset

outline-offset属性对轮廓进行偏移，并在超出边框边缘的位置绘制轮廓
