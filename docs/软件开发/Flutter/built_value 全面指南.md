# 序列化

```dart
static Task fromMap(Map<String, dynamic> map) => serializers.deserializeWith(serializer, map);  
  
Map<String, dynamic> toMap() {  
  return serializers.serializeWith(serializer, this);  
}  
  
static Task fromJson(String source) => serializers.deserializeWith(serializer, json.decode(source));  
  
String toJson()  {  
  return json.encode(serializers.serialize(this));  
}
```

# 序列化忽略指定属性

```dart
@nullable  
@BuiltValueField(serialize: false)  
Category get category;
```

# 属性不参与 compare

```dart
@BuiltValueField(compare: false) 
```

# 赋予默认值

```dart
static void _initializeBuilder(TaskBuilder builder) => builder  
  ..id = Uuid().v1()  
  ..status = TaskStatus.pending  
 ..createDate = DateTime.now().millisecondsSinceEpoch;
```

# 原理

是给工厂构造方法传入一个函数，然后具体的实现类在实例化自己时根据这个函数进行赋值