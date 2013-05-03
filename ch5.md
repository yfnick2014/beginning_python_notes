Conditionals, Loops, and Some Other Statements
===
---
More About print and import
---
###用逗号打印###
实际上你可以打印多条表达式，只需使用逗号分隔开：
```python
>>> print 'Age:', 42
Age: 42
```
可以看到，每个参数之间强行插入了一个空格。
> 注意`print`参数并不是生成一个元组
```python
>>> 1, 2, 3
>>> print 1, 2, 3
1 2 3
>>> print (1, 2, 3)
(1, 2, 3)
```  

如果你想连接文本和变量的值而不用格式化字符串，这种方式会显得非常有用：
```python
>>> name = 'Gumby'
>>> salutation = 'Mr.'
>>> greeting = 'Hello,'
>>> print greeting, salutation, name
Hello, Mr. Gumby
```
###以其他名称导入###
通常当你从模块导入时，可以使用  
`import somemodule`  
或  
`from somemodule import somefunction`  
或  
`from somemodule import somefunction, anotherfunction, yetanotherfunction`  
或  
`from somemodule import *`
以上第四种只有在你确信需要导入模块里所有内容时才使用。
但如果你有两个模块，每个模块均包含一个函数名为`open`，你可以使用上面第一种格式导入模块，然后按如下的方式使用函数：
```python
module1.open(...)
module2.open(...)
```
但还有另外一种方式，你可以在后面加上`as`从句，指定使用的函数名甚至是模块名：
```python
>>> import math as foobar
>>> foobar.sqrt(4)
2.0
>>> from math import sqrt as foobar
>>> foobar(4)
2.0
# For the open functions, you might use the following
from module1 import open as open1
from module1 import open as open2
```
Assignment Magic
---
###序列拆封###
你可以同时为多个变量赋值：
```python
>>> x, y, z = 1, 2, 3
>>> print x, y, z
1 2 3
```
听起来没有什么用吗？好吧，这样你可以同时交换多个变量的值：
```python
>>> x, y = y, x
>>> print x, y, z
2 1 3
```
实际上，这里我在做的叫做序列拆封（或迭代器拆封），一个序列(或任意迭代器对象)可以拆封成多个变量。
```python
>>> values = 1, 2, 3
>>> values
(1, 2, 3)
>>> x, y, z = values
>>> x
1
```
当函数或方法返回元组（或其他序列或迭代器对象）时它会相当有用。
比如当你想从字典中搜索（和删除）一个任意的键值对，你可以使用`popitem`方法以元组的形式返回键值对，那么你就可以将返回的元组直接拆封成两个变量：
```python
>>> scoundrel = {'name': 'Robin', 'girlfriend': 'Marion'}
>>> key, value = scoundrel.popitem()
>>> key
'girlfriend'
>>> value
'Marion'
```
这样可以允许函数以元组封装的形式返回多个值，同时可以通过简单的赋值来访问返回的值。
拆封的序列必须含有列在`=`左边变量个数相同的元素，否则在赋值时Python会发生异常。
```python
>>> x, y, z = 1, 2
Traceback (most recent call last):
   File "<stdin>", line 1, in <module>
ValueError: need more than 2 values to unpack
>>> x, y, z = 1, 2, 3, 4
Traceback (most recent call last):
   File "<stdin>", line 1, in <module>
ValueError: too many values to unpack
```
> Python3.0有另一个拆封的特点：你可以使用`*`操作符。  
> 例如，`a, b, rest* = [1, 2, 3, 4]`在赋值给`a`和`b`后将剩余的值赋值给`rest`。
> 在这个例子中，`rest`将会成为`[3, 4]`  

###链赋值###
当你希望绑定多个变量到同一个值时，可以直接使用链赋值。
```python
x = y = somefunction()
# which is same as
y = somefunction()
x = y
# note that the preceding statements may not be the same as
x = somefunction()
y = somefunction()
```
###增强赋值###
```python
>>> x = 2
>>> x += 1
>>> x *= 2
>>> x
6

# it also works with other data types
>>> fnord = 'foo'
>>> fnord += 'bar'
>>> fnord *= 2
>>> fnord
'foobarfoobar'
```
Blocks: The Joy of Indentation
---
代码块是一组语句，当条件为真时执行（条件语句），或执行多次（循环语句）等。通过缩进一部分代码或者在前面填入空格，可以形成代码块。
> 注意，你也可以使用制表符来缩进你的代码块。标准的方式是建议使用空格，而不是制表符，尤其是在每层缩进使用4个空格。  

代码块的每行必须使用相同数量的空格缩进，以下用伪代码来表示如何缩进：
```
this is a line
this is another line:
    this is another block
    continuing the same block
    the last line of this block
phew, there we escaped the inner block
```
在许多编程语言中，通常会使用一个特殊的词或字符（比如，`{`用来开始一块代码，`}`用来结束这块代码）来表示代码块。
而在Python中，使用`:`来表示代码块开始，接着代码块中的每行代码都要缩进，当回到相同数量的缩进时表示代码块已经结束。
Conditions and Conditional Statements
---
###布尔值的意义###
当被看成为布尔表达式时，以下的值均被解释器认为是假的值：  
`False    None    0    ""    ()    []    {}`  
换句话说，标准值`False`和`None`、所有的数值0（包括浮点、长整型等）、空的序列（例如空字符串、元组和列表）和空字典都被认为是假。其他所有的都被解释为真，包含特殊值`True`。  
这就意味着Python中所有值都可以被解释成为真值，当然标准真值是`True`和`False`。
在一些编程语言中（例如C和Python2.3以前），标准真值为`0`（表示假）和`1`（表示真）。
实际上来说，`True`和`False`并没有那么大差别，它们只是`0`和`1`的华丽替身。如果你看到逻辑表达式返回`1`或`0`，你要知道这表示的是`True`还是`False`。
```python
>>> True
True
>>> False
False
>>> True == 1
True
>>> False == 0
True
>>> True + False + 42
43
```
布尔值`True`和`False`属于`bool`类型，可以用来（就像`list`、`str`和`tuple`）转换其他值：
```python
>>> bool('I think, therefore I am')
True
>>> bool(42)
True
>>> bool('')
False
>>> bool(0)
False
```
###条件表达式和if语句###
```python
name = raw_input('What is your name? ')
if name.endswith('Gumby'):
    print 'Hello, Mr. Gumby'
```
###else语句###
```python
name = raw_input('What is your name? ')
if name.endswith('Gumby'):
    print 'Hello, Mr. Gumby'
else:
    print 'Hello, Stranger'
```
###elif语句###
```python
num = input('Enter a number: ')
if num > 0:
    print 'The number is positive'
elif num < 0:
    print 'The number is negative'
else:
    print 'The number is zero'
```
###嵌套块###
```python
name = raw_input('What is your name? ')
if name.endswith('Gumby'):
    if name.startswith('Mr.):
        print 'Hello, Mr. Gumby'
    elif name.startswith('Mrs.'):
        print 'Hello, Mrs. Gumby'
    else:
        print 'Hello, Gumby'
else:
    print 'Hello, Stranger'
```
###更多复杂条件###
