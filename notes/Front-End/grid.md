# Grid

CSS Grid布局 （又名"网格"），是一个基于二维网格布局的系统，主要目的是改变我们基于网格设计的用户接口方式。

## 基础

定义一个容器元素并设置display：grid，使用 grid-template-columns 和 grid-template-rows 属性设置网格的列与 行的大小，然后使用 grid-column 和 grid-row 属性将其子元素放入网格之中。

### 网格容器(Grid Container)

当一个元素设置display: grid属性时，它就会成为所有网格项(Grid Items)的父元素

```html
<div class="container">
    <div class="item item-1"></div>
    <div class="item item-2"></div>
    <div class="item item-3"></div>
</div>
```

### 网格项(Grid Item)

上面的 item 元素都是网格项

### 网格线(Grid Line)

分界线构成了网格的结构。他们可以是垂直的("列网格线")也可以是水平的("行网格线")，并且存在于一行或一列的任一侧。

### 网格轨道(Grid Track)

两个相邻网格线之间的空间。你可以把它们想像成网格的行或列。

### 网格单元格(Grid Cell)

两个相邻的行和两个相邻的列之间的网格线空间。它是网格的一个"单位"。

### 网格区域(Grid Area)

四条网格线所包围的所有空间。网格区域可由任意数量的网格单元格组成。

## 容器属性

### display

定义一个元素成为网格容器，并对其内容建立一个网格格式的上下文。

```css
.container{
    display: grid | inline-grid   
}
/* column, float, clear, 和 vertical-align 元素对网格容器不起作用。 */
```

### grid-template-rows

利用以空格分隔的值定义网格的列和行。值的大小代表轨道的大小，并且它们之间的空格表示网格线。

```css
.container{
    grid-template-columns: <track-size> ... | <line-name> <track-size> ... | subgrid;
    grid-template-rows: <track-size> ... | <line-name> <track-size> ... | subgrid;
}
/*
<track-size>: 可以是一个长度、百分比或者是网格中自由空间的一小部分(使用fr单位)
<line-name>: 你选择的任意名称
subgrid - 如果你的网格容器本身就是一个网格项(即嵌套网格)，你可以使用此属性指定行和列的大小继承于父元素而不是自身指定。
*/
```

### grid-template-areas

使用grid-area属性定义网格区域名称，从而定义网格模板。网格区域重复的名称就会导致内容跨越这些单元格。
句点表示一个空单元格。语法本身提供了一种可视化的网格结构。

```css
.container{
    grid-template-areas: "<grid-area-name> | . | none | ..."
}
/* <grid-area-name>: 使用grid-area属性定义网格区域名称
.: 句点表示一个空单元格
none: 无网格区域被定义 */
```

### grid-column-gap/grid-row-gap

指定网格线的大小。你可以把它想像成在行/列之间设置间距宽度。

```css
.container{
    grid-column-gap: <line-size>;
    grid-row-gap: <line-size>;
}
/* <line-size>: 一个长度值 */
```

### grid-gap

grid-column-gap 和 grid-row-gap的简写值。

```css
.container{
    grid-gap: <grid-column-gap> <grid-row-gap>;
}
/* <grid-column-gap> <grid-row-gap>: 长度值 */
```

### justify-items

沿列轴对齐网格项中的内容(相反于align-item属性定义的沿行轴对齐)。此值适用于容器内所有的网格项。

```css
.container{
    justify-items: start | end | center | stretch;
}
/* start: 内容与网格区域的左端对齐
end: 内容与网格区域的右端对齐
center: 内容处于网格区域的中间位置
stretch: 内容宽度占据整个网格区域空间(默认值) */
```

### align-items

```css
.container{
    align-items: start | end | center | stretch;
}
/* start: 内容与网格区域的顶端对齐
end: 内容与网格区域的底部对齐
center: 内容处于网格区域的中间位置
stretch: 内容高度占据整个网格区域空间(默认值) */
```

### justify-content

当你使用px这种非响应式的单位对你的网格项进行大小设置时，就有可能出现一种情况--你的网格大小可能小于其网格容器的大小。
在这种情况下，你就可以设置网格容器内网格的对齐方式。此属性会将网格沿列轴进行对齐(相反于align-content属性定义的沿行轴对齐)。

```css
.container{
    justify-content: start | end | center | stretch | space-around | space-between | space-evenly;    
}
/* start: 网格与网格容器的左端对齐
end: 网格与网格容器的右端对齐
center: 网格处于网格容器的中间
stretch: 调整网格项的大小，使其宽度填充整个网格容器
space-around: 在网格项之间设置偶数个空格间隙，其最边缘间隙大小为中间空格间隙大小的一半
space-between: 在网格项之间设置偶数个空格间隙，其最边缘不存在空格间隙
space-evenly: 在网格项之间设置偶数个空格间隙，同样适用于最边缘区域 */
```

