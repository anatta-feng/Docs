#  dict 转为 Json 字符串
```python
j = json.dumps(dic)
```
# Json 格式字符串转为 dict
```python
d = json.loads(str)
```

# dict字典转json数据

```python
def dict_to_json():
    dict = {}
    dict['name'] = 'many'
    dict['age'] = 10
    dict['sex'] = 'male'
    print(dict)  # 输出：{'name': 'many', 'age': 10, 'sex': 'male'}
    j = json.dumps(dict)
```

# 对象转json数据

```python
import json

def obj_to_json():
    stu = Student('007', '007', 28, 'male', '13000000000', '123@qq.com')
    print(type(stu))  # <class 'json_test.student.Student'>
    stu = stu.__dict__  # 将对象转成dict字典
    print(type(stu))  # <class 'dict'>
    print(stu)  # {'id': '007', 'name': '007', 'age': 28, 'sex': 'male', 'phone': '13000000000', 'email': '123@qq.com'}
    j = json.dumps(obj=stu)
    print(j)  # {"id": "007", "name": "007", "age": 28, "sex": "male", "phone": "13000000000", "email": "123@qq.com"}


if __name__ == '__main__':
    obj_to_json()
```

