# ListView Item 选中变色效果

平常一般 Button，TextView 等，选中的时候变色，需要做 selector 图

```xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:state_focused="true"
        android:drawable="@drawable/.." />
  <item android:state_focused="false"
        android:drawable="@drawable/.." /> 
</selector>
```

在 ListView 里也是只需要一张 selector，设置到 Item 的根视图就行，但是又稍稍有点不一样

```xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:state_selected="true"
        android:drawable="@drawable/.." />
  <item android:state_selected="false"
        android:drawable="@drawable/.." /> 
</selector>
```

---

一般控件设置焦点与否，而像 ListView 这种，设置是否选择