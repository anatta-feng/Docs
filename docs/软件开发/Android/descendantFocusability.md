# android:descendantFocusability 用法

* beforeDescendants：viewgroup 会优先其子类控件而获取到焦点
* afterDescendants：viewgroup 只有当其子类控件不需要获取焦点时才获取焦点
* blocksDescendants：viewgroup 会覆盖子类控件而直接获得焦点