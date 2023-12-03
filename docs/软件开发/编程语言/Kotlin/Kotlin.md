# () -> Unit 和 lambda 的区别

() -> Unit 是 lambda 的子集（lambda 的一种形式）

() -> Unit 定义了 lambda 的形式：无参，返回值为 Unit

## Lambda 的定义

lambda 是一个函数表达式，不能显示使用 return，函数的最后一句代码的返回值就是 lambda 的返回值

如果直接使用 lambda

```kotlin
val lam = {
    print('Test')
}
```

那么这个表达式的返回值就是 Any ，即形式为 () -> Any

如果不显示声明 lambda 的变量形式为 () -> Unit (`val lam: (() -> Unit)`)，也想得到同样的效果，可以这么写

```kotlin
val lam = {
    print('Test')
    Unit
}
```

