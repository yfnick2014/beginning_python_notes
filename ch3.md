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
###模板字符串###
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
String Formatting:The Long Version
---
如果右边的操作数是元组，它的每个元素被单独格式化，且每个值都需要转换说明符。
> 注意：如果元组转换为转换说明符的一部分，元组必须要用括号括起来。
```python
>>> '%s plus %s equals %s' % (1, 1, 2)
'1 plus 1 equals 2'
>>> '%s plus %s equals %s' % 1, 1, 2 # Lacks parentheses!
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
TypeError: not enough arguments for format string
```   

一个基本的转换说明符包括：
- `%`：它标记转换说明符的开始。
- 转换标记：可选项。`-`表示左对齐，`+`表示符号填充，` `表示正数前空格填充，`0`表示零填充。
- 最小字符宽度：可选项。指定转换后的字符串长度至少为这么宽。如果为`*`，宽度通过元组值读取。
- `.`跟上精度：可选项。如果转换的是实数，显示精确到小数点后几位；如果转换的是字符串，精度表示最大字符宽度；如果精度是`*`，那么精度会从元组值读取。
- 转换类型：可以是列表3-1中的任何类型。

表3-1：字符串格式化转换类型
<table>
  <tr>
    <th>转换类型</th>
    <th>含义</th>
  </tr>
  <tr>
    <td>d,i</td>
    <td>有符号十进制整数</td>
  </tr>
  <tr>
    <td>o</td>
    <td>无符号八进制</td>
  </tr>
  <tr>
    <td>u</td>
    <td>无符号十进制</td>
  </tr>
  <tr>
    <td>x</td>
    <td>无符号十六进制（小写）</td>
  </tr>
  <tr>
    <td>X</td>
    <td>无符号十六进制（大写）</td>
  </tr>
  <tr>
    <td>e</td>
    <td>浮点指数格式（小写）</td>
  </tr>
  <tr>
    <td>E</td>
    <td>浮点指数格式（大写）</td>
  </tr>
  <tr>
    <td>f,F</td>
    <td>浮点十进制格式</td>
  </tr>
  <tr>
    <td>g</td>
    <td>如果指数大于-4或小于精度则和e相同，否则和f相同</td>
  </tr>
  <tr>
    <td>G</td>
    <td>如果指数大于-4或小于精度则和E相同，否则和F相同</td>
  </tr>
  <tr>
    <td>c</td>
    <td>单个字符（接受一个整数或单个字符的字符串）</td>
  </tr>
  <tr>
    <td>r</td>
    <td>字符串（用`repr`转换任何Python对象）</td>
  </tr>
  <tr>
    <td>s</td>
    <td>字符串（用`str`转换任何Python对象）</td>
  </tr>
</table>
###简单转换###
仅包含一个转换类型
```python
>>> 'Price of eggs: $%d' % 42
'Price of eggs: $42'
>>> 'Hexadecimal price of eggs: %x' % 42
'Hexadecimal price of eggs: 2a'
>>> from math import pi
>>> 'Pi: %f...' % pi
'Pi: 3.141593...'
>>> 'Very inexact estimate of pi: %i' % pi
'Very inexact estimate of pi: 3'
>>> 'Using str: %s' % 42L
'Using str: 42'
>>> 'Using repr: %r' % 42L
'Using repr: 42L'
```
###宽度和精度###
转换说明符可能包含字段宽度和精度。这两个参数需指定两个整数（先是宽度，后是精度），通过`.`分隔。如果你只想使用精度，`.`也必须包含。
```python
>>> '%10f' % pi    # Field width 10
'  3.141593'
>>> '%10.2f' % pi    # Field width 10, precision 2
'      3.14'
>>> '%.2f' % pi
# Precision 2
'3.14'
>>> '%.5s' % 'Guido van Rossum'
'Guido'
```
###符号、对齐和零填充###
在宽度和精度数字前，你可以放置标记符号，可以是`0`、`+`、`-`或者` `。`0`表示数字将会使用零填充。
```python
>>> '%010.2f' % pi
'0000003.14'
```
> 注意这里的`0`和数字`010`前面的`0`是不同的，后者表示八进制。  

