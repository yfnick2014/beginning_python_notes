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
*Listing 4-1：使用字典*
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
避免这个问题的一种方式就是使用深拷贝，拷贝任何包含的值，通过使用`copy`模块里的`deepcopy`函数。
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
###fromkeys###
`fromkeys`方法创建给定键的新字典，默认值均为`None`。
```python
>>> {}.fromkeys(['name', 'age'])
{'age': None, 'name': None}
```
这个例子先创建一个空字典，接着调用`fromkeys`方法创建另一个字典，有点多余的方式。反而你可以直接在`dict`（所有字典的类型）上调用方法。
```python
>>> dict.fromkeys(['name', 'age'])
{'age': None, 'name': None}
```
如果你不想使用`None`为默认值，你可以提供你的默认值：
```python
>>> dict.fromkeys(['name', 'age'], '(unknown)')
{'age': '(unknown)', 'name': '(unknown)'}
```
###get###
`get`方法是一种访问字典项目的方法。平时当你访问字典中不存在的元素时，会产生错误：
```python
>>> d = {}
>>> print d['name']
Traceback (most recent call last):
   File "<stdin>", line 1, in ?
KeyError: 'name'
>>> print d.get('name')
None
```
正如你所看到的，当你用`get`访问不存在的键时并没有报异常，相反你会得到返回值`None`。你还可以提供你的默认值来替换`None`：
```python
>>> d.get('name', 'N/A')
'N/A'
```
当键存在时，`get`和其他字典查询一样：
```python
>>> d['name'] = 'Eric'
>>> d.get('name')
'Eric'
```
*Listing 4-2：使用`get`方法替代例子4-1*
```
# A simple database using get()

# Insert database (people) from Listing 4-1 here.

labels = {
   'phone': 'phone number',
   'addr': 'address'
}

name = raw_input('Name: ')

# Are we looking for a phone number or an address?
request = raw_input('Phone number (p) or address (a)? ')

# Use the correct key:
key = request # In case the request is neither 'p' nor 'a'
if request == 'p': key = 'phone'
if request == 'a': key = 'addr'

# Use get to provide default values:
person = people.get(name, {})
label = labels.get(key, key)
result = person.get(key, 'not available')

print "%s's %s is %s." % (name, label, result)
```
运行结果
```
Name: Gumby
Phone number (p) or address (a)? batting average
Gumby's batting average is not available.
```
###has_key###
`has_key`方法检查字典是否存在给定的键。表达式`d.has_key(k)`和`k in d`同等，选择哪种表达方式完全看个人品味，尽管`has_key`在Python3.0就不存在了。
```python
>>> d = {}
>>> d.has_key('name')
False
>>> d['name'] = 'Eric'
>>> d.has_key('name')
True
```
###items and iteritems###
`item`方法返回字典中键值对项目列表，且项目的顺序是任意的。
```python
>>> d = {'title': 'Python Web Site', 'url': 'http://www.python.org', 'spam': 0}
>>> d.items()
[('url', 'http://www.python.org'), ('spam', 0), ('title', 'Python Web Site')]
```
`iteritems`方法跟`item`方法类似，只不过返回的是迭代器而不是列表，而且使用`iteritems`在大多数情况下要更高效。
```python
>>> it = d.iteritems()
>>> it
<dictionary-iterator object at 169050>
>>> list(it) # Convert the iterator to a list
[('url', 'http://www.python.org'), ('spam', 0), ('title', 'Python Web Site')]
```
###keys and iterkeys###
`keys`方法返回字典中键的列表，而`iterkeys`返回键的迭代器。
###pop###
`pop`方法可以根据一个给定的键获取值，同时从字典删除该键值对。
```python
>>> d = {'x': 1, 'y': 2}
>>> d.pop('x')
1
>>> d
{'y': 2}
```
###popitem###
`popitem`方法同`list.pop`方法（弹出列表最后一个元素）类似。不同之处在于`popitem`弹出的项目是任意的，因为字典没有所谓的最后一个元素。如果你想以一种有效的方式依次删除和处理项目，可以使用`popitem`方法，不需要先遍历键的列表。
```python
>>> d
{'url': 'http://www.python.org', 'spam': 0, 'title': 'Python Web Site'}
>>> d.popitem()
('url', 'http://www.python.org')
>>> d
{'spam': 0, 'title': 'Python Web Site'}
```
尽管`popitem`同列表`pop`方法类似，但是在字典中没有等价于列表的`append`方法（在列表末尾添加元素）。因为字典并没有顺序，这样的方法没有任何意义。
###setdefault###
`setdefault`方法在某种程度上而言和`get`相似，都是通过给定的键查找值。除了`get`的功能外，`setdefault`方法在字典中没有给定的键时设置对应的值。
```python
>>> d = {}
>>> d.setdefault('name', 'N/A')
'N/A'
>>> d
{'name': 'N/A'}
>>> d['name'] = 'Gumby'
>>> d.setdefault('name', 'N/A')
'Gumby'
>>> d
{'name': 'Gumby'}
```
正如你所看到的，当键不存在时，`setdefault`返回默认值并更新字典；如果键存在则返回它的值而并不修改字典。默认是可选的，同`get`一样，当没有给定时默认为`None`。
```python
>>> d = {}
>>> print d.setdefault('name')
None
>>> d
{'name': None}
```
###update###
`update`方法使用另一个字典中的项目来更新字典。
```python
>>> d = {
       'title': 'Python Web Site',
       'url': 'http://www.python.org',
       'changed': 'Mar 14 22:09:15 MET 2008'
   }
>>> x = {'title': 'Python Language Website'}
>>> d.update(x)
>>> d
{'url': 'http://www.python.org', 'changed':
'Mar 14 22:09:15 MET 2008', 'title': 'Python Language Website'}
```
