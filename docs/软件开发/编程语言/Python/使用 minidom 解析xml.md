# 概念

## DOM

`Document Object Model`的简称，是使用一个对象树的形式来表示一个 xml 文档

## 元素和节点

### 元素

元素就是标记，成对出现。xml 文档就是由元素组成的，但是元素与元素之间可以有文本。元素的内容也是文本。

在 **minidom** 中定义了许多节点，元素也属于节点的一种，他不是叶子节点（存在子节点），也有一些叶子节点（如文本节点：下面不再有子节点）。

**注：**元素与元素之间的所有东西，都是子节点，知识类型不同：文本内容是文本节点，还有元素节点等等

# 图解

![1](https://ws3.sinaimg.cn/large/006tNc79ly1fmdxscj4xbj315s0y448c.jpg)

# 实例

试着解析这个 xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<catalog>
    <maxid>4</maxid>
    <item id="1">
        <caption>Python</caption>
        <item id="4">
            <caption>测试</caption>
        </item>
    </item>
    <item id="2">
        <caption>Zope</caption>
    </item>
</catalog>
```

## 得到 dom 对象

```python
from xml.dom import minidom as mini
dom = mini.parse('filePath')
```

## 得到文档元素对象

```python
root = dom.documentElement
```

根元素(catalog)

## 节点属性

每一个节点都有它的 `nodeName`，`nodeValue`，`nodeType` 属性。

* `nodeName` 为节点名字
* `nodeValue` 为节点的值，只对文本节点有效
* `nodeType` 节点类型，有这么几种
  * ATTRIBUTE_NODE
  * CDATA_SECTION_NODE
  * COMMENT_NODE
  * DOCUMENT_FRAGMENT_NODE
  * DOCUMENT_NODE
  * DOCUMENT_TYPE_NODE
  * ELEMENT_NODE
  * ENTITY_NODE
  * ENTITY_REFERENCE_NODE
  * NOTATION_NODE
  * PROCESSING_INSTRUCTION_NODE
  * TEXT_NODE

catalog 是 ELEMENT_NODE 类型

## 子元素、子节点的访问

访问子元素、子节点的方法很多

### 知道元素名

使用 `getElementsByTagName(name)` 方法

```python
root.getElementsByTagName('maxid')
```

返回一个列表。

### 得到全部子节点（包括元素）

使用 `childNodes` 属性

```python
root.childNodes

[<DOM Text node "'\n    '">, <DOM Element: maxid at 0x1046a06d0>, <DOM Text node "'\n    '">, <DOM Element: item at 0x1046a0800>, <DOM Text node "'\n    '">, <DOM Element: item at 0x1046a0a60>, <DOM Text node "'\n'">]
```

从输出知道，两个标记间的内容都被视为文本节点。

每个节点都是一个对象，不同的节点对象有不同的属性和方法

### 小结

`getElementsByTagName `可以搜索当前元素的所有子元素，而 `childNodes` 只保存了当前元素的第一层子节点。

所以可以便利 childNodes 来访问每一个节点，根据 nodeType 来得到不同的内容。比如：打印出所有的元素的名字

```python
for node in root.childNodes:
    if node.nodeType == node.ELEMENT_NODE:
        print(node.nodeName)
```

### 属性

如果一个元素有属性，可以使用`getAttribute`方法，如：

```python
itemList = root.getElementsByTagName('item')
item = itemList[0]
item.getAttribute('id')

1
```



简单小结如何使用 minidom 读取 xml 信息

1. 导入依赖，生成 dom 对象
2. 得到文档对象
3. 通过`getElementsByTagName()`方法和`childNodes`属性（或其他）找到要处理的元素
4. 取的元素下文本节点的内容



[参考](http://boyeestudio.cnblogs.com/archive/2005/08/16/216408.html)