相信大家都碰过这个坑：

```java
String[] arr = new String[]{"12", "23", "54"};
List<String> list = Arrays.asList(arr);
list.add("A");
```

看上去没什么毛病，但是结果会抛异常：

```log
Exception in thread "main" java.lang.UnsupportedOperationException
	at java.util.AbstractList.add(AbstractList.java:148)
	at java.util.AbstractList.add(AbstractList.java:108)
	at com.android.signapk.A.main(A.java:36)
```

具体的原因看源码就可以了解了，这里想聊一聊这样**设计的目的**。

先说结论：经过 `Arrays.asList()` 转换后的 `List` 是和参数的数组**共享元素**的，也就是说，改变了 `List` 中 `index` 为 `0` 的元素后，`Array` 中 `index` 为 `0` 的元素，也会**同步变成改变后的元素**。

看 demo：

```java
String[] arr = new String[]{"12", "23", "54"};
List<String> list = Arrays.asList(arr);
System.out.println(Arrays.toString(arr));
System.out.println(list);
list.set(0, "21");
System.out.println(Arrays.toString(arr));
```

数组转成 `List` 后，对第一个元素进行改变，并且在改变前后打印最初的 Array，看看输出：

```log
[12, 23, 54]
[21, 23, 54]

Process finished with exit code 0
```

