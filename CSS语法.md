# CSS语法

### vertical-align的属性应用:

**必须是行内元素或者是行内块元素，一般使用行内块元素更好**

**vertical-align : baseline |top | middle | bottom**

**baselien: 基线对齐，默认；top: 顶顶对齐；middle: 中线对齐；bottom: 底线对齐。** 

#### 解决图片底部默认空白缝隙问题

bug: 图片低侧会有一个空白缝隙，原因是行内块元素会和文字的基线对齐。

解决办法：

1. 给图片添加 vertical-align: middle | top | bottom;
2. 把图片转换为块级元素 display: block;


### margin负值的巧妙运用：

给一组ul列表中的li列表项添加边框，多个列表相邻，会导致边框1px+1px=2px，可以使用margin-left: -1px;，使边框重叠，效果上不会变宽。

但如果给li添加:hover时，使鼠标悬浮时改变其边框颜色，会导致边框显示不全。

解决方法：

1. 如果原先的li没有定位，直接添加相对定位 position: relative;。
2. 如果原先的li有定位，可以利用z-index: 1;提高层级。

