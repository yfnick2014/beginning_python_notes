Dictionaries: When Indices Won't Do
===
---
本章介绍字典，一种可以通过名称查找值的数据结构。这种数据结构类型被称为映射，在Python中唯一内建的映射类型就是字典。
Creating and Using Dictionaries
---
字典的写法是这样: `phonebook = {'Alice': '2341', 'Beth': '9102', 'Cecil': '3258'}`  
字典由键值对组成。在上面这个例子中，名称为键，电话号码为值。每个键值对中键和值用`:`分隔，键值对之间用`,`隔开，所有键值对用`{}`括起来，空的字典写成`{}`。
> 字典中键是唯一的，而值可以不唯一。  

###The dict Function###
可以使用`dict`函数从其他映射或序列的`(key, value)`对来构造字典。
```python
>>> items = [('name', 'Gumby'), ('age', 42)]
>>> d = dict(items)
>>> d
{'age': 42, 'name': 'Gumby'}
>>> d['name']
'Gumby'
```
还可以使用关键字参数，如下。
```python
>>> d = dict(name='Gumby', age=42)
>>> d
{'age': 42, 'name': 'Gumby'}
```