`-`表示左对齐。
```python
>>> '%-10.2f' % pi
'3.14
```
` `表示空格会放在正数前，可以用来对齐正数和负数。
```python
>>> print ('% 5d' % 10) + '\n' + ('% 5d' % -10)
   10
  -10
```
`+`表示符号会置于正数和负数之前。
```python
>>> print ('%+5d' % 10) + '\n' + ('%+5d' % -10)
  +10
  -10
```
*Demo:格式化字符串*
```python
# Print a formatted price list with a given width

width = input('Please enter width: ')

price_width = 10
item_width = width - price_width

header_format = '%-*s%*s'
format        = '%-*s%*.2f'

print '=' * width

print header_format % (item_width, 'Item', price_width, 'Price')

print '-' * width

print format % (item_width, 'Apples', price_width, 0.4)
print format % (item_width, 'Pears', price_width, 0.5)
print format % (item_width, 'Cantaloupes', price_width, 1.92)
print format % (item_width, 'Dried Apricots (16 oz.)', price_width, 8)
print format % (item_width, 'Prunes (4 lbs.)', price_width, 12)

print '=' * width
```
运行结果
```
Please enter width: 35
===================================
Item                          Price
-----------------------------------
Apples                         0.40
Pears                          0.50
Cantaloupes                    1.92
Dried Apricots (16 oz.)        8.00
Prunes (4 lbs.)               12.00
===================================
```
String Methods
---
字符串比列表拥有更加丰富的方法，因为字符串“继承”了字符串模块的许多函数。
> 尽管字符串方法已经完全盖过字符串模块，但是字符串模块仍然包含一些常量和函数是字符串方法所不能提供的。  
> 以下是字符串中一些常用的常量：  
> - `string.digits`: 包含0-9数字的字符串
> - `string.letters`: 包含所有字母（大写和小写）的字符串
> - `string.lowercase`: 包含所有小写字母的字符串
> - `string.printable`: 包含所有打印字符的字符串
> - `string.punctuation`: 包含所有标点符号的字符串
> - `string.uppercase`: 包含所有大写字母的字符串  
> 
> 注意字符串常量字母（例如string.letters）是依赖于语言环境的，如果你想确认你要使用ASCII，你可以使用名称中包含`ascii_`的变种，例如`string.ascii_letters`  

###find###
`find`方法在字符串中寻找子串，如果子串存在则返回最左边的索引，如果找不到则返回-1
```python
>>> 'With a moo-moo here, and a moo-moo there'.find('moo')
7
>>> title = "Monty Python's Flying Circus"
>>> title.find('Monty')
0
>>> title.find('Python')
6
>>> title.find('Flying')
15
>>> title.find('Zirquss')
-1
```
> 注意：字符串方法`find`不会返回布尔值，如果`find`返回0，表示子串存在且在索引0处。  

你还可以为你的查询提供起始点和终止点。
```python
>>> subject = '$$$ Get rich now!!! $$$'
>>> subject.find('$$$')
0
>>> subject.find('$$$', 1) # Only supplying the start
20
>>> subject.find('!!!')
16
>>> subject.find('!!!', 0, 16) # Supplying start and end
-1
```
> 注意由起始和终止值指定的范围，包含第一个索引但不包含第二个索引，这在Python是常例。  

###join###
字符串中一个非常重要的方法，`join`是`split`的逆操作。它用来连接序列的元素。需要指出的是，连接的序列元素必须都是字符串。
```python
>>> seq = [1, 2, 3, 4, 5]
>>> sep = '+'
>>> sep.join(seq) # Trying to join a list of numbers
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
TypeError: sequence item 0: expected string, int found
>>> seq = ['1', '2', '3', '4', '5']
>>> sep.join(seq) # Joining a list of strings
'1+2+3+4+5'
>>> dirs = '', 'usr', 'bin', 'env'
>>> '/'.join(dirs)
'/usr/bin/env'
>>> print 'C:' + '\\'.join(dirs)
C:\usr\bin\env
```
###lower###
`lower`方法返回字符串的小写形式：
```python
>>> 'Trondheim Hammer Dance'.lower()
'trondheim hammer dance'
```
如果你想编写大小写不敏感的代码（即不区分大小写的代码），这会显得很有用。例如，假设你想检验一个用户名是否在列表内，如果你的列表包含字符串`'gumby'`而用户输入它的名称是`'Gumby'`，那么你无法找到它。
```python
>>> if 'Gumby' in ['gumby', 'smith', 'jones']: print 'Found it!'
...
>>>
```
当然，如果你存储了`'Gumby'`而用户写成了`'gumby'`或者甚至是`'GUMBY'`，会发生相同的结果。解决办法就是在存储和查找时将所有名称转换为小写，那么就有如下的代码。
```python
>>> name = 'Gumby'
>>> names = ['gumby', 'smith', 'jones']
>>> if name.lower() in names: print 'Found it!'
...
Found it!
>>>
```
> 和`lower`相关的是`title`方法，用来标题化字符串，也就是说，所有单词以大写字母开头，其他字母均为小写。然而，单词边界可能会给出一些不自然的结果。
```python
>>> "that's all folks".title()
"That'S All, Folks"
```
> 另一种方式是采用字符串模块中`capwords`函数。
```python
>>> import string
>>> string.capwords("that's all, folks")
"That's All, Folks"
```  

