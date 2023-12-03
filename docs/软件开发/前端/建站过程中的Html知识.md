# HTML

## nav

nav标签定义导航链接的部分

## article

article标签用来定义独立的内容，被定义的内容本身必须是有意义的并且必须是独立于文档的其余部分

比如：

* 帖子
* 博客
* 评论

# CSS

## clear

规定元素的哪一侧不允许其他的浮动元素

## position

指定一个元素的定位方法的类型

| 值                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [absolute](https://www.runoob.com/css/css-positioning.html#position-absolute) | 生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| [fixed](https://www.runoob.com/css/css-positioning.html#position-fixed) | 生成固定定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| [relative](https://www.runoob.com/css/css-positioning.html#position-relative) | 生成相对定位的元素，相对于其正常位置进行定位。因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。 |
| [static](https://www.runoob.com/css/css-positioning.html#position-static) | 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。 |
| [sticky](https://www.runoob.com/css/css-positioning.html#position-sticky) | 粘性定位，该定位基于用户滚动的位置。它的行为就像 position:relative; 而当页面滚动超出目标区域时，它的表现就像 position:fixed;，它会固定在目标位置。**注意:** Internet Explorer, Edge 15 及更早 IE 版本不支持 sticky 定位。 Safari 需要使用 -webkit- prefix (查看以下实例)。 |
| inherit                                                      | 规定应该从父元素继承 position 属性的值。                     |
| initial                                                      | 设置该属性为默认值，详情查看 [CSS initial 关键字](https://www.runoob.com/cssref/css-initial.html)。 |

## display

规定元素应该生成的框的类型

| 值                 | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| none               | 此元素不会被显示。                                           |
| block              | 此元素将显示为块级元素，此元素前后会带有换行符。             |
| inline             | 默认。此元素会被显示为内联元素，元素前后没有换行符。         |
| inline-block       | 行内块元素。（CSS2.1 新增的值）                              |
| list-item          | 此元素会作为列表显示。                                       |
| run-in             | 此元素会根据上下文作为块级元素或内联元素显示。               |
| compact            | CSS 中有值 compact，不过由于缺乏广泛支持，已经从 CSS2.1 中删除。 |
| marker             | CSS 中有值 marker，不过由于缺乏广泛支持，已经从 CSS2.1 中删除。 |
| table              | 此元素会作为块级表格来显示（类似 \<table>），表格前后带有换行符。 |
| inline-table       | 此元素会作为内联表格来显示（类似 \<table>），表格前后没有换行符。 |
| table-row-group    | 此元素会作为一个或多个行的分组来显示（类似 \<tbody>）。      |
| table-header-group | 此元素会作为一个或多个行的分组来显示（类似 \<thead>）。      |
| table-footer-group | 此元素会作为一个或多个行的分组来显示（类似 \<tfoot>）。      |
| table-row          | 此元素会作为一个表格行显示（类似 \<tr>）。                   |
| table-column-group | 此元素会作为一个或多个列的分组来显示（类似 \<colgroup>）。   |
| table-column       | 此元素会作为一个单元格列显示（类似\ <col>）                  |
| table-cell         | 此元素会作为一个表格单元格显示（类似 \<td> 和 \<th>）        |
| table-caption      | 此元素会作为一个表格标题显示（类似 \<caption>）              |
| inherit            | 规定应该从父元素继承 display 属性的值。                      |

## overflow

定义如果内容溢出一个元素的框，会发生什么

| 值      | 描述                                                     |
| ------- | -------------------------------------------------------- |
| visible | 默认值。内容不会被修剪，会呈现在元素框之外。             |
| hidden  | 内容会被修剪，并且其余内容是不可见的。                   |
| scroll  | 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。 |
| auto    | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。 |
| inherit | 规定应该从父元素继承 overflow 属性的值。                 |

## @media

使用 @media 查询，可以针对不同的媒体类型定义不同的样式

### 实例

如果文档宽度小于 300 像素则修改背景颜色(background-color):

```css
@media screen and (max-width: 300px) {
    body {
        background-color:lightblue;
    }
}
```

### CSS 语法

```css
@media *mediatype* and|not|only *(media feature)* {    *CSS-Code;*}
```

你也可以针对不同的媒体使用不同 *stylesheets* :

```css
<link rel="stylesheet" media="*mediatype* and|not|only (*media feature*)" href="*mystylesheet.css*">
```

### 媒体类型

| 值         | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| all        | 用于所有设备                                                 |
| aural      | 已废弃。用于语音和声音合成器                                 |
| braille    | 已废弃。 应用于盲文触摸式反馈设备                            |
| embossed   | 已废弃。 用于打印的盲人印刷设备                              |
| handheld   | 已废弃。 用于掌上设备或更小的装置，如PDA和小型电话           |
| print      | 用于打印机和打印预览                                         |
| projection | 已废弃。 用于投影设备                                        |
| screen     | 用于电脑屏幕，平板电脑，智能手机等。                         |
| speech     | 应用于屏幕阅读器等发声设备                                   |
| tty        | 已废弃。 用于固定的字符网格，如电报、终端设备和对字符有限制的便携设备 |
| tv         | 已废弃。 用于电视和网络电视                                  |

### 媒体功能

| 值                      | 描述                                                         |
| ----------------------- | ------------------------------------------------------------ |
| aspect-ratio            | 定义输出设备中的页面可见区域宽度与高度的比率                 |
| color                   | 定义输出设备每一组彩色原件的个数。如果不是彩色设备，则值等于0 |
| color-index             | 定义在输出设备的彩色查询表中的条目数。如果没有使用彩色查询表，则值等于0 |
| device-aspect-ratio     | 定义输出设备的屏幕可见宽度与高度的比率。                     |
| device-height           | 定义输出设备的屏幕可见高度。                                 |
| device-width            | 定义输出设备的屏幕可见宽度。                                 |
| grid                    | 用来查询输出设备是否使用栅格或点阵。                         |
| height                  | 定义输出设备中的页面可见区域高度。                           |
| max-aspect-ratio        | 定义输出设备的屏幕可见宽度与高度的最大比率。                 |
| max-color               | 定义输出设备每一组彩色原件的最大个数。                       |
| max-color-index         | 定义在输出设备的彩色查询表中的最大条目数。                   |
| max-device-aspect-ratio | 定义输出设备的屏幕可见宽度与高度的最大比率。                 |
| max-device-height       | 定义输出设备的屏幕可见的最大高度。                           |
| max-device-width        | 定义输出设备的屏幕最大可见宽度。                             |
| max-height              | 定义输出设备中的页面最大可见区域高度。                       |
| max-monochrome          | 定义在一个单色框架缓冲区中每像素包含的最大单色原件个数。     |
| max-resolution          | 定义设备的最大分辨率。                                       |
| max-width               | 定义输出设备中的页面最大可见区域宽度。                       |
| min-aspect-ratio        | 定义输出设备中的页面可见区域宽度与高度的最小比率。           |
| min-color               | 定义输出设备每一组彩色原件的最小个数。                       |
| min-color-index         | 定义在输出设备的彩色查询表中的最小条目数。                   |
| min-device-aspect-ratio | 定义输出设备的屏幕可见宽度与高度的最小比率。                 |
| min-device-width        | 定义输出设备的屏幕最小可见宽度。                             |
| min-device-height       | 定义输出设备的屏幕的最小可见高度。                           |
| min-height              | 定义输出设备中的页面最小可见区域高度。                       |
| min-monochrome          | 定义在一个单色框架缓冲区中每像素包含的最小单色原件个数       |
| min-resolution          | 定义设备的最小分辨率。                                       |
| min-width               | 定义输出设备中的页面最小可见区域宽度。                       |
| monochrome              | 定义在一个单色框架缓冲区中每像素包含的单色原件个数。如果不是单色设备，则值等于0 |
| orientation             | 定义输出设备中的页面可见区域高度是否大于或等于宽度。         |
| resolution              | 定义设备的分辨率。如：96dpi, 300dpi, 118dpcm                 |
| scan                    | 定义电视类设备的扫描工序。                                   |
| width                   | 定义输出设备中的页面可见区域宽度。                           |

## opacity

设置一个元素的透明度级别

### 语法

```css
opacity: *value*|inherit;
```

| 值      | 描述                                               |
| ------- | -------------------------------------------------- |
| *value* | 指定不透明度。从0.0（完全透明）到1.0（完全不透明） |
| inherit | Opacity属性的值应该从父元素继承                    |

## transform

应用于元素的2D或3D转换。这个属性允许你将元素旋转，缩放，移动，倾斜等。

### 语法

```css
transform: none|*transform-functions*;
```


| 值                                                           | 描述                                    |
| ------------------------------------------------------------ | --------------------------------------- |
| none                                                         | 定义不进行转换。                        |
| matrix(*n*,*n*,*n*,*n*,*n*,*n*)                              | 定义 2D 转换，使用六个值的矩阵。        |
| matrix3d(*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*) | 定义 3D 转换，使用 16 个值的 4x4 矩阵。 |
| translate(*x*,*y*)                                           | 定义 2D 转换。                          |
| translate3d(*x*,*y*,*z*)                                     | 定义 3D 转换。                          |
| translateX(*x*)                                              | 定义转换，只是用 X 轴的值。             |
| translateY(*y*)                                              | 定义转换，只是用 Y 轴的值。             |
| translateZ(*z*)                                              | 定义 3D 转换，只是用 Z 轴的值。         |
| scale(*x*[,*y*]?)                                            | 定义 2D 缩放转换。                      |
| scale3d(*x*,*y*,*z*)                                         | 定义 3D 缩放转换。                      |
| scaleX(*x*)                                                  | 通过设置 X 轴的值来定义缩放转换。       |
| scaleY(*y*)                                                  | 通过设置 Y 轴的值来定义缩放转换。       |
| scaleZ(*z*)                                                  | 通过设置 Z 轴的值来定义 3D 缩放转换。   |
| rotate(*angle*)                                              | 定义 2D 旋转，在参数中规定角度。        |
| rotate3d(*x*,*y*,*z*,*angle*)                                | 定义 3D 旋转。                          |
| rotateX(*angle*)                                             | 定义沿着 X 轴的 3D 旋转。               |
| rotateY(*angle*)                                             | 定义沿着 Y 轴的 3D 旋转。               |
| rotateZ(*angle*)                                             | 定义沿着 Z 轴的 3D 旋转。               |
| skew(*x-angle*,*y-angle*)                                    | 定义沿着 X 和 Y 轴的 2D 倾斜转换。      |
| skewX(*angle*)                                               | 定义沿着 X 轴的 2D 倾斜转换。           |
| skewY(*angle*)                                               | 定义沿着 Y 轴的 2D 倾斜转换。           |
| perspective(*n*)                                             | 为 3D 转换元素定义透视视图。            |

## transition

transition 属性设置元素当过渡效果，四个简写属性为：

- transition-property
- transition-duration
- transition-timing-function
- transition-delay

**注意：** 始终指定transition-duration属性，否则持续时间为0，transition不会有任何效果。

### 语法

```css
transition: *property duration timing-function delay*;
```

| 值                           | 描述                                       |
| ---------------------------- | ------------------------------------------ |
| *transition-property*        | 指定CSS属性的name，transition效果          |
| *transition-duration*        | transition效果需要指定多少秒或毫秒才能完成 |
| *transition-timing-function* | 指定transition效果的转速曲线               |
| *transition-delay*           | 定义transition效果开始的时候               |