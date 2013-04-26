Working with Strings
===
---
Basic String Operations
---
字符串支持序列所有的标准操作（索引、分片、乘、检查成员、获取长度、获取最大值和最小值）。需要记住的一点是，字符串是不可修改的，所以所有对字符或分片赋值操作都是非法的。
```python
>>> website = 'http://www.python.org'
>>> website[-3:] = 'com'
Traceback (most recent call last):
  File "<pyshell#19>", line 1, in ?
    website[-3:] = 'com'
TypeError: object doesn't support slice assignment
```
String Formatting:The Short Version
---
字符串格式化使用字符串格式化操作符：`%`。在`%`左边放置格式化字符串，右边放置格式化的值。你可以使用单独的值比如一个字符串或者数字，你也可以使用一个元组（如果你想格式化多个值），当然你还可以使用一个字典。
最常见的例子是元组：
```python
>>> format = "Hello, %s. %s enough for ya?"
>>> values = ('world', 'Hot')
>>> print format % values
Hello, world. Hot enough for ya?
```
> 注意：如果你使用的是列表或者其他类型的序列，而不是元组，那么序列会被解释成一个单独的值。只有元组和字典允许你格式化多个值。

