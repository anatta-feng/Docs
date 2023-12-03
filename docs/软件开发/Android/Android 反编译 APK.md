# Android 反编译 APK

> 有时候看到别人的 APP 里有一些比较好的效果，自己想实现，却实现不好，这时候就需要去反编译一下别人的 App 看看源码
>
> **注 ** ：反编译前务必自己动脑动手想办法还原，反编译是最后动用的手段

## 工具

* [apktool](http://ibotpeaches.github.io/Apktool/install/)
* [dex2jar](https://github.com/pxb1988/dex2jar)
* [JD-GUI](http://jd.benow.ca/)

## ApkTool 在 Mac 上的配置

在官网下载两个文件，一个 jar 包，一个 shell 脚本，shell 脚本去掉所有的后缀，两个文件放入 ```/usr/local/bin/```目录下，然后修改文件可执行权限：```sudo chmod +x [file]```

### 用 ApkTool 反编译

命令 ```apktool d [apk file]```

可以直接 cd 进入 apk 所在的目录执行，也可以后面加上 apk 文件的绝对路径

### 用ApkTool 回编译

命令 ```apktool b [file]```

---

## dex2jar 反编译 java 源文件

用 dex2jar 反编译 java 源文件需要拿到 dex 文件，这个文件很好拿

将 apk 的后缀名改为 zip，用解压文件打开，就可以看到根目录下有一个 ```classes.dex```文件，将这个文件复制到 dex2jar 的目录下，然后在终端中执行```sh d2j-dex2jar.sh classes.dex```执行完毕会在目录下生成一个```classes_dex2jar.jar```文件，这时候只需要用 JD-GUI 打开这个 jar 文件就可以尽情的阅读源码了！

---

> 在执行 ```sh d2j-dex2jar.sh classes.dex```时可能会遇到 ```./d2j_invoke.sh: Permission denied``` 的问题，这是因为```d2j_invoke.sh```文件没有可执行权限，执行```sudo chmod +x d2j_invoke.sh```命令即可