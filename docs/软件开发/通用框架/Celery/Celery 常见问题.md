# python3.7安装Celery4.2.0，redis2.10.6，运行报错
## 报错信息
```python
from . import async, base
                      ^
SyntaxError: invalid syntax
```
## 错误原因
是async名称更换了
```python
[Rename `async` to `asynchronous` (async is a reserved keyword in Python 3.7) *#4879](https://github.com/celery/celery/pull/4879)*
```
## 解决方案
通过 Github 安装 celery
```shell
pip install --upgrade https://github.com/celery/celery/tarball/master
```

---

