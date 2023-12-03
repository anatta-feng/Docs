# MVP 架构

> MVP 架构是从 MVC 设计模式变种而来的

##  MVC

要了解 MVP 先要了解 MVC

MVC 是 Models Views Controller 的缩写

早期的 MVC 的分工是这样的

* Model，负责的是数据，这里的“数据“不仅局限于数据本身，还包括处理数据的逻辑。
* View，负责数据的表现形式，将数据及数据的变化呈现给用户
* Controller，负责用户的输入，将用户的命令转化为消息传递给 model 或 View，是一个翻译者

这是早期的分工，在后来的演进中 Controller 逐渐变为隔离 Model 和 View。

---

## MVP

MVP 是从 MVC 演化过来的，所以和 MVC 是十分相似的：Model，View，Presenter，其中 Presenter 的作用和 Controller 的作用是十分相似的，都是为了隔离 Model 和 View。

我来大概说一下

Model 负责数据的读取，View 负责数据的展示，Presenter 负责隔离并联系 Model 和 View

所以 Presenter 有 Model 和 View 的引用，View 和Model 有 Presenter 的引用。

这样，如果 View 发起一条获取数据的请求，会去调用 Presenter 的请求数据接口，在这个接口内部 Presenter 会去调用 Model 的函数，让 Model 去读取数据或进行业务逻辑操作，当逻辑完成后 Model 调用 Presenter 的接口，在这个接口内部 Presenter 会调用 View 的函数去展现这些数据。

这样就实现了业务逻辑与界面的的分离。这样确实逻辑上会清晰很多，后续维护也方便了，但是大家说的那么多的好处，我还是没能理解太多，或许是写得不够多吧，后续用 MVP 写一个项目大概就会懂了。

MVP 的缺点也有，就是以前在一个 Activity 中实现的东西，现在分成了三个类。其实就算不用 MVP 也没有人会傻到代码全写在一个类里吧。

不过搞清楚了 MVP，还是很开心的，果然自己动手写才是王道！