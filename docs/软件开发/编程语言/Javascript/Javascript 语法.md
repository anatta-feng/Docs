# 三个点

扩展运算符（spread）是三个点（...)。相当于 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。

## 常用场景

### 函数接收参数

### 替代数组的 apply 方法

### 合并数组

```javascript
let a = [2, 3, 4]
let b = [1, ...a]
```

### 合并对象 / 解构赋值

```javascript
let s = {
	a: 1,
	b: 2,
	c: 3
}
let a = {
	s
}
let b = {
	...s
}
```

对象的解构赋值用于从一个对象取值，相当于将目标对象自身的所有可遍历的（enumerable）、但尚未被读取的属性，分配到指定的对象上面。所有的键和它们的值，都会拷贝到新对象上面。

```javascript
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x // 1
y // 2
z // { a: 3, b: 4 }
```

# 解构赋值


# 模板字符串

模板字面量 是允许嵌入表达式的字符串字面量。你可以使用多行字符串和字符串插值功能。它们在ES2015规范的先前版本中被称为“模板字符串”。

## 语法

`string text`

`string text line 1
 string text line 2`

`string text ${expression} string text`

tag \`string text ${expression} string text\`