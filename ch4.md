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
Basic Dictionary Operations
---
字典的基本操作和序列的基本相同：
- `len(d)`返回`d`中键值对数量
- `d[k]`返回对应键`k`的值
- `d[k] = v`将键`k`与值`v`相关联
- `del d[k]`删除键为`k`的项目
- `k in d`检查在`d`中是否存在键为`k`的项目  

尽管字典和列表有一些相同的特点，但仍然存在一些重要的差别：
- **键类型**：字典的键不一定是整数，可以是任何不可变类型，比如浮点数、字符串或者元组。
- **自动添加**：你可以对一个不存在的键赋值，这样会创建一个新的项目；但你不能对列表范围外的索引元素进行赋值（除了使用`append`类似的方法之外）。
- **成员关系**：表达式`k in d`（其中`d`是字典）查找的是键而不是值；而表达式`v in l`（其中`l`是列表）查找的是指而不是索引。  

```python
>>> x = []
>>> x[42] = 'Foobar'
Traceback (most recent call last):
   File "<stdin>", line 1, in ?
IndexError: list assignment index out of range
>>> x = {}
>>> x[42] = 'Foobar'
>>> x
{42: 'Foobar'}
```
*Demo：使用字典*
```python
# A simple database

# A dictionary with person names as keys. Each person is represented as
# another dictionary with the keys 'phone' and 'addr' referring to their phone
# number and address, respectively.
people = {

    'Alice': {
        'phone': '2341',
        'addr': 'Foo drive 23'
        },
    'Beth': {
        'phone':'9102',
        'addr':'Bar street 42'
        },
    'Ceil': {
        'phone':'3158',
        'addr':'Baz avenue 90'
        }
    }

# Descriptive labels for the phone number and address. These will be used
# when printing the output.
labels = {
    'phone': 'phone number',
    'addr':'address'
}

name = raw_input('Name: ')
# Are we looking for a phone number or an address?
request = raw_input('Phone number (p) or address (a)? ')

# Use the correct key:
if request == 'p':  key = 'phone'
if request == 'a':  key = 'addr'

# Only try to print information if the name is a valid key in
# our dictionary:
if name in people:  print "%s's %s is %s." % \
   (name, labels[key], people[name][key])
```
运行结果
```
Name: Beth 
Phone number (p) or address (a)? p 
Beth's phone number is 9102.
```
String Formatting with Dictionaries
---
字典的格式化字符串也很容易，在`%`后加上用`()`包含的键即可：
```python
>>> phonebook 
{'Beth': '9102', 'Alice': '2341', 'Cecil': '3258'} 
>>> "Cecil's phone number is %(Cecil)s." % phonebook 
"Cecil's phone number is 3258."
```
发现这种格式化字符串在模板系统里会相当有用（例如在使用HTML时）：
```python
>>> template = '''<html> 
    <head><title>%(title)s</title></head> 
    <body> 
    <h1>%(title)s</h1> 
    <p>%(text)s</p> 
    </body>''' 
>>> data = {'title': 'My Home Page', 'text': 'Welcome to my home page!'} 
>>> print template % data 
<html> 
<head><title>My Home Page</title></head> 
<body> 
<h1>My Home Page</h1> 
<p>Welcome to my home page!</p> 
</body> 
```
Dictionary Methods
---
###clear###
`clear`方法从字典中删除所有项目，这是一种原地操作（像`list.sort`），因此它没有返回值（或者说`None`）。
```python
>>> d = {} 
>>> d['name'] = 'Gumby' 
>>> d['age'] = 42 
>>> d 
{'age': 42, 'name': 'Gumby'}
>>> returned_value = d.clear() 
>>> d 
{} 
>>> print returned_value 
None 
```
这有什么用呢？我们来看两种情况，下面是第一种：
```python
>>> x = {} 
>>> y = x 
>>> x['key'] = 'value' 
>>> y 
{'key': 'value'} 
>>> x = {} 
>>> y 
{'key': 'value'}
```
下面是第二种情况：
```python
>>> x = {} 
>>> y = x 
>>> x['key'] = 'value' 
>>> y 
{'key': 'value'} 
>>> x.clear() 
>>> y 
{} 
```
在这两种情况下，`x`和`y`开始都指向同一个字典。在第一种情况下，`x`变为空是通过赋值为新的空字典，这并不影响`y`指向先前的字典；
如果你期望的是删除先前字典的所有元素，那么你必须使用`clear`方法，在第二种情况下`y`也成了空字典。
###copy###
`copy`方法返回一个有相同键值对的新字典（浅拷贝，因为值本身是相同的，而不是拷贝）：
```python
>>> x = {'username': 'admin', 'machines': ['foo', 'bar', 'baz']} 
>>> y = x.copy() 
>>> y['username'] = 'mlh' 
>>> y['machines'].remove('bar') 
>>> y 
{'username': 'mlh', 'machines': ['foo', 'baz']} 
>>> x 
{'username': 'admin', 'machines': ['foo', 'baz']}
```
可以发现，当你替换拷贝中的值时，原来的并不变化。然而，如果你修改一个值（就地而不是替换它），原来的也发生变化，因为相同的值存储在这里。  
避免这个问题的一种方式是使用深拷贝，拷贝任何包含的值，通过使用`copy`模块里的`deepcopy`函数。
```python
>>> from copy import deepcopy 
>>> d = {} 
>>> d['names'] = ['Alfred', 'Bertrand'] 
>>> c = d.copy() 
>>> dc = deepcopy(d) 
>>> d['names'].append('Clive') 
>>> c 
{'names': ['Alfred', 'Bertrand', 'Clive']} 
>>> dc 
{'names': ['Alfred', 'Bertrand']} 
```
