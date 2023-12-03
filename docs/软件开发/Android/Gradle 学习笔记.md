---
title: Gradle 学习笔记
date: 2018-1-11 23:44:25
categories: [笔记]
tags: [Gradle, Groovy]
---
> 我是因为 Android 才接触的 Gradle，现在想要搭建出漂亮的工程，编译脚本一定要熟练

整理自：[Gradle 完整指南（Android）](https://www.jianshu.com/p/9df3c3b6067a)

首先，Gradle中非常重要的两个对象：**Project** 和 **Tase**。

一个项目至少有一个 Project，一个`build.gradle`代表一个 Project，每个 Project 中包含多个 Task，Task中又包含了许多 action，action 是一个代码块，里面包含了许多需要被执行的代码。

由于 build.gradle 文件中存在非常多的 task，所以需要一种逻辑（依赖逻辑）来保证 task 的执行顺序。几乎所有的 task 都需要依赖其他的 task 来执行，没有被依赖的 task 会首先执行。

# Gradle 的编译周期

三个阶段

* **初始化阶段**：创建 Project 对象，如果有多个 build.gradle，也会创建Project
* **配置阶段**：在这个阶段，会执行所有的编译脚本，同时还会创建 Project 中所有的 task，为最后一个阶段做准备
* **执行阶段**：这个阶段 Gradle 会根据传入的参数决定如何执行这些 task，action 的代码就在这个阶段执行

# 项目结构

一个 Gradle 项目，其基础结构：

![](https://ws2.sinaimg.cn/large/006tNc79ly1fnd1bhaftjj30fk0buabd.jpg)

项目中有一个`setting.gradle`、`build.gradle` 。Gradle Wrapper。

* **setting.gradle**：这个文件定义了哪些 module 应该被加入到编译过程
* **顶层build.gradle**：这个文件里的配置最终会被应用到所有的项目中。
  * 典型配置：

```groovy
buildscript {
    repositories {
        jcenter()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:1.2.3'
    }
}

allprojects{
    repositories{
        jcenter()
    }
}
```

1. **buildscript**：定义了 Android 编译工具的类路径。`repositories`中，`jcenter`是一个 Maven 仓库。
2. **allprojects**：定义的东西会被应用到所有的 module 中。

* **每个项目单独的 build.gradle：**针对每个 module 的配置，如果这里的定义和顶层文件有冲突，则覆盖顶层文件的定义

```groovy
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'

android {
    compileSdkVersion 26
    defaultConfig {
        applicationId "com.fxc.pics"
        minSdkVersion 21
        targetSdkVersion 26
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(include: ['*.jar'], dir: 'libs')
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jre7:$kotlin_version"
    implementation 'com.android.support:appcompat-v7:26.1.0'
}

```

* **apply plugin**： 应用了 Android 的 Gradle 插件
* **android**：关于 Android 的所有的特殊配置都在这里。是由 Android plugin 提供的
  * *defaultConfig*：程序的默认配置
  * *applicationId*：
* **buildTypes**：定义了编译类型，针对每个类型可以有不同的编译配置，不同的编译配置对应有不同的编译命令。默认有 debug、release
* **dependencies**：属于 Gradle 的依赖配置

# Android Tasks

有四个基本的 task，Android 继承他们并分别进行了自己的实现

* **assemble**：对所有的 buildType 生成 apk
* **clean**：移除所有的编译输出文件
* **check**：执行 lint 检测编译
* **build**：同时执行 assemble 和 check 命令

# Configuration

## BuildConfig

经常用的一个类**BuildConfig**，这个类是由 Gradle 生成的。

我们可以设置一些 key-value，这些值在不同的编译类型下不同。比如：

```groovy
    buildTypes {
        debug {
            buildConfigField "String", "API_URL", "\"debug\""
        }
        release {
            buildConfigField "String", "API_URL", "\"release\""
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
```

这样，在项目中就可以通过`BuildConfig.API_URL`来读取相应的值



还可以为不同的编译类型设置不同的资源文件：

```groovy
buildTypes {
  debug {
    resValue "string", "app_names", "debug"
  }
  release {
    resValue "string", "app_names", "release"
    minifyEnabled false
    proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
  }
}
```

这样就可以通过`R.string.app_names`来读取值

BuildConfig 还有更多妙用

## Repositories

代码仓库。我们平时的一些依赖就是在这里下载的。

Gradle 支持三种类型的仓库：Maven、lvy 和静态文件/文件夹

在编译的执行阶段会从仓库中取出需要的依赖

Gradle 支持多种 maven 仓库，共有的`jCenter`

私有仓库需要手动加入连接

```groovy
repositories {
  maven {
    url "http://test/com/maven"
  }
}
```

如果仓库有密码，也可以同时传入用户名和密码

```groovy
repositories {
  maven {
    url "http://test/com/maven"
    credentials {
      username 'user'
      password 'passwd'
    }
  }
}
```

也可以配置本地仓库

```groovy
repositories {
  flatDir {
    dirs 'aars' 
  }
}
```

## Dependencies

引用库的时候，每个库名称包含三个元素：组名，库名，版本号

```groovy
dependencies {
  implementation 'com.android.support:appcompat-v7:26.1.0'
}
```

### Local dependencies

####File dependencies 

通过 files() 方法可以添加文件依赖，如果有许多依赖，可以通过 fileTree() 方法添加一个文件夹，或者使用通配符

```groovy
dependencies {
  implementation fileTree(dir: 'libs', include: [*.jar])
}
```

#### Native libraries



# Build Type

Android 默认有 Debug 和 Release 两种编译类型，我们可以新建新的编译类型

```groovy
android {
  buildTypes {
    init.initWith(buildTypes.debug)
    init {
      debuggable = false
    }
  }
}
```

[Build Type 更多接口属性参考这里](https://www.jianshu.com/p/a0e8f1c756ed)

# Source sets

每当创建一个新的 buildType 的时候，Gradle 默认都会创建一个新的 source set。我们可以建立 main 文件夹同级的文件夹，根据编译类型的不同我们可以选择对某些源码进行替换

文件结构

![](https://ws2.sinaimg.cn/large/006tKfTcly1fnd3dfwbeuj30mo0t4n01.jpg)

**注意**：

* 文件夹要自己手动创建
* 如果要在不同的 source set 中添加替换的类，要在 main 中删除对应的类

# Product flavors

`Build Type`是根据一份代码编译一个程序的不同类型。`Product flavors`是根据同一份源码编译不同的程序（包名也不同）。

Build Type和Product flavors不一样，属性也不同。**所有的Product flavors版本和 defaultConfig 共享所有属性**

# Groovy 语法

## 变量

Groovy 是隐式变量类型，用 `def` 关键字引用

```groovy
def name = 'test'
```

单引号是纯粹的字符串，双引号可以支持**插值**操作，语法和 Kotlin 一样（或者说 Kotlin 和 Groovy 一样）

```groovy
def t = 'dd'
def tt = "Hello, $t!"
```

## 方法

和 Python 一样，通过`def`关键字声明方法，方法如果不指定返回值，默认返回最后一行代码的值

```groovy
def square(def num) {
  num * num
}
```

## 类

通过`class`关键字定义类

```groovy
class Test {
  String name
  String getName() {
    return name
  }
}
```

特性和 Kotlin 十分相似

* 在 Groovy 中，默认所有的类和方法都是 public，所有的类的字段都是 private
* 可以通过 `new` 获取类的实例，通过 def 接收对象引用
* Groovy 方法调用不需要括号，也不需要分号
* 可以直接通过属性名获取字段值，但是底层还是通过 get 方法拿到的

## map、collections

在 Groovy 中 List 的定义：

```groovy
List list = [1, 2, 3]
```

遍历列表：

```groovy
list.each() { element -> 
  println element
}
```

定义 map

```groovy
Map m = [name:'x', age:12]
```

获取 map 的值

```groovy
m.get('name')
m['name']
```

# Task

##创建一个 Task

```groovy
task hello {
  println 'Hello, world!'
}
```

运行

```shell
gradle hello
```

gradle 生命周期分为三步：初始化，配置，执行。上面的代码在配置过程就会执行。如果想要在执行阶段执行 task，应当：

```groovy
task hello << {
  println 'Hello, world!'
}
```

## 添加 action

task 包含 action

如果需要加入自己的 action，可以重写`doFirst()`和`doLast()`方法

```groovy
task hello {
  doLast {
    ...
  }
  doFirst {
    ...
  }
}
```

doLast 可以使用`<<`符号简化：

```groovy
task hello << {
  //
}
```



## Task 依赖

Task 的依赖关系有两种`mastRunAfter`和`dependsOn`

```groovy
task t1 << {
  //
}
task t2 << {
  //
}
t2.mustRunAfter t1
```

```groovy
task t1 << {
  //
}
task t2 << {
  //
}
t2.dependsOn t1
//获取
task t1 << {
  //
}
task t2(dependsOn t1) << {
  //
}
```

前者必须在参数中加入`gradle t2 t1`才能顺利执行，否则只会执行单独的任务，顺序是 t1，t2。

后者只需要执行`gradle t2`就可以同时执行两个任务，顺序是 t1， t2。