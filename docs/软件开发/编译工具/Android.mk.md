# 分包配置
分包即是多个 `classes.dex` 。
在 Android.mk 中加入 `LOCAL_DEX_FLAGS = -—multi-dex`。