### align-content

同上，此属性会将网格沿行轴进行对齐(相反于justify-content属性定义的沿列轴对齐)。

```css
.container{
    align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
/* start: 网格与网格容器的顶端对齐
end: 网格与网格容器的底部对齐
center: 网格处于网格容器的中间
stretch: 调整网格项的大小，使其高度填充整个网格容器
space-around: 在网格项之间设置偶数个空格间隙，其最边缘间隙大小为中间空格空隙大小的一半
space-between: 在网格项之间设置偶数个空格间隙，其最边缘不存在空格间隙
space-evenly: 在网格项之间设置偶数个空格间隙，同样适用于最边缘区域 */
```

### grid-auto-columns/grid-auto-rows

指定任何自动生成的网格轨道(隐式网格跟踪)的大小。
当你显式定位行或列(使用 grid-template-rows/grid-template-columns属性)时,就会产生超出定义范围内的隐式网格轨道。

```css
.container{
    grid-auto-columns: <track-size> ...;
    grid-auto-rows: <track-size> ...;
}
/* <track-siz>: 可以是长度、 百分比或网格自由空间的一小部分(使用fr单位) */
```

### grid-auto-flow

如果你不显式的在网格中放置网格项，自动布局算法就会自动踢出此网格项。此属性用来控制自动布局算法的工作原理。

```css
.container{
    grid-auto-flow: row | column | row dense | column dense
}
/* row: 告诉自动布局算法填充每一行，必要时添加新行
column: 告诉自动布局算法填充每一列，必要时添加新列
dense: 告诉自动布局算法试图填补网格中之前较小的网格项留有的空白 */
```

## 网格项属性(Grid Items)

### grid-column-start/grid-column-end/grid-row-start/grid-row-end

使用特定的网格线确定网格项在网格内的位置。
grid-column-start/grid-row-start 属性表示网格项的网格线的起始位置，
grid-column-end/grid-row-end属性表示网格项的网格线的终止位置。

```css
.item{
    grid-column-start: <number> | <name> | span <number> | span <name> | auto
    grid-column-end: <number> | <name> | span <number> | span <name> | auto
    grid-row-start: <number> | <name> | span <number> | span <name> | auto
    grid-row-end: <number> | <name> | span <number> | span <name> | auto
}
/* <line>: 可以是一个数字来引用相应编号的网格线，或者使用名称引用相应命名的网格线
span <number>: 网格项包含指定数量的网格轨道
span <name>: 网格项包含指定名称网格项的网格线之前的网格轨道
auto: 表明自动定位，自动跨度或者默认跨度之一 */
```

### grid-column/grid-row

grid-column-start + grid-column-end, 和 grid-row-start + grid-row-end属性分别的简写形式。

### grid-area

给网格项进行命名以便于模板使用grid-template-areas属性创建时可以加以引用。
另外也可以被grid-row-start + grid-column-start + grid-row-end + grid-column-end属性更为简洁的加以引用。

```css
.item{
    grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end>;
}
/* <name>: 你所定义的名称
<row-start> / <column-start> / <row-end> / <column-end>: 可以为数字或者名称 */
```

### justify-self

沿列轴对齐网格项中的内容(相反于align-item属性定义的沿行轴对齐)。此值适用于单一网格项中的内容。

```css
.item{
    justify-self: start | end | center | stretch;
}
/* start: 内容与网格区域的左端对齐
end: 内容与网格区域的右端对齐
center: 内容处于网格区域的中间位置
stretch: 内容宽度占据整个网格区域空间(默认值) */
```

### align-self

沿行轴对齐网格项中的内容(相反于justify-item属性定义的沿列轴对齐)。此值适用于单一网格项中的内容。

```css
.item{
    align-self: start | end | center | stretch;
}
/* start: 内容与网格区域的顶端对齐
end: 内容与网格区域的底部对齐
center: 内容处于网格区域的中间位置
stretch: 内容高度占据整个网格区域空间(默认值) */
```

## 兼容性

目前还处于 W3C 的工作草案之中，并且默认情况下，还不被所有的浏览器所支持。
Internet Explorer 10 和 11 已经可以实现支持，但也是利用一种过时的语法实现的。

在 Chrome，导航到chrome://flags 并启用" web 实验平台功能"。该方法同样适用于 Opera (opera://flags)。在Firefox中，启用 layout.css.grid.enabled 标志。