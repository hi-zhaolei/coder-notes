# CSS

## 清除浮动

1.clear清除浮动（添加空div法）

在浮动元素下方添加空div,并给该元素写css样式： {clear:both;height:0;overflow:hidden;}

2.给浮动父级设置高度

3.父级同时浮动

4.父级设置成inline-block

5.br 清浮动

```html
<div class="box">
    <div class="top"></div>
    <br clear="both" />
</div>
<!--br 标签自带clear属性，将它设置成both其实和添加空div原理是一样的-->
```

6.给父级添加overflow:hidden

```css
.clear {
    overflow: hidden;
    *zoom: 1;  /*zoom 兼容IE6 IE7*/
}
```

7.after伪类 清浮动

```css
.clear:after{
    content:".";
    clear:both;
    display:block;
    height:0;
    overflow:hidden;
    visibility:hidden;
}
.clear{
    zoom:1;
}
```

8.bootstrap清除浮动

```css
.clearfix:before,
.clearfix:after {
    content: " ";
    display: table;
}

.clearfix:after {
    clear: both;
}

/**
 * For IE 6/7 only
 */
.clearfix {
    *zoom: 1;
}
```

1.:after伪类在元素末尾插入了一个包含空格的字符，并设置display为table

display:table会创建一个匿名的table-cell，从而触发块级上下文（BFC），因为容器内float的元素也是BFC，由于BFC有不能互相重叠的特性，并且设置了clear: both，:after插入的元素会被挤到容器底部，从而将容器撑高。
并且设置display:table后，content中的空格字符会被渲染为0*0的空白元素，不会占用页面空间。

content包含一个空格，是为了解决Opera浏览器的BUG。当HTML中包含 contenteditable 属性时，如果 content 没有包含空格，会造成清除浮动元素的顶部、底部有一个空白（设置font-size：0也可以解决这个问题）。

2.:after伪类的设置已经达到了清除浮动的目的，但还要设置:before伪类，原因如下

:before的设置也触发了一个BFC，由于BFC有内部布局不受外部影响的特性，因此:before的设置可以阻止margin-top的合并。
这样做，其一是为了和其他清除浮动的方式的效果保持一致；其二，是为了与ie6/7下设置zoom：1后的效果一致（即阻止margin-top合并的效果）。

3.zoom: 1用于在ie6/7下触发haslayout和contain floats