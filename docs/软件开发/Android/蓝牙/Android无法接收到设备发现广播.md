> Android 从6.0开始，加入了动态权限的机制。

蓝牙设备发现的一种实现方式，就是通过广播接收`android.bluetooth.device.action.FOUND` 来得知搜索到了蓝牙设备。

# 问题

但是在 Android 6.0以上，出现问题了，接收不到这个广播。

# 解决方案

在 Android 6.0 以上如果要查找周围 WIFI 和蓝牙设备，需要添加以下两个权限

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

然后在代码中添加 Runtime 权限申请

```java
if (Build.VERSION.SDK_INT >= 6.0) {
	ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, MY_PERMISSION_REQUEST_CONSTANT);
}
```

```java
public void onRequestPermissionsResult(int requestCode, String permissions[], int[] grantResults) {
    switch (requestCode) {
        case MY_PERMISSION_REQUEST_CONSTANT: {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
            }
            return;
        }
    }
}
```

---

当然，如果你是系统组的应用，那么不用理睬这两个权限。