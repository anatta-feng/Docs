> 利用 python 和 lint 删除项目无用资源

# 背景

有部分老项目是在Eclipse环境开发的，最近公司要求应用瘦身，老项目也在其中。如果在 AS 下开发就不会有这样的问题，但是在 Eclipse 中就不太方便了，于是就写了这个脚本。第一次用Python写东西，代码里可能会有许多`Java、C`这样的痕迹，见谅。

# 使用方法

将`python`目录下的`delUnused.py`放到项目目录下，然后直接运行即可。

# 代码说明

## 利用lint进行代码审查

`lint --check UnusedResources --xml [resultPath] [projectPath]`

命令含义是检查项目中未使用的资源文件，并且用xml格式输出结果，需要提供检查结果输出的路径和项目路径。在脚本中已经自动提供了。

```python
def exec_lint_command():
    cmd = 'lint --check UnusedResources --xml %s %s' % (_get_lint_result_path(), _get_project_dir_path())
    p = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE, stdin=subprocess.PIPE, stderr=subprocess.PIPE)
    c = p.stdout.readline().decode()
    while c:
        print(c)
        c = p.stdout.readline().decode()
```

这里给一个检查结果实例吧

```xml
    <issue
        id="UnusedResources"
        severity="Warning"
        message="The resource `R.layout.activity_all_player` appears to be unused"
        category="Performance"
        priority="3"
        summary="Unused resources"
        explanation="Unused resources make applications larger and slow down builds."
        errorLine1="&lt;LinearLayout xmlns:android=&quot;http://schemas.android.com/apk/res/android&quot;
"
        errorLine2="^"
        quickfix="studio">
        <location
            file="res\layout\activity_all_player.xml"
            line="2"
            column="1"/>
    </issue>
```

我们能用到的信息有`id` `message` `location` 等。

## 解析检查结果

我是利用 `minidom` 解析的，具体的解析方法不多说，[参考](http://fxcdev.com/2017/12/11/%E4%BD%BF%E7%94%A8-minidom-%E8%A7%A3%E6%9E%90xml/)。

* 获取根节点

```python
def _parse_lint_report():
    file = minidom.parse(_get_lint_result_path())
    root = file.documentElement
    beans = _parse_xml(root)
    return beans
```

* 解析第一层子节点 

```python
def _parse_xml(element, beans=None):
    if beans is None:
        beans = []
    for node in element.childNodes:
        if node.nodeName == ISSUE_KEY and node.nodeType is node.ELEMENT_NODE:
            lint_bean = _LintBean()
            lint_bean.id = node.getAttribute(ID_KEY)
            lint_bean.severity = node.getAttribute(SEVERITY_KEY)
            lint_bean.message = node.getAttribute(MESSAGE_KEY)
            _parse_location(node, lint_bean)
            lint_bean.print()
            beans.append(lint_bean)
    return beans
```

* 解析location 子节点

```python
def _parse_location(node, bean):
    if not node.hasChildNodes():
        return
    for child in node.childNodes:
        if child.nodeName == LOCATION_KEY and node.nodeType is node.ELEMENT_NODE:
            bean.location.file = child.getAttribute(LOCATION_FILE_KEY)
            bean.location.line = child.getAttribute(LOCATION_LINE_KEY)
            bean.location.column = child.getAttribute(LOCATION_COLUMN_KEY)
```

用Java习惯了，解析数据喜欢用Bean

```python
class _Location(object):
    def __init__(self):
        self.file = ''
        self.line = 0
        self.column = 0

class _LintBean(object):
    def __init__(self):
        self.id = ''
        self.severity = ''
        self.message = ''
        self.location = _Location()

    def print(self):
        print('find a %s, cause: %s. filePath: %s. line: %s' % (
            self.id, self.message, self.location.file, self.location.line))
```

# 处理无用资源

解析完数据，可以得到三种资源：

* Drawable，就一个文件，可以直接删
* xml中的一个节点，但是这个xml中就这一个节点，直接删文件
* xml中的一个节点，这个xml中有多个节点，删除节点

对这三种资源进行区分和删除

```python
for lint in lint_result:
    total_unused_resource += 1
    if lint.id != 'UnusedResources':
        continue
    if lint.location.line != '':
        is_single = _is_single_node(lint.location.file)
        if is_single:
            total_del_file += 1
            del_file(lint.location.file)
        else:
            total_remove_attr += 1
            node_name = get_node_name(lint.message)
            del_node(lint.location.file, node_name)
    else:
        total_del_file += 1
        del_file(lint.location.file)
```

删除文件

```python
def del_file(file_path):
    try:
        os.remove(file_path)
        print('remove %s success.' % file_path)
    except FileNotFoundError:
        print('remove %s error.' % file_path)
```

删除节点：

```python
def del_node(file_path, node_name):
    file = minidom.parse(file_path)
    root = file.documentElement
    nodes = root.childNodes
    for node in nodes:
        if node.nodeType in (node.TEXT_NODE, node.COMMENT_NODE):
            continue
        if node_name == node.getAttribute('name'):
            root.removeChild(node)
            file.writexml(open(file_path, 'w', encoding='UTF-8'), encoding='UTF-8')
            print('remove %s, node_name:%s. success!' % (file_path, node_name))
            return
```

至此，大功告成。

# 总结

当然，apk瘦身不仅仅是删除无用资源就可以的，还有冗余代码，重复文件等等，后续继续优化。