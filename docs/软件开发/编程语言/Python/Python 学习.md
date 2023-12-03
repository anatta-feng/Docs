---
title: Python 学习
date: 2017-12-18 18:00
categories:
  - 笔记
tags:
  - python
---

> 最近需要弄一点机器学习，学点 Python

Python 是一门动态语言，言下之意就是弱类型

# 基本类型

## 布尔值

布尔值为首字母大写 `True` `False`

### 布尔值运算

与或非

`and` `or` `not`

# list 和 tuple

##list 就是普通的 list

* 初始化

  * ```python
    test = ['1', '2', '3']
    ```

* 相当于没有泛型的 list，什么都能放

* 索引从0开始，负值索引代表倒序：-1，倒数第一个

* 访问 `test[1]`

* 追加 `test.append('11')`

* 插入 `test.insert('231')`

* 移除末尾的元素 `test.pop()`

* 移除指定位置的元素 `test.pop(3)`

* 替换 `test[1] = "333"`

## tuple 叫元组，和 list 十分相似，但是一旦初始化就不能改变

不知容量不能改变，元素也不能改变

* 初始化 `tuple("11", "33")`

## 不同

两者声明的时候一个是 `[]`, 一个是 `()`

# 逻辑判断

```python
age = 3
if age >+ 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
```

即：

```python
if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4>
```

`elif` 是 `else if` 的缩写

## 简写

```python
if x:
    print('True')
```

只要`x`是非零数值、非空字符串、非空list等，就判断为`True`，否则为`False`。

# 循环

两种循环：`for` `while`

## for

for 循环是 for in 格式，`Kotlin`就是这种格式

```python
names = ["Danny", "Bob"]
for name in names:
    prinf(name)
```

所以`for x in ...`循环就是把每个元素代入变量`x`，然后执行缩进块的语句。

还有一种情况，我们需要循环 n 次

```python
for i in range(10):
    print(i)
```

`range(n)`可以生成一个 n-1的整数序列

说到这里就想起 **Kotlin** 里

是这样的

```kotlin
for(i in 0 until 10) {
  // 0 ~ 9
}
for(i in 0..10) {
  // 0~10
}
```

## while 循环

就是普通的 while 循环了

# 接口

## enumerate()

enumerate 是 python 的内置函数

enumerate 的含义是枚举、列举的意思

对于一个可迭代的/可遍历的对象，enumerate 将其组成一个索引序列，利用它可以同时获取索引值和值

enumerate 多用于在 for 循环中得到计数

```python
for index, item in enumerate(items):
    print('current is %d line, value is %s.' % (index, item))
```

# 函数式编程

## 高阶函数

> * 变量可以指向函数
> * 函数名也是变量
>   * 函数名其实就是指向函数的变量
> * 传入函数
>   * 变量可以接受函数，函数的参数也能接受函数

### map/reduce

### filter

### sorted

## 返回函数

## 匿名函数



## 装饰器

##偏函数