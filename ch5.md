Conditionals, Loops, and Some Other Statements
===
---
More About print and import
---
###Printing with Commas###
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
###Importing Something As Something Else###
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
###Sequence Unpacking###
