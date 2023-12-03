# 构造函数

## 简写构造函数

```dart
MyHomePage(this.title) // 表示缩写
// 两个效果等同
MyHomePage(String title) {
  this.title = title;
}
```

## 可选参数

```dart
MyHomePage({this.title}) // 表示可选，并且可以赋予默认值

MyHomePage({this.title = "标题"})
```

## 初始化列表

构造函数题执行之前初始化实例变量，每个变量之间用逗号隔开

```dart
MyHomePage({this.title})
  : type = "主页",
    name = "as";
```

# List 相关

## List 的 map 遍历中获取 index

```dart
list.asMap().entries
  .map((e) => FlSpot(e.key.toDouble(), e.value))
  .toList();
```

先将 List 转为 Map，然后再通过 entries 来进行 map。

# 泛型相关

## 给函数单独添加泛型

```dart
T fold<T>(T initialValue, T combine(T previousValue, E element)) {  
  var value = initialValue;  
  for (E element in this) value = combine(value, element);  
  return value;  
}
```