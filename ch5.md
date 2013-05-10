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
###条件表达式和`if`语句###
```python
name = raw_input('What is your name? ')
if name.endswith('Gumby'):
    print 'Hello, Mr. Gumby'
```
###`else`语句###
```python
name = raw_input('What is your name? ')
if name.endswith('Gumby'):
    print 'Hello, Mr. Gumby'
else:
    print 'Hello, Stranger'
```
###`elif`语句###
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
####比较操作符####
*Table 5-1. Python比较操作符*
<table>
   <tr>
      <th>表达式</th>
      <th>描述</th>
   </tr>
   <tr>
      <td>x == y</td>
      <td>x等于y</td>
   </tr>
   <tr>
      <td>x < y</td>
      <td>x小于y</td>
   </tr>
   <tr>
      <td>x > y</td>
      <td>x大于y</td>
   </tr>
   <tr>
      <td>x >= y</td>
      <td>x大于或等于y</td>
   </tr>
   <tr>
      <td>x <= y</td>
      <td>x小于或等于y</td>
   </tr>
   <tr>
      <td>x != y</td>
      <td>x不等于y</td>
   </tr>
   <tr>
      <td>x is y</td>
      <td>x和y是相同对象</td>
   </tr>
   <tr>
      <td>x is not y</td>
      <td>x和y是不同对象</td>
   </tr>
   <tr>
      <td>x in y</td>
      <td>x是容器y的成员</td>
   </tr>
   <tr>
      <td>x not in y</td>
      <td>x不是容器y的成员</td>
   </tr>
