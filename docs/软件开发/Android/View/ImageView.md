# 解决 Android ImageView 用 setImageDrawable 方法图片缩小的问题

## 是否调整边框，以保持可绘制对象的原始比例 

`setAdjustViewBounds` 

## 控制图片如何 resized/moved 来匹对 ImageView 的 size


CENTER /center  按图片的原来 size 居中显示，当图片长 / 宽超过 View 的长 / 宽，则截取图片的居中部分显示

CENTER_CROP / centerCrop  按比例扩大图片的 size 居中显示，使得图片长 (宽) 等于或大于 View 的长 (宽)

CENTER_INSIDE / centerInside  将图片的内容完整居中显示，通过按比例缩小或原来的 size 使得图片长 / 宽等于或小于 View 的长 / 宽

FIT_CENTER / fitCenter  把图片按比例扩大 / 缩小到 View 的宽度，居中显示

FIT_END / fitEnd   把图片按比例扩大 / 缩小到 View 的宽度，显示在 View 的下部分位置

FIT_START / fitStart  把图片按比例扩大 / 缩小到 View 的宽度，显示在 View 的上部分位置

FIT_XY / fitXY  把图片不按比例扩大 / 缩小到 View 的大小显示

MATRIX / matrix 用矩阵来绘制，动态缩小放大图片来显示。