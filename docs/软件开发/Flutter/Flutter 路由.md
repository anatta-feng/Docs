# 路由 1.0

Flutter 的整个路由系统是一个树形结构，其中分为两部分：

- Navigator
- Route

Navigator  是管理者，他管理许多路由，一个路由则是一个界面。

![图片](https://mmbiz.qpic.cn/mmbiz_png/0ib44Sib5AL7OEGgnnCroHxfkhnvFf8oevpiboGqnL6WBOgzFkmKWLqk3zzibnFcGLmt8Bib6segonEHlloib17KR5zg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

平时使用的时候似乎并没有用到路由，尤其使用 pushNamed 的时候，都是直接用的 Widget 就行，这样会导致理解上忽略了 Route 的存在。

```dart
MaterialApp(  
 title: "Timez",  
  onGenerateRoute: (settings) {  
    switch (settings.name) {  
      case Routes.index:  
        return MaterialWithModalsPageRoute(  
            builder: (context) => IndexRoute());
	}
  },  
  routes: {  
    Routes.categoryContent: (context) => CategoryContentRoute(),  
  },
);
```

其实 `routes` 内部的 Widget 最后都会包裹金 Route 里。

## 嵌套路由

给 Route 里的 Widget 中包含一个 Navigator，即可实现嵌套路由，实现不全屏的切换路由。

# 路由 2.0

觉得路由 2.0 的整个关系很乱。

根部是一个 Navigator，然后底下一堆 Pages，每个 Page 里面又是一个 Route。

路由 2.0 他的整个体系都换成状态描述了。开发者需要根据状态描述去构建整个路由的栈。至于嵌套路由，还是跟 1.0 一样，Navigator 嵌套 Navigator。

下次试试同时显示两个嵌套，看看 pop 的时候要怎么样。

至于说是用状态来描述路由栈，但是我觉得略微有点扯淡。因为路由复杂了之后，对于状态管理和状态与路由的对应关系，会非常变态。

简而言之，灵活性更强，但是也更加复杂。

如果有两个需要互相跳的界面，并且需要按序返回。比如说影片详情页和演员详情页，此时使用状态来管理路由栈，简直就是噩梦。