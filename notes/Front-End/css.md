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