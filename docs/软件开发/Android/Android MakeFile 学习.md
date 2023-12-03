---
title:Android MakeFile 学习
date:2017-10-3 20:05:25
categories:[笔记]
tags:[Make, Android]
---

# Make 简介

make 是一个项目构建工具，在大型软件项目中发挥着巨大的作用。

makefile 包含一组用来编译程序的规则，一项规则可分为三部分：target(目标), prereq(必要条件), command(执行的命令)。

# Sample

##格式

```makefile
target: prereq1 prereq2
	commands
```

## 一个最简单的 makefile

代码 main.c：

```c
#include <stdio.h>
int main(int argc, char** argv)
{
    printf("hello, makefile!\n");
}
```

对应的 makefile `hello-makefile`:

```makefile
#first makefile
hello-makefile: main.c
	gcc -o hello-makefile main.c
```



# Makefile 基本语法

## 从上而下的结构

默认从最上层的工作目标开始工作，把有些工作目标，比如`clean`工作放在文件的最底部

## 特殊符号

* 井号（#）用来表示注释
* 反斜杠（\）是续行符

## 通配符

与常用的 shell 通配符一致

星号`*`代表任意数量的任意字符 问号`?`代表任意一个字符

## .PHONY

假象工作目标,可以避免名字冲突. 比如:

```makefile
.PHONY: clean
clean:
	rm -f *.o
```

常用的`. PHONY`

```makefile
all 执行编译应用程序的所有工作
install 从已编译的二进制文件中进行程序的安装
clean 清除生成的二进制文件
distclean 清除所有生成的文件
TAGS 建立可供编辑器使用的标记表
check 执行与程序相关的任何测试
```

## 变量

```makefile
$(varlable)
${}
```

变量名称是单一字符的就不用括号了

## VPATH

告诉 make 如果在当前目录没有找到就去指定目录找:

```makefile
VPATH = src include
```

## C++标识

有时需要输入一些参数告诉 gcc 做一些事情, 加参数

```makefile
CPPFLAGS = -I include
```

## include 关键字

有时需要调用其他的 makefile, 只需要将其 include 加入即可

```makefile
include makefilePath
```

---

# 最后

怎么直接 make 命令就找到 makefile 文件我不知道，每次都是

```
www:makefile fxc$ make
make: *** No targets specified and no makefile found.  Stop.
```

但是查看 make 命令的用法：

`make [ -f makefile ][ options ] ... [ targets ] …`

于是就可以这样：

`make -f makefile`



# 备注

2017.12.31

今天遇到问题，make 的时候如果一行命令报错了，就会终止 make 的执行。

经过一番查找，找到了解决方法：

> 在命令前加上`-`就会在命令出错的时候继续执行下去

```makefile
test:
	-rm test
	-rm hello
```

这样，即使 test 不存在，也能继续执行

---

# 更新

= 是最基本的赋值
:= 是覆盖之前的值
?= 是如果没有被赋值过就赋予等号后面的值
+= 是添加等号后面的值

