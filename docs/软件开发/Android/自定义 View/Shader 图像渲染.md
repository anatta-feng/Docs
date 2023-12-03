# Shader 图像渲染

Android 提供了 Shader 类用于渲染图像以及几何图形

> Shader 包含五个直接子类
>
> * BitmapShader
>   * 图像渲染
> * ComposeShader
>   * 混合渲染
> * LinearGradient
>   * 线性渲染
> * RadialGradient
>   * 环形渲染
> * SweepGradient
>   * 梯度渲染

## 使用方法

使用 Shader 类的时候首先需要构建需要的 Shader 对象，然后通过 Paint 的 `setShader()` 方法来设置渲染对象，最后用这个 Paint 去画图像就行了

## BitmapShader （图像渲染）

BitmapShader 的作用是将一张位图作为纹理来对某一区域进行填充。

BitmapShader 的构造器为

```java
public BitmapShader(Bitmap bitmap, Shader.TileMode tileX, Shader.TileMode tileY);
```

