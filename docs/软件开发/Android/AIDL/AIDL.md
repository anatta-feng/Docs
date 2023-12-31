> AIDL（Android 接口定义语言）， 可以利用它定义客户端与服务使用进程间通信 (IPC) 进行相互通信时都认可的编程接口。 在 Android 上，一个进程通常无法访问另一个进程的内存。 尽管如此，进程需要将其对象分解成操作系统能够识别的原语，并将对象编组成跨越边界的对象。 编写执行这一编组操作的代码是一项繁琐的工作，因此 Android 会使用 AIDL 来处理。

# 定义 AIDL 接口

必须使用 Java 编程语言语法在 `.aidl` 文件中定义 AIDL 接口，然后将它保存在托管服务的应用以及任何其他绑定到服务的应用的源代码（`src/` 目录）内。

##  创建 .aidl 文件

AIDL 使用简单语法，能通过可带参数和返回值的一个或多个方法来声明接口。 参数和返回值可以是任意类型，甚至可以是其他 AIDL 生成的接口。

> 所有非原语参数都需要指示数据走向的方向标记。可以是 `in`、`out` 或 `inout`



## 实现接口

## 向客户端公开该接口

