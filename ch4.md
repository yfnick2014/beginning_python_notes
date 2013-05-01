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
