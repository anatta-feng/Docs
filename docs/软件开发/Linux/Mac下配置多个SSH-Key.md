有时我们一台**mac**上可能会对应多个**git**账号，这时就需要**mac**上面创建不同的**key**来对应不同的**git**账号。

闲言不语，直接说实现步骤：

#### 1.打开终端，前往.ssh目录

```
➜  cd .ssh
➜  .ssh 
```

#### 2.生成一个ssh-key

```
➜  ssh-keygen -t rsa -C "youremail@email.com"
```

后面填写的是你的邮箱账号

#### 3.自定义生成的key

如果我们 **Mac** 上面已经有了 **ssh-key** 再创建 **ssh-key** 的话，默认会在 **~/.ssh/** 目录下生成 **id_rsa** 和 **id_rsa.pub** 两个文件，如果不自定义，就会把原有的给覆盖掉。为了加以区分，我们需要自定义一下生成的 **key** 的名字,后面的**id_rsa_test_github**为你自定义的名字

```
Enter file in which to save the key (/Users/a-375/.ssh/id_rsa): id_rsa_test_github
```

#### 4.设置密码

需要输入两次密码,输入密码时是看不见的，这个密码在你提交代码到**Github**时会用到【注意：记住这个密码，最简单的方式就是设置的和**github**账户登入密码一样，容易记住】

```
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
```

#### 5.成功生成ssh-key

```
Your identification has been saved in /Users/xxx/.ssh/id_rsa_test_github.
Your public key has been saved in /Users/xxx/.ssh/id_rsa_test_github.pub.
The key fingerprint is:
SHA256:/e91V1xop8k8wowRYJeJmEUrTTda32Pgr+EXboCNl3g youremail@email.com
The key's randomart image is:
+---[RSA 2048]----+
|       =*o*o.    |
|      o+.*o= o . |
|      . + . o * o|
|       . . X B *.|
|        S * E O o|
|           = * o.|
|            + + +|
|             + .o|
|             .o  |
+----[SHA256]-----+
```

#### 6.将ssh-key添加到ssh-agent

**（1）**到上面这一步我们已经创建好了 **ssh-key**，此时还需要将新的 **ssh-key** 添加到**ssh agent** ，因为默认只读  **id_rsa**，首先查看一下已经添加进去的 **ssh-key**,当出现下面 这种情况是说明 **ssh agent** 里面并没有把我们新生产的 **ssh-key**添加进去

```
➜  ssh-add -l
The agent has no identities.
```

**（2）**可以选择把我们生成的 **ssh-key** 添加进去，也可以指定添加

```
//全部添加
ssh-add  

//指定添加（可以切换到.ssh下添加，也可以直接指定路径添加）
➜  .ssh ssh-add id_rsa_test_github                   
Enter passphrase for id_rsa_test_github: 
Identity added: id_rsa_test_github (id_rsa_test_github)
```

**（3）**这时输入下面指令就能看见我们添加进去的 **ssh-key**

```
ssh-add -l
```

### 接下来将我们配置好的ssh-key的公钥提交到github上并进行测试连接

在 **~/.ssh/** 目录下会生成 **id_rsa_test_github** 和 **id_rsa_test_github.pub** 私钥和公钥。 我们将 **id_rsa_test_github.pub** 中的内容粘帖到 **github** 的 **SSH-key** 的配置中，这里获取 **id_rsa_test_github.pub** 的内容可以使用终端也可以使用 **sublime** 或 **atom** 等一些编辑器。

# 注

这个操作会在重启后被重置，如果需要持久化，需要用到ssh config

在`~/.ssh/config`文件中添加

```
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_github
```