</table>
**等于操作符**
```python
>>> "foo" == "foo"
True
>>> "foo" == "bar"
False
>>> "foo" = "foo"
SyntaxError: can't assign to literal
```
**`is`:身份操作符**
```python
>>> x = y = [1, 2, 3]
>>> z = [1, 2, 3]
>>> x == y
True
>>> x == z
True
>>> x is y
True
>>> x is z
False
```
为什么`x is not z`？尽管它们是相等的，为什么？因为`is`测试的是两者是否相同，而不是是否相等。
变量`x`和`y`绑定到相同的列表，而`z`绑定到了另外一个列表，且正好这个列表的值和前一个列表的值相等。它们可能相等，但是它们不是同一个对象。
```python
>>> x = [1, 2, 3]
>>> y = [2, 4]
>>> x is not y
True
>>> del x[2]
>>> y[1] = 1
>>> y.reverse()
>>> x == y
True
>>> x is y
False
```
**`in`:成员操作符**
```python
name = raw_input('What is your name? ')
if 's' in name:
    print 'Your name contains the letter "s".'
else:
    print 'Your name does not contain the letter "s".'
```
**比较字符串和序列**  
```python
# strings are compared according to their order when sorted alphabetically
>>> "alpha" < "beta"
True
# ignore the difference between uppercase and lowercase letters
>>> 'FnOrD'.lower() == 'Fnord'.lower()
True
# other sequences are compared in the same manner
>>> [1, 2] < [2, 1]
True
# the sequences contain other sequences as elements
>>> [2, [1, 4]] < [2, [1, 5]]
True
```
**布尔操作符**
```python
# reads a number and checks whether it's between 1 and 10(inclusive)
number = input('Enter a number between 1 and 10: ')
if number <= 10 and number >=1 :
    print 'Great!'
else:
    print 'Wrong!'
```
`and`操作符就是所谓的布尔操作符，它需要两个真值，如果均为真则返回真，否则返回假。还有另外两个布尔操作符`or`和`not`。
```python
# you can combine truth values in any way you like
if ((cash > price) or customer_has_good_credit) and not out_of_stock:
    give_goods()
```
###断言###
在Python中断言和`if`语句很类似，通常断言有点类似这样（伪代码）：
```
if not condition:
    crash program
```
那么为什么需要断言？因为当错误条件发生时使程序崩溃要比之后崩溃要好。
你需要保证某些条件为真，这样可以接下来做下一件事情。  
断言的关键字为`assert`，如果你要保证某些事情必须为真才能继续，可以将`assert`语句作为程序的检查点：
```python
>>> age = 10
>>> assert 0 < age < 100
>>> age = -1
>>> assert 0 < age < 100
Traceback (most recent call last):
   File "<stdin>", line 1, in ?
AssertionError
# a string may be added after the condition, to explain the assertion
>>> age = -1 
>>> assert 0 < age < 100, 'The age must be realistic' 
Traceback (most recent call last): 
   File "<stdin>", line 1, in ? 
AssertionError: The age must be realistic 
```
Loops
---
###`while`循环###
```python
# print from 1 to 100(inclusive)
x = 1
while x <= 100:
    print x
    x += 1
```
###`for`循环###
```python
# iterate a list
words = ['this', 'is', 'an', 'ex', 'parrot']
for word in words:
    print word
# another example
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
for number in numbers:
    print number
```
Python内建的`range`函数可以方便遍历一个数字范围：
```python
>>> range(0, 10)
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> range(10)
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
# the following program writes out the numbers from 1 to 100
for number in range(1, 101):
    print number
```
###遍历字典###
```python
d = {'x': 1, 'y': 2, 'z': 3}
for key in d:
    print key, 'corresponds to', d[key]
```
如果你关心的只有字典的值，那么你可以用`d.values`来替代`d.keys`。你可能还记得`d.items`返回键值对元组，利用这一特点你可以使用序列拆封：
```python
for key, value in d.items():
    print key, 'corresponds to', value
```
###一些迭代组件###
Python有一些函数可以用来遍历序列或其他迭代的对象，其中一些在`itertools`模块中提供，但也有一些内建函数可以派上用场。
####并行迭代####
有时候你希望同时遍历两个序列，例如你希望遍历以下两个列表：
```python
names = ['anne', 'beth', 'george', 'damon']
ages = [12, 45, 32, 102]
```
如果你想打印姓名和对应的年龄，你可以如下操作：
```python
for i in range(len(names)):
    print names[i], 'is', ages[i], 'years old'
```
上面的例子中`i`作为循环指数，除了这种方法外还有一种内建的函数`zip`可以使用，它用来将序列压缩到一起，返回元组的列表：
```python
>>> zip(names, ages)
[('anne', 12), ('beth', 45), ('george', 32), ('damon', 102)]
```
那么接下来可以在循环中拆封元组：
```python
for name, age in zip(names, ages):
    print name, 'is', age, 'years old'
```
`zip`在处理不同长度序列时会以最短序列为基准：
```python
>>> zip(range(5), xrange(100000000))
[(0, 0), (1, 1), (2, 2), (3, 3), (4, 4)]
```
####编号迭代####
在一些情况下，你需要在遍历对象序列的同时访问当前访问对象的索引。例如，你可能需要替换字符串列表中所有包含`xxx`子串的字符串：
```python
for string in strings:
    if 'xxx' in string:
        index = strings.index(string) # Search for the string in the list of strings
        strings[index] = '[censored]'

# maybe a better version
index = 0
for string in strings:
    if 'xxx' in string:
        strings[index] = '[censored]'
    index += 1

# use the build-in function enumerate
for index, string in enumerate(strings):
    if 'xxx' in string:
        strings[index] = '[censored]'
```
####逆向遍历和排序遍历####
我们来看另外一组有用的函数：`reversed`和`sorted`，它们同列表方法`reverse`和`sort`类似，但它们可用于任何序列和迭代对象，还有一点是它们返回逆向和排序后的结果而不是直接修改对象：
```python
>>> sorted([4, 3, 6, 8, 3]) 
[3, 3, 4, 6, 8] 
>>> sorted('Hello, world!') 
[' ', '!', ',', 'H', 'd', 'e', 'l', 'l', 'l', 'o', 'o', 'r', 'w'] 
>>> list(reversed('Hello, world!')) 
['!', 'd', 'l', 'r', 'o', 'w', ' ', ',', 'o', 'l', 'l', 'e', 'H'] 
>>> ''.join(reversed('Hello, world!')) 
'!dlrow ,olleH'
```
###跳出循环###
####break####
```python
from math import sqrt 
for n in range(99, 0, -1): 
     root = sqrt(n) 
     if root == int(root): 
          print n 
          break
```
####continue####
```python
for x in seq: 
     if condition1: continue 
     if condition2: continue 
     if condition3: continue 

     do_something() 
     do_something_else() 
     do_another_thing() 
     etc() 

# In many cases, however, simply using an if statement is just as good
for x in seq: 
     if not (condition1 or condition2 or condition3): 
          do_something() 
          do_something_else() 
          do_another_thing() 
          etc()
```
