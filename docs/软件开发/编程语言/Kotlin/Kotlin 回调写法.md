---
title: Kotlin 回调写法
date: 2018-1-14 23:44:25
categories: [笔记]
tags: [Kotlin]
---
> Kotlin 有两种回调写法，一种接近 Java，一种则是 Kotlin 风格，个人感觉 有点像Lambda

# Java 风格

先定义回调接口

```kotlin
class P {
  lateinit var listener: TL
  fun setListener(l: TL) {
    listener = l
  }
  interface TL {
    fun tl(s: String)
  }
  
  fun doSomethine() {
    listener.tl("Hello")
  }
}
```

调用方式：

```kotlin
fun main() {
  val p = P()
  p.setListener(object: P.TL {
    override fun tl(s: String) {
      println(s)
    }
  })
  p.test()
}
```

# Kotlin风格

重点来了！

Kotlin 不需要再定义接口了

被调用方

```kotlin
class P {
  lateinit var listener: (String) -> Unit
  fun setListener(l: (String) -> Unit) {
    listener = l
  }
  
  fun test() {
    listener.invoke("Hello")
    //or
    //listener("Hello")
    // *.() == *.invoke()
  }
}
```

调用方：

```kotlin
fun main() {
  val p = Person
  p.setListener {
    println(it)//只有一个参数用 it代表
  }
  p.test()
}
```

如果有多个参数：

```kotlin
  p.setListener { i, s ->
    println(i + s)//参数名自己定义
  }
```

