# 什么是 GDB

GDB, 是 `The GNU Project Debugger` 的缩写, 是 Linux 下功能全面的调试工具。GDB 支持断点、单步执行、打印变量、观察变量、查看寄存器、查看堆栈等调试手段。在 Linux 环境软件开发中，GDB 是主要的调试工具，用来调试 C 和 C++ 程序。

# 怎么进入 GDB

如果需要调试程序，需要在编译程序时生成可 debug 的可执行程序`gcc -g -o debug debug.c`

然后输入`gdb debug`进入调试 debug 程序的界面

输入`run`运行待调试程序

输入`quit`退出 gdb

# GDB 常用命令

| 命令               | 简写形式     | 说明             |
| ---------------- | -------- | -------------- |
| list             | l        | 查看源码           |
| backtrace        | bt、where | 打印函数栈信息        |
| next             | n        | 执行下一行          |
| step             | s        | 一次执行一行，遇到函数会进入 |
| finish           |          | 运行到函数结束        |
| continue         | c        | 继续运行           |
| break            | b        | 设置断点           |
| info breakpoints |          | 显示断点信息         |
| delete           | d        | 删除断点           |
| print            | p        | 打印表达式的值        |
| run              | r        | 启动程序           |
| until            | u        | 执行到指定行         |
| info             | i        | 显示信息           |
| help             | h        | 帮助             |

待续。。。