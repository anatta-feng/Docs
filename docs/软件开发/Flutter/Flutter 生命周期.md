# App 生命周期

WidgetsBindingObserver 有一个回调：didChangeAppLifecycleState，会在 App 生命周期变化的时候回调一个参数：AppLifecycleState，值为：

- resumed
- inactive
- paused
- detached

```
WidgetsBinding.instance.addObserver(WidgetsBindingObserver)
```

# 路由变化

MaterialApp 有一个参数 `navigatorObservers`，接收一个 NavigatorObserver 数组，在路由的每一次变化都会回调当前路由和前一个路由。

# 路由设置不保存状态
MaterialApp 有一个参数：onGenerateRoute，使用这个参数的话就不能用 routes 了，在构建 Route 的时候将 maintainState 设置为 false 即可。如：

```dart
onGenerateRoute: (RouteSettings settings) {  
	if (settings.name == '/') {  
		return new MaterialPageRoute<Null>(  
		  settings: settings,  
		  builder: (_) => new MyApp(),  
		  maintainState: false,  
		);  
	}  
	if (settings.name == '/s') {  
		return new MaterialPageRoute<Null>(  
		  settings: settings,  
		  builder: (_) => new MySecondPage(),  
		  maintainState: true,  
	);  
}
```

你需要知道每个通过路由展示在界面的上的 Page 或 PopupWindow,都有两个状态值：

-   opaque：是否是全屏不透明的，当一个组件`opaque=true`时，那么可以认为它之下的组件都不需要再绘制了。一般情况下，一个 Page 的`opaque=true`， 不是全屏的 PopupWindow `opaque=false`。
-   maintainState：一个已经不可见(被上面的盖住完全看不到啦~)的组件，是否还需要保存状态。当`maintainState=true`时，会让这个组件继续活着，但不会再进行绘制操作了。如果`maintainState=false`，则不会继续维持这个 Widget 的生存状态了。

# 在路由返回后做点事情

做法和接收路由返回参数一致

```dart
final result = await Navigator.push(
	context,
	MaterialPageRoute(builder: (context) => SelectionScreen()),
);
// do something
```

# 监听 Widget 是否可见

使用 visibility_detector 监听。