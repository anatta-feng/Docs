> 今早群里在讨论 AIDL 的问题，我突然想到一个问题：客户端通过 AIDL 传递给服务端一个非原语参数对象，然后在客户端改变这个参数，服务端会跟着变化吗？

了解一点 Binder 原理的同学，应该都会觉得不可能，因为 Binder 是通过调用时将内存变化同步给被调用方，从而实现的跨进程通信。改变数据类的属性时，压根都没有调用 Binder，怎么可能同步。一开始我也是这么想的，不过深入研究后却发现了一点不一样东西。

# 官方文档

看到传递非原语参数，应该第一时间就想到 `in`，`out`，`inout` 这三个标记，查了一下官方文档，描述很简单

> 所有非原语参数均需要指示数据走向的方向标记。这类标记可以是 in、out 或 inout（见下方示例）。原语默认为 in，不能是其他方向。


这句话里对于我们这个问题有用的线索就只有一个：**数据走向**。

那么让我们大胆的猜测一下，这个数据走向是否指的就是该参数内部的数据流向？如果是这样，那么数据走向应该是这样的：

- `in`：参数可以从客户端流向服务端
- `out`：参数可以从服务端流向客户端
- `inout`：参数可以在客户端和服务端互相流动

从客户端**流向**服务端比较好理解，就是**传参**，但是从服务端**流向**客户端到底是什么意思？

写一个 Demo 验证下：

# Demo 代码

## JavaBean

```java
public class Person implements Parcelable {
  private String name;
	private String age;

  public Person() {
  }

  public Person(String name, String age) {
    this.name = name;
		this.age = age;
  }

  protected Person(Parcel in) {
    name = in.readString();
		age = in.readString();
  }
}
```

## AIDL:

```java
interface IYaYaInterface {
  void setPersonIn(in Person person);
  void setPersonOut(out Person person);
  void setPersonInOut(inout Person person);
  void changePerson();
  void personChanged();
  Person getPerson();
}
```

## Service:

```java
class MyYaYa extends IYaYaInterface.Stub {

  @Override
  public void setPersonIn(Person person) throws RemoteException {
    Log.d(TAG, "setPersonIn1: " + person);
    mPerson = person;
    mPerson.setPrice("666666");
    Log.d(TAG, "setPersonIn2: " + person);
  }

  @Override
  public void setPersonOut(Person person) throws RemoteException {
    Log.d(TAG, "setPersonOut1: " + person);
    mPerson = person;
    mPerson.setPrice("666666");
    Log.d(TAG, "setPersonOut2: " + person);
  }

  @Override
  public void setPersonInOut(Person person) throws RemoteException {
    Log.d(TAG, "setPersonInOut1: " + person);
    mPerson = person;
    mPerson.setPrice("666666");
    Log.d(TAG, "setPersonInOut2: " + person);
  }

  @Override
  public void changePerson() throws RemoteException {
    mPerson.setName("CCCCCCC");
    Log.d(TAG, "changePerson: " + mPerson);
  }

  @Override
  public void personChanged() throws RemoteException {
    Log.d(TAG, "personChanged: " + mPerson);
  }

  @Override
  public Person getPerson() throws RemoteException {
    return mPerson;
  }
}
```

## 客户端:

```java
findViewById(R.id.in).setOnClickListener(v -> {
  try {
    Person in = new Person("In", "1");
    Log.d(TAG, "in1: " + in);
    mService.setPersonIn(in);
    Log.d(TAG, "in2: " + in);
  } catch (RemoteException e) {
    e.printStackTrace();
  }
});

findViewById(R.id.out).setOnClickListener(v -> {
  try {
    Person out = new Person("Out", "1");
    Log.d(TAG, "out1: " + out);
    mService.setPersonOut(out);
    Log.d(TAG, "out2: " + out);
  } catch (RemoteException e) {
    e.printStackTrace();
  }
});

findViewById(R.id.inout).setOnClickListener(v -> {
  try {
    Person inOut = new Person("InOut", "1");
    Log.d(TAG, "inOut1: " + inOut);
    mService.setPersonInOut(inOut);
    Log.d(TAG, "inOut2: " + inOut);
  } catch (RemoteException e) {
    e.printStackTrace();
  }
});
```

