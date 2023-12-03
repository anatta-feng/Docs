# SharedUserId
> The name of a Linux user ID will be shared with other apps. By default, Android assigns each app its own unique user ID. However, if this attribute is set to the same val for two or more app, they will all share the same ID - provided that their certificate sets are identical. App with the same user ID can access each other's data and, if desired, run in the same process.  

> **翻译：** 将与其他应用共享的 Linux 用户 ID。默认情况下，Android 会给每个 app 分配一个独一无二的用户 ID。但是当两个或者更多的 App 的这个属性设置了同样的值（假设他们的签名文件是一致的）。有相同 user ID 的 app 将能互相访问数据，如果需要的话，也可以运行在同一个进程中。  
## Sample
> 每个 App 都会根据自己的包名在手机文件系统的/data/data/packageName/ 路径建立文件夹（需要 root 权限才可以浏览），用于存储数据。  
> 代码中通过 Context 操作 ID 资源的时候，相关文件都在此路径下。
> 正常情况下，不同的 app 无法互相访问其他 app 的文件夹，但是如果有相同的 uid，就可以互相访问了。
### A 应用获取 B 应用的data数据
```kotlin
class ActivityA : Activity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        ...
    }
    /**
     * 通过这个方法可以获取到目标 app 的 context， 就可以去访问目标 app 的数据。
     **/
    fun getTargetContext() : Context {
        val ctx = this.createPackageContext("packageName", flags)
        return ctx
    }
}
```
### A 应用获取 B 应用的资源文件
```kotlin
fun getTargetResource(ctx: Context) {
    val context = ctx.createPackageContext("packageName", flags)
    val res = context.resources
    val drawable_id = res.getIdentifier("resName", "drawable", "packageName")
    ...
}
```