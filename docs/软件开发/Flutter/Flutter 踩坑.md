# 布局相关

## 添加 ListView 后报错：`Vertical viewport was given unbounded height.`

这是因为 `ListView` 在 `Column` 中，需要在 `ListView` 外围套一层 `Expanded`。