从代码可以知道，我们分别定义了使用 `in` ，`out`，`inout` 修饰的三个方法，然后在客户端分别调用了这三个方法，服务端则在收到对象后马上修改对象的属性。我们看一下运行起来的 Log，观察下**数据流向**。

# Log 验证

## 服务端 Log

```log
D/AIDLDebug: setPersonIn1: Person{name='In', age='1'}
D/AIDLDebug: setPersonIn2: Person{name='In', age='666666'}
D/AIDLDebug: setPersonOut1: Person{name='null', age='null'}
D/AIDLDebug: setPersonOut2: Person{name='null', age='666666'}
D/AIDLDebug: setPersonInOut1: Person{name='InOut', age='1'}
D/AIDLDebug: setPersonInOut2: Person{name='InOut', age='666666'}
```

可以看到 `in` 和 `inout` 服务端都是可以接收到**完整的属性**，而 `out` 则服务端**完全接收不到任何属性**。

这验证了一部分我们的猜想：`in` 和 `inout` 可以从客户端传递参数中的属性到服务端，而 `out` 则不能从客户端传递参数的属性到服务端。

接着验证数据从服务端流向客户端，我们在服务端中修改了参数的属性，看一下客户端的对象属性有没有跟随变化：

## 客户端

```java
D/AIDLDebug: in1: Person{name='In', age='1'}
D/AIDLDebug: in2: Person{name='In', age='1'}
D/AIDLDebug: out1: Person{name='Out', age='1'}
D/AIDLDebug: out2: Person{name='null', age='666666'}
D/AIDLDebug: inOut1: Person{name='InOut', age='1'}
D/AIDLDebug: inOut2: Person{name='InOut', age='666666'}
```

Log 证实了我们的猜想，in 的对象属性完全没有发生变化，而 out 和 inout 都同步了服务端的修改。

至此，我们的猜想已经得到了验证，最后还剩下一个问题，就是 in out inout 的生命周期，也就是说在该**函数作用域**之外，同步是否还会生效？修改我们的代码：

# 验证生命周期

## 代码修改

### 客户端

```java
findViewById(R.id.in).setOnClickListener(v -> {
  try {
    Person in = new Person("In", "1");
    Log.d(TAG, "in1: " + in);
    mService.setPersonIn(in);
    Log.d(TAG, "in2: " + in);
    in.setPrice("3");
    mService.personChanged();
  } catch (RemoteException e) {
    e.printStackTrace();
  }
});

findViewById(R.id.out).setOnClickListener(v -> {
  try {
    Person out = new Person("Out", "1");
    Log.d(TAG, "out1: " + out);
    mService.setPersonOut(out);
    mService.changePerson();
    Log.d(TAG, "out2: " + out);
  } catch (RemoteException e) {
    e.printStackTrace();
  }
});
```

有两点修改：

1. `in` 修饰符在 `setPersonIn()` 之后，客户端自行修改了对象参数，然后调用 `personChanged()` 在服务端打印对象
2. `out` 修饰符在 `setPersonOut()` 之后，调用 `changePerson()` 改变服务端的对象属性，然后打印客户端的对象

## Log 验证

### 服务端

```log
D/AIDLDebug: setPersonIn1: Person{name='In', age='1'}
D/AIDLDebug: setPersonIn2: Person{name='In', age='666666'}
D/AIDLDebug: personChanged: Person{name='In', age='666666'}
D/AIDLDebug: setPersonOut1: Person{name='null', age='null'}
D/AIDLDebug: setPersonOut2: Person{name='null', age='666666'}
D/AIDLDebug: changePerson: Person{name='CCCCCCC', age='666666'}
```

### 客户端

```log
D/AIDLDebug: in1: Person{name='In', age='1'}
D/AIDLDebug: in2: Person{name='In', age='1'}
D/AIDLDebug: out1: Person{name='Out', age='1'}
D/AIDLDebug: out2: Person{name='null', age='666666'}
```

可以看到，数据的流动不能始终保持，在离开了相应的**函数作用域**之后，流动就会失效。

# 结论

方向标记规定了在跨进程通信中**参数内部的数据流向**：

