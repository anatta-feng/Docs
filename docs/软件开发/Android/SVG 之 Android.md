# 简介

SVG 全称为 Scalable Vector Graphics（可伸缩矢量图形），SVG 在 Android 端的实现是 VectorDrawable，是 SVG 的缩减版。

# 基础语法

## 示例

```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android"
    android:width="400dp"
    android:height="400dp"
    android:viewportHeight="400"
    android:viewportWidth="400">
    <path
        android:pathData="M 200 100 L 300 300 L 100 300 z"
        android:strokeColor="#FFF"
        android:strokeWidth="5"
        android:fillColor="#000" />
</vector>
```

这个示例是一个三角形

![image-20181127211532612](https://ws4.sinaimg.cn/large/006tNbRwgy1fxmxkya3jaj3088089dfv.jpg)

## vector 标签

vector 标签是 VectorDrawable 的根标签，该标签包含以下属性

| 属性           | 说明                                                      |
| -------------- | --------------------------------------------------------- |
| name           | 该 drawable 的名字                                        |
| width          | 该 drawable 的内部宽度，通常使用 dp                       |
| height         | 该 drawable 的内部高度，通常使用 dp                       |
| viewportWidth  | 定义矢量图视图宽度（path 绘制的虚拟画布）                 |
| viewportHeight | 定义矢量图视图高度（path 绘制的虚拟画布）                 |
| tint           | 定义该 drawable 的 tint 颜色。默认是没有 tint 颜色的      |
| tintMode       | 定义 tint 颜色的 Porter-Duff blending 模式，默认为 src_in |
| autoMirrored   | 设置当系统为 RTL 布局的时候，是否自动镜像该图片。         |
| alpha          | 该图片的透明度属性                                        |

## path 标签

path 在 VectorDrawable 中代表一条线，该标签包含以下属性

| 属性             | 说明                                                 |
| ---------------- | ---------------------------------------------------- |
| name             | 该 path 的名字，可以通过这个名字获取该 path 的引用。 |
| pathData         | 该 path 的路径信息                                   |
| fillColor        | 定义填充路径的颜色                                   |
| fillAlpha        | 路径填充颜色的透明度                                 |
| strokeColor      | 定义路径描边颜色，如不定义则不显示描边               |
| strokeWidth      | 描边宽度                                             |
| strokeAlpha      | 描边的透明度                                         |
| trimPathStart    | 从路径起始位置截断路径的比率，取值范围0~1            |
| trimPathEnd      | 从路径结束位置截断路径的比率，取值范围0~1            |
| trimPathOffset   | 设置路径截取的范围                                   |
| strokeLineCap    | 设置路径线帽的形状，取值为 butt，round，square       |
| strokeLineJoin   | 设置路径交界处的连接方式，取值为 miter，round，bevel |
| strokeMiterLimit | 设置斜角的上限                                       |

### pathData 属性

pathData 是 path 标签的属性，属性内容包含了 path 的绘制信息，以下是 pathData 的举出语法：

| 关键字 | 等价函数                                              | 含义                                 |
| ------ | ----------------------------------------------------- | ------------------------------------ |
| M      | move_to(X,Y)                                          | 将画笔移动到指定的坐标位置           |
| L      | line_to(X,Y)                                          | 画直线到指定的坐标位置               |
| H      | horizontal_line_to(x)                                 | 画水平线到指定的 X坐标位置           |
| V      | vertical_line_to(Y)                                   | 画垂直线到指定的 Y坐标位置           |
| C      | curve_to(X1,Y1,X2,Y2,EndX,EndY)                       | 三次贝塞尔曲线                       |
| S      | smooth_corve_to(X1,Y1,X2,Y2,EndX,EndY)                | 三次贝塞尔曲线，更顺滑               |
| Q      | quadratic_belzier_curve(X,Y,EndX,EndY)                | 两次贝塞尔曲线                       |
| T      | smooth_quadratic_belzier_curve_to(X,Y,EndX,EndY)      | 两次贝塞尔曲线，更顺滑               |
| A      | elliptical_arc(RX, RY, XRotation, Flag1, Flag2, X, Y) | 弧线                                 |
| Z      | closePath                                             | 关闭路径（会自动绘制连接起点和终点） |

## group 标签

group 标签可以将多个 path 集中处理，以下是 group 支持的属性：

| 属性       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| name       | group 名字                                                   |
| rotation   | 该 group 的路径旋转多少度                                    |
| pivotX     | 缩放和旋转该 group 时候的 X 参考点，是相对于 vector 的 viewport 属性来指定的 |
| pivotY     | 缩放和旋转该 group 时候的 Y 参考点，是相对于 vector 的 viewport 属性来指定的 |
| scaleX     | 定义 X 轴的缩放倍数                                          |
| scaleY     | 定义 Y 轴的缩放倍数                                          |
| translateX | 定义 X 轴的位置，是相对于 vector 的 viewport 属性来指定的    |
| translateY | 定义 Y 轴的位置，是相对于 vector 的 viewport 属性来指定的    |

# 进阶应用

## 动画

待补充

# 高级应用

## 设置颜色

Drawable 是可以直接设置颜色的，有两个接口，`setTint` 和 `setColorFilter` ，但是都有很大的局限性，只能设置纯色图标，一旦遇到彩色的图标就无能为力了。

但是仔细想想，VectorDrawable 是由很多条 Path 组成的，每条 Path 都有自己的 name，而且 VectorDrawable 本来就是 xml 格式，那么我们完全可以通过解析 VectorDrawable 文件来提取出每条 Path 对象，然后更改其属性。

具体怎么把 xml 转化为 Drawable，则需要涉及一点自定义 Drawable ，以及自定义 View 的知识。

**大概说一下思路。**

首先利用 Resource 的接口，将 VectorDrawable 作为一个 xml 读取出来，然后解析出 pathData、clipPath 等数据，根据其语法，将其转化为 Android 的 Path 类然后将其渲染到 Canvas 上。这部分十分繁琐，好在已经有[前辈写好了这部分代码](https://github.com/devendroid/VectorChildFinder)，我在他的基础上增加了一个接口，使得这个类库更具有普适性。[改动后的代码在这里](https://gitlab.gz.cvte.cn/fengxuchao/vectorchildfinder)

### 类库用法

```java
VectorChildFinder vector = new VectorChildFinder(this, R.drawable.my_vector, imageView);

VectorDrawableCompat.VFullPath path1 = vector.findPathByName("path1");
path1.setFillColor(Color.RED);

VectorDrawableCompat.VGroup group1 = vector.findGroupByName("group1");
group1.setTranslateX(10);

imageView.invalidate();
```

其中 `VPath` 类代表 path 标签，可以操作 path 的一系列属性，颜色、描边等等。而 `VGroup` 则代表 group 标签，可以对 group 内的 path 做位移、旋转等操作。

可以实现以下效果：


![](https://github.com/devendroid/VectorChildFinder/raw/master/assets/vector1.0.0.gif)