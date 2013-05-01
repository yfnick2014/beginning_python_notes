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
通常当你从模块导入时，可以使用`import somemodule`或`from somemodule import somefunction`或`from somemodule import somefunction, anotherfunction, yetanotherfunction`或`from somemodule import *`
