---
title: String方法中 == 与 equals
date: 2016-9-2 11:55
categories:
  - 笔记
tags:
  - Java
---
>== 是比较当前对象和传进来的对象是不是一个对象


首先看一段代码：

	public class Test {

		public static void main(String[] args) {

			String a1 = new String("str");
			String a2 = new String("Str");
	
			System.out.println(a1 == a2);
			Systen,out.println(a1.equals(a2));
			
			String a3 = "str1";
			String a4 = "str1";
			
			System.out.println(a3 == a4);
			System.out.println(a3.equals(a4));
	
		}
	}

输出结果：

<!-- more -->

	false
	true
	
	true
	true

## 在第一种情况（用 new 实例化进行赋值） ##

在 Object 类中 equals 方法的源代码是

	public boolean equals(Object obj) {
	   return (this == obj);
	}

在 Object 类中 equals 与 == 是等效的，但是 String 重写了 equals 方法，所以输出 true。

== 比较对象的引用是否相同；equals 比较对象中的值是否相同

## 在第二种情况下（直接自动装箱实例化赋值） ##

因为 Java 对 String 不变类型有优化，当用 = 直接赋值的时候会在内存中先搜索是否存在相同字符串，如果存在则将引用指向该内存区，所以 == 返回值为 true。由此可得，当采用 new 进行实例化时会重新开辟一块内存空间，会导致引用不同， == 返回值false。