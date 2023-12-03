# 静态使用

```xml
<?xml version="1.0" encoding="utf-8"?>

 <!-- 
    android:shape=["rectangle" | "oval" | "line" | "ring"]
    shape的形状，默认为矩形，可以设置为矩形（rectangle）、椭圆形(oval)、线(line)、环形(ring)

    下面的属性只有在android:shape="ring时可用：
    android:innerRadius           内环的半径。
    android:innerRadiusRatio    浮点型，以环的宽度比率来表示内环的半径，
    例如，如果android:innerRadiusRatio，表示内环半径等于环的宽度除以5，这个值是可以被覆盖的，默认为9.

    android:thickness              环的厚度
    android:thicknessRatio      浮点型，以环的宽度比率来表示环的厚度，例如，如果android:thicknessRatio="2"，
    那么环的厚度就等于环的宽度除以2。这个值是可以被android:thickness覆盖的，默认值是3.
    
    android:useLevel            boolean值，如果当做是LevelListDrawable使用时值为true，否则为false.
  -->
<shape xmlns:android="http://schemas.android.com/apk/res/android"
　　　　　　　android:shape="rectangle" >
    <!--宽度和高度 
        android:width   整型 宽度
        android:height  整型 高度
    -->
    <size
        android:width="50dp"
        android:height="50dp"/>

    <!--圆角
        android:radius              　　　   整型 半径
        android:topLeftRadius       　　  整型 左上角半径
        android:topRightRadius               整型 右上角半径
        android:bottomLeftRadius           整型 左下角半径
        android:bottomRightRadius         整型 右下角半径
    -->
    <corners
        android:radius="10dp"/><!-- 设置圆角半径，可以分别设置4个角 -->
    
    <!--渐变，这个设置之后一般就不要设置solid填充色了
        android:startColor  颜色值     起始颜色
        android:endColor    颜色值     结束颜色
        android:centerColor 整型       渐变中间颜色，即开始颜色与结束颜色之间的颜色
        android:angle       整型       
　　　　 渐变角度(PS：当angle=0时，渐变色是从左向右。 然后逆时针方向转，当angle=90时为从下往上。angle必须为45的整数倍)

        android:type        ["linear" | "radial" | "sweep"] 渐变类型(取值：linear、radial、sweep)
                            linear 线性渐变，这是默认设置
                            radial 放射性渐变，以开始色为中心。
                            sweep 扫描线式的渐变。

        android:useLevel     ["true" | "false"]              
　　　　 如果要使用LevelListDrawable对象，就要设置为true。设置为true无渐变。false有渐变色

        android:gradientRadius 整型   
        渐变色半径.当 android:type="radial" 时才使用。单独使用 android:type="radial"会报错。

        android:centerX      整型     渐变中心X点坐标的相对位置
        android:centerY      整型     渐变中心Y点坐标的相对位置
    -->
    <gradient
        android:startColor="@android:color/white"
        android:centerColor="@android:color/black"
        android:endColor="@android:color/black"
        android:useLevel="true"
        android:angle="45"
        android:type="radial"
        android:centerX="0"
        android:centerY="0"
        android:gradientRadius="90"/>
    
    <!-- 间隔
        内边距，即内容与边的距离 
        android:left        整型 左内边距
        android:top        整型 上内边距
        android:right      整型 右内边距
        android:bottom   整型 下内边距
    -->
    <padding
        android:left="5dp"
        android:top="5dp"
        android:right="5dp"
        android:bottom="5dp"/>
    
    <!--填充 
        android:color   颜色值 填充颜色
    -->
    <solid
        android:color="@android:color/white"/><!-- 填充的颜色 -->
    
    <!--描边 
        android:width          整型      描边的宽度
        android:color           颜色值    描边的颜色
        android:dashWidth   整型      表示描边的样式是虚线的宽度， 值为0时，表示为实线。值大于0则为虚线。
        android:dashGap     整型      表示描边为虚线时，虚线之间的间隔 即“ - - - - ”
    -->
    <stroke
        android:width="1dp" <!-- 边框宽度 -->
        android:color="@android:color/black"
        android:dashWidth="1dp"
        android:dashGap="2dp"/>
    
</shape>
```

#  动态创建

```java
View view = null;    // 这个view是你需要设置背景的view
int strokeWidth = 1;     // 1dp 边框宽度
int roundRadius = 5;     // 5dp 圆角半径
int strokeColor = Color.parseColor("#FFFF0000");//边框颜色
int fillColor = Color.parseColor("#FF00FF00"); //内部填充颜色

GradientDrawable gd = new GradientDrawable();//创建drawable
gd.setColor(fillColor);
gd.setCornerRadius(roundRadius);
gd.setStroke(strokeWidth, strokeColor);
gd.setGradientType(GradientDrawable.RECTANGLE);
view.setBackgroundDrawable(gd);
        
// 创建渐变的shape drawable
int colors[] = { 0xff255779 , 0xff3e7492, 0xffa6c0cd };//分别为开始颜色，中间夜色，结束颜色
GradientDrawable gradientDrawable = new GradientDrawable(GradientDrawable.Orientation.TOP_BOTTOM, colors);
view.setBackgroundDrawable(gd);
```

