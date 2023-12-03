在了解尾递归之前，先要了解**尾调用**。

# 什么是尾调用

尾调用指的是一个方法或者函数的调用在另一个方法或者函数的最后一条指令中进行。

例如：

```c
int a(int i) {
    a += 1;
    return b(a);
}
```

`b`函数的调用时`a`函数的最后一条语句，因为这是一个尾调用。如果在调用了`b`函数以后。`a`函数还执行了别的指令之后才返回的话，`b`就不是一次尾调用了。

# 尾调用优化

任何的尾调用，不只是尾递归，函数调用本身都可以被优化掉，变得跟goto操作一样。这就意味着，在函数调用前先把栈给设置好，调用完成后再恢复栈的这个操作（分别是prolog和epilog）可以被优化掉。

# 意义

如果没有尾调用优化，在递归中就会一直给方法栈中添加新的方法，如果经过了尾调用优化，那么就相当于开了一个新的方法栈。

## 优化前

| int  a(int data) {    |                                                              |
| --------------------- | ------------------------------------------------------------ |
| data = do_this(data); | push EIP ! prolog push data ! prolog jmp do_this ! call ... pop data ! epilog pop EIP ! epilog |
| return b(data);       | push EIP ! prolog push data ! prolog jmp do_that! call ... pop data ! epilog pop EIP ! epilog |
| }                     | pop data ! epilog pop EIP ! epilog                           |

## 优化后

| int func_a(int data) { |                                                              |
| ---------------------- | ------------------------------------------------------------ |
| data = do_this(data);  | push EIP ! prolog push data ! prolog jmp do_this ! call ... pop data ! epilog pop EIP ! epilog |
| return do_that(data);  | jmp do_that ! goto ...                                       |
| }                      | pop data ! epilog pop EIP ! epilog                           |

## 抽象成伪代码

### 优化前

```c
call factorial(3)
  call fact(3,1)
    call fact(2,3)
      call fact(1 6)
        call fact(0,6)
        return 6
      return 6
    return 6
  return 6
return 6
```

### 优化后

```c
call factorial(3)
  call fact(3,1)
  update variables with (2,3)
  update variables with (1,6)
  update variables with (0,6)
  return 6
return 6
```

---

但是 Java 并没有做尾调用优化，这是一个悲剧，智能程序猿自己用循环的方式来实现递归了。