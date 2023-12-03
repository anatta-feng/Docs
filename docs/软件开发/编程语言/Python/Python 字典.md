# 判断字典中是否有这个 key
```python
'key' in dict
```

# 获取字典 keys

```python
data_dict = {
    'a': 'A',
    'b': 'B'
}
keys = data_dict.keys()
```

此时获取到的 keys 的类型是`dict_keys`，并不是 list 类型，如果要获取 list 类型还需要：

```python
keys = list(keys)
```