- `in`：表示参数内部数据只能从客户端流向服务端
- `out`：表示参数内部数据只能从服务端流向 客户端
- `inout`：表示参数内部数据可以在客户端和服务端之间互相流动

而且，该流动特性只在被修饰的**函数作用域**内有效，一旦离开该作用域，流动特性就会失效。

# 源码：

通过源码印证下我们的结论，看一下生成的 AIDL java 文件：

```java
@Override public boolean onTransact(int code, android.os.Parcel data, android.os.Parcel reply, int flags) throws android.os.RemoteException
{
  java.lang.String descriptor = DESCRIPTOR;
  switch (code)
  {
    case INTERFACE_TRANSACTION:
    {
      reply.writeString(descriptor);
      return true;
    }
    case TRANSACTION_setPersonIn:
    {
      data.enforceInterface(descriptor);
      // 可以看到 _arg0 就是客户端传进来的参数
      com.yaya.服务端.Person _arg0;
      if ((0!=data.readInt())) {
        _arg0 = com.yaya.服务端.Person.CREATOR.createFromParcel(data);
      }
      else {
        _arg0 = null;
      }
      // 在这里将参数传递给服务端
      this.setPersonIn(_arg0);
      reply.writeNoException();
      return true;
    }
    case TRANSACTION_setPersonOut:
    {
      data.enforceInterface(descriptor);
      com.yaya.服务端.Person _arg0;
      // out 根本没有从客户端拿数据，而是 new 了一个新对象
      _arg0 = new com.yaya.服务端.Person();
      // 然后将新对象传递给服务端
      this.setPersonOut(_arg0);
      reply.writeNoException();
      // 在这里将服务端的对象重新写到序列化中，可以返给客户端。如果在 setPersonOut 方法中改变了参数，会在这里传递给客户端
      if ((_arg0!=null)) {
        reply.writeInt(1);
        _arg0.writeToParcel(reply, android.os.Parcelable.PARCELABLE_WRITE_RETURN_VALUE);
      }
      else {
        reply.writeInt(0);
      }
      return true;
    }
    case TRANSACTION_setPersonInOut:
    {
      data.enforceInterface(descriptor);
      // inout 在这里获取了客户端传递的参数
      com.yaya.服务端.Person _arg0;
      if ((0!=data.readInt())) {
        _arg0 = com.yaya.服务端.Person.CREATOR.createFromParcel(data);
      }
      else {
        _arg0 = null;
      }
      // 在这里传递给服务端
      this.setPersonInOut(_arg0);
      reply.writeNoException();
      // 在这里回写给客户端
      if ((_arg0!=null)) {
        reply.writeInt(1);
        _arg0.writeToParcel(reply, android.os.Parcelable.PARCELABLE_WRITE_RETURN_VALUE);
      }
      else {
        reply.writeInt(0);
      }
      return true;
    }
    case TRANSACTION_changePerson:
    {
      data.enforceInterface(descriptor);
      this.changePerson();
      reply.writeNoException();
      return true;
    }
    case TRANSACTION_personChanged:
    {
      data.enforceInterface(descriptor);
      this.personChanged();
      reply.writeNoException();
      return true;
    }
    case TRANSACTION_getPerson:
    {
      data.enforceInterface(descriptor);
      com.yaya.服务端.Person _result = this.getPerson();
      reply.writeNoException();
      if ((_result!=null)) {
        reply.writeInt(1);
        _result.writeToParcel(reply, android.os.Parcelable.PARCELABLE_WRITE_RETURN_VALUE);
      }
      else {
        reply.writeInt(0);
      }
      return true;
    }
    default:
    {
      return super.onTransact(code, data, reply, flags);
    }
  }
}
```

参看源码中的**注释**，可以从代码层面印证我们的猜想，而且明确了为什么仅在**函数作用域**内才会流动。

# 注意：

如果你在 Android 11 上进行试验，可能会发现怎么都 bind 不上 Service：

```verilog
ActivityManager: Unable to start service Intent { act=com.yaya.server cmp=com.yaya.server/.YaYaService } U=0: not found
```

这是因为 Android 11 更新了隐私策略，默认情况下安装包之间互相是不可见的。