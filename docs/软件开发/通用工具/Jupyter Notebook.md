最近手上收集了一些数据，想要分析一下。然后想起来很久前就打算给自己搭一个 Jupyter Lab 环境，择日不如撞日，干脆就今天搞定。

# 那么什么是 Jupyter Notebook 呢？

> **Juptyer Notebook** 是从 **IPython** 演变出来的在线交互式计算环境，可以编写文档，这个文档可以包含代码、文本（Markdown）、数学、图标和富媒体。**Jupyter Lab** 是 **Jupyter Notebook** 的下一代用户界面。

# 简单上手
单纯跑一个 **Jupyter** 其实是很简单的：

```shell
pip install jupyterlab
jupyter lab
```

上面的命令就可以跑起来一个 Jupyter 了，只不过需要 Python 环境。

因为我要跑在服务器上，不希望服务器的环境太过杂乱，这样迁移起来就会很麻烦，所以最好跑在 Docker 里。

不幸的是我在实践 Docker 部署的过程中踩了很多坑，脑袋都给踩懵了。而且这部分偏偏网上资料比较少，只有[官方文档](https://jupyter-docker-stacks.readthedocs.io/en/latest/)和部分博客有涉及。所以我在此记录下我的踩坑历程，希望对后人有所帮助。

# 所以我都遇到了什么问题？

1. Docker 挂载后无法启动，提示权限被拒
2. 启动后设置密码报 500 错误
3. 启动后设置密码不生效
4. 新建文件报 404 错误

我大致就遇到了上面这些问题，各个都很奇葩。接下来，敬请观赏我心酸的心路历程：

## Docker 挂载匿名券后 jupyter 容器无法启动

其实直接跑起来容器，是不会报错的，但是这样我们的数据也就跟容器共生共死了，这可不能接受。

所以需要将 Jupyter 的数据路径映射到宿主机。接下来就出问题了，启动报错：

```
Container must be run with group "users" to update files
Executing the command: jupyter lab
Traceback (most recent call last):
  File "/opt/conda/lib/python3.7/site-packages/traitlets/traitlets.py", line 528, in get
    value = obj._trait_values[self.name]
KeyError: 'runtime_dir'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
"""堆栈信息隐藏掉"""
PermissionError: [Errno 13] Permission denied: '/home/jovyan/.local'
```

开始我怀疑是 SELinux 搞鬼，设置后并没有效果，而且其他镜像跑起来也很正常，一时间没有想出什么好办法，只好直接将挂载的路径权限改成 `777` 后正常启动了（本来是 `755`）。这操作...明明其他镜像同样权限都没问题...

![](media/15733019969423/15733071957282.jpg)

## 启动后初始化密码报 500 错误

终于启动成功了，很开心。但是初始化密码的时候又把我给干蒙了，直接报了 Http 500 错误。
看报错是找不到 `jupyter_notebook_config.json` 文件，这个文件是 jupyter 的配置文件。找不到这个文件说明配置初始化失败了，我们可以手动初始化，或者...多重启几遍服务（~~这个是真的有用~~）

### 手动初始化 juptyer 配置

1. 首先通过 `exec` 命令进入容器
2. 执行命令 `jupyter lab --generate-config`
3. 将配置路径下的文件权限都改为 `777`。执行 `chmod 777 .jupyter/*`
4. 退出容器

**注：**这个方法我们迁移服务器的时候不需要再做一遍了，因为我们的数据会备份，配置文件也在我们备份的范围内。

## 启动后设置密码不生效

折腾好配置，终于可以初始化密码了。结果重新登录的时候直接提示我密码错误 `Invalid credentials`。
我他妈的真的，要疯了。
终于，我找到了解决方案。重启两遍服务就好了。[GitHub issue](https://github.com/jupyter/docker-stacks/issues/717)

![](media/15733019969423/15733074291244.jpg)

垃圾！

## 结束了吗？没有。新建文件提示 404 
到这里我已经心如止水了，报错就报错吧。淦！

![-w384](media/15733019969423/15733063095629.jpg)

还好这个问题比较容易搞定。看了下 log

```log
[W 13:31:34.069 LabApp] Blocking Cross Origin API request for /api/contents/.  Origin: https://jupyter.fxcdev.com, Host: toner-jupyter:8888
[W 13:31:34.069 LabApp] Not Found
[W 13:31:34.070 LabApp] 404 POST /api/contents/?1573306294042 (172.28.0.3) 1.76ms referer=https://jupyter.fxcdev.com/lab?
```

是跨域导致的。为什么会跨域呢？
因为走了 nginx 转发，所以域名改变了。配置一下 nginx 就可以了。
在 location 对象里加上 `proxy_set_header Host $host;` 完美解决问题。

# 结语

文章比较乱，先综述一下问题解决方式吧：
1. Docker 挂载匿名券后无法启动，提示权限被拒 ———— 修改挂载路径权限为 `777`
2. 启动后设置密码报 500 错误 ———— 手动初始化 jupyter 配置，或者多重启几遍
3. 启动后设置密码不生效 ———— 多重启几遍 jupyter
4. 新建文件报 404 错误 ———— 配置 nginx 转发的时候顺便带上域名

最后，终于能舒服的用上环境了，过阵子数据分析完了如果有所发现，我会放出来给大家看看～敬请期待！

![](media/15733019969423/15733074925421.jpg)