###replace###
`replace`方法用来替换字符串中的子串为另一子串。如果你曾经用过单词处理程序中“查找和替换”的功能，你无疑能了解到这个方法的用处。
```python
>>> 'This is a test'.replace('is', 'eez')
'Theez eez a test'
```
###split###
`split`是`join`方法的逆操作，用来将字符串分割成序列。
```python
>>> '1+2+3+4+5'.split('+')
['1', '2', '3', '4', '5']
>>> '/usr/bin/env'.split('/')
['', 'usr', 'bin', 'env']
>>> 'Using the default'.split()
['Using', 'the', 'default']
```
注意如果没有提供分隔符，默认根据连续的空白字符（空格、制表符、换行符等）来分割。
###strip###
`strip`方法返回除去左端和右端空白字符的字符串。
```python
>>> ' internal whitespace is kept '.strip()
'internal whitespace is kept'
```
同`lower`类似，`strip`在比较输入值和存储值时也比较有用。我们回到`lower`部分的用户名例子，如果用户不经意间在名称后多输了一个空格。
```python
>>> names = ['gumby', 'smith', 'jones']
>>> name = 'gumby '
>>> if name in names: print 'Found it!'
...
>>> if name.strip() in names: print 'Found it!'
...
Found it!
>>>
```
你还可以指定被除去的字符，通过在字符串参数中列出它们。
```python
>>> '*** SPAM * for * everyone!!! ***'.strip(' *!')
'SPAM * for * everyone'
```
###translate###
`translate`和`replace`的类似之处在于匹配字符串的局部，不同的是`translate`只能处理单个字符。它的强大在于可以同时执行多个替换，而且比`replace`更高效。  
对于`translate`方法有一些特别技术性的用途（例如转换换行符或者其他依赖平台的特殊字符），但我们先看一个简单的例子————将纯英文文本转换成德国口音。即，你必须要替换字符`c`为`k`，字符`s`为`z`。  
在使用`translate`前，你必须制作转换表，转换表描述了字符转换的信息。由于这个表有256个输入，你不能自己写出来，而是使用字符串模块的`maketrans`函数。  
`maketrans`函数有两个参数：两个相同长度的字符串，表示字符串中存在的第一个字符串中的字符，会被第二个字符串中相同位置的字符所替代。代码如下：
```python
>>> from string import maketrans
>>> table = maketrans('cs', 'kz')
```
> 转换表存储了对应于ASCII字符集的256个字符的转换字符表。
```python
>>> table = maketrans('cs', 'kz')
>>> len(table)
256
>>> table[97:123]
'abkdefghijklmnopqrztuvwxyz'
>>> maketrans('', '')[97:123]
'abcdefghijklmnopqrstuvwxyz'
```  

一旦你得到了这个转换表，你可以将它作为`translate`方法的参数，用它来转换你的字符串。
```python
>>> 'this is an incredible test'.translate(table)
'thiz iz an inkredible tezt'
```
`translate`方法还提供了第二个可选参数，用来指定被删除的字母。比如如果你想模仿说话很快的德国人，你可以删除所有空格。
```python
>>> 'this is an incredible test'.translate(table, ' ')
'thizizaninkredibletezt'
```
> ####非英文字符的问题####
> 有时字符串方法`lower`也解决不了问题，例如正巧你要使用非英文字母表。比如你想将大写挪威单词`BØLLEFRØ`转换为小写形式。
```python
>>> print 'BØLLEFRØ'.lower()
bØllefrØ
```
> 显然，这并不好使，因为Python不认为`Ø`是一个字母。在这种情况下，你可以使用`translate`来转换。
```python
>>> table = maketrans('ÆØÅ', 'æøå')
>>> word = 'KÅPESØM'
>>> print word.lower()
kÅpesØm
>>> print word.translate(table)
KåPESøM
>>> print word.translate(table).lower()
kåpesøm
```
> 同时，用Unicode也可能解决你的问题。
```python
>>> print u'ærnæringslære'.upper()
ÆRNÆRINGSLÆRE
```
