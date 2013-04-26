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
> 注意：如果你使用的是列表或者其他类型的序列，而不是元组，那么序列会被解释成一个单独的值。  
> **只有元组和字典允许你格式化多个值。**  

`%s`称为转换说明符，它们放在值被插入的位置。`s`表示格式化的值为字符串，如果给定的值不是字符串，会被`str`转换为字符串。
> 注意：如果需要在格式化字符串中包含`%`，你必须写成`%%`那么Python不会在最开始将`%`解析成为转换说明符  

```python
# formatting floats
>>> format = "Pi with three decimals: %.3f"
>>> from math import pi
>>> print format % pi
Pi with three decimals: 3.142
```
###TEMPLATE STRINGS###
`string`模块提供另外一种格式化值的方式：模板字符串。它很类似UNIX shell中变量替换，`$foo`被替换成名为foo的关键字参数，而参数是通过`substitute`模板方法传递的。
```python
>>> from string import Template
>>> s = Template('$x, glorious $x!')
>>> s.substitute(x='slurm')
'slurm, glorious slurm!'
```
如果匹配字段是单词的一部分，为了明确表示它在哪里结束，名称必须`{}`括起来。
```python
>>> s = Template("It's ${x}tastic!")
>>> s.substitute(x='slurm')
"It's slurmtastic!"
```
为了插入`$`符号，使用`$$`：
```python
>>> s = Template("Make $$ selling $x!")
>>> s.substitute(x='slurm')
'Make $ selling slurm!'
```
除了使用关键字参数，你可以用字典提供键值对：
```python
>>> s = Template('A $thing must never $action.')
>>> d = {}
>>> d['thing'] = 'gentleman'
>>> d['action'] = 'show his socks'
>>> s.substitute(d)
'A gentleman must never show his socks.'
```
还有一个方法`safe_substitute`，在出现缺失值或不正确使用`$`字符的情况下，该方法不会提示错误信息。
