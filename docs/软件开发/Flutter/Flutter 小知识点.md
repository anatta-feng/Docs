# List map 中 await async

```dart
ongoingTasks = await Future.wait(ongoingTasks.map((task) async {  
  var ongoing = await ongoingDataSource.getOngoingById(task.id);  
  return task.rebuild((t) => t  
  ..ongoing = ongoing.toBuilder());  
}));
```

# Widget 可见性

1. 可以通过 Stafule 来控制是否添加该 Widget
2. 通过 `Offstage` 来控制
```dart
Offstage(  
  offstage: task?.category == null,
  child: Text(task?.category?.name ?? ""),  
),
```

# async 和 async* 的区别

