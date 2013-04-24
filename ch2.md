Lists and Tuples
===========================
---
Python有6种内建的序列类型。除了本章要讲到的列表(Lists)和元组(Tuples)外，还有字符串、Unicode字符串、缓存对象和xrange对象。  

*列表和元组的主要区别在于，列表是可以修改的，而元组是不能修改的。*  

序列在处理一组数据时相当有用，比如在数据库中就可以用序列来表示人的信息（姓名和年龄），可以表示为列表的形式如下：  
```python
>>> edward = ['Edward Gumby', 42]
>>> john = ['John Smith', 50]
>>> database = [edward, john]
>>> database
[['Edward Gumby', 42], ['John Smith', 50]]
```
序列基本操作
-----------
###索引###
序列中所有的元素从0开始编号，可以用数字索引来访问单个元素，如:  
```python
>>> greeting = 'Hello'  
>>> greeting[0]
'H'
```
Python也可以用负数索引来访问元素.从序列中最右侧的元素开始计数，向左依次减1，其中最右测元素索引为-1.  
```python
>>> greeting[-1]
'o'
```
*Demo: 一个利用索引打印日期的例子*
```python
# Print out a date, given year, month, and day as numbers

months = [
    'January',
    'February',
    'March',
    'April',
    'May',
    'June'
    'July',
    'August',
    'September',
    'October',
    'November',
    'December'
]

# A list with one ending for each number from 1 to 31
endings = ['st', 'nd', 'rd'] + 17 * ['th'] \
        + ['st', 'nd', 'rd'] + 7 * ['th'] \
        + ['st']

year    = raw_input('Year: ')
month   = raw_input('Month (1-12): ')
day     = raw_input('Day (1-31): ')

month_number = int(month)
day_number = int(day)

# Remember to subtract 1 from month and day to get a correct index
month_name = months[month_number-1]
ordinal = day + endings[day_number-1]

print month_name + ' ' + ordinal + ', ' + year
```
运行结果
```
Year: 1974
Month (1-12): 8
Day (1-31): 16
August 16th, 1974
```

###分片###
分片可以提取序列中的部分元素。最简单的方法可以通过给定两个索引，并用`:`分隔开。
```python
>>> tag = '<a href="http://www.python.org">Python web site</a>'
>>> tag[9:30]
'http://www.python.org'
>>> tag[32:-4]
'Python web site'
```
可以看出第一个索引表示序列中开始包含的第一个元素索引，而第二个索引则表示分片后面的第一个索引。也就是说，第一个索引是分片内的元素，而第二个索引是分片外的元素。
```python
>>> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> numbers[3:6]
[4, 5, 6]
>>> numbers[0:1]
[1]
```
*需要注意的一点是对某个序列做分片操作时，第一个索引元素一定要在第二个索引元素前面。（不指定步长的情况下）*  
另外，这两个索引是可以省略不写的。
```python
>>> numbers[-3:]
[8, 9, 10]
>>> numbers[:3]
[1, 2, 3]
>>> numbers[:]
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
*Demo: 利用分片从URL中提取域名*
```python
# Split up a URL of the form http://www.something.com

url = raw_input('Please enter the URL: ')
domain = url[11:-4]

print "Domain name: " + domain
```
运行结果
```
Please enter the URL: http://www.python.org
Domain name: python
```
在Python2.3开始对分片添加了第三个参数（步长），这个参数用来指定分片中相邻元素之间索引的距离，默认情况下为1.
```python
>>> numbers[0:10:1]
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> numbers[0:10:2]
[1, 3, 5, 7, 9]
>>> numbers[::4]
[1, 5, 9]
# 逆序
>>> numbers[8:3:-1]
[9, 8, 7, 6, 5]
>>> numbers[::-2]
[10, 8, 6, 4, 2]
```
###添加###
相同类型的序列可以用`+`来实现序列的串联
```python
>>> [1, 2, 3] + [4, 5, 6]
[1, 2, 3, 4, 5, 6]
>>> 'Hello, ' + 'world!'
'Hello, world!'
>>> [1, 2, 3] + 'world!'
Traceback (innermost last):
File "<pyshell#2>", line 1, in ?
[1, 2, 3] + 'world!'
TypeError: can only concatenate list (not "string") to list
```
###乘###
序列与一个数x相乘，会创建该序列重复x次的新序列。
```python
>>> 'python' * 5
'pythonpythonpythonpythonpython'
>>> [42] * 10
[42, 42, 42, 42, 42, 42, 42, 42, 42, 42]
```
> `None`为Python中的常量，表示为空。可以利用`None`来初始化固定长度的列表。  

```python
>>> sequence = [None] * 10
>>> sequence
[None, None, None, None, None, None, None, None, None, None]
```
*Demo: 在处于中心的盒子中打印句子*  

```python
# Print a sentence in a centered "box" of correct Width

# Note that the integer division operator (//) only works in Python
# 2.2 and newer. In earlier versions, simply use plain division (/)

sentence = raw_input("Sentence: ")

screen_width = 80
text_width = len(sentence)
box_width = text_width + 6
left_margin = (screen_width - box_width) // 2

print
print ' ' * left_margin + '+'   + '-' * (box_width-2) + '+'
print ' ' * left_margin + '|  ' + ' ' * text_width    + '  |'
print ' ' * left_margin + '|  ' +       sentence      + '  |'
print ' ' * left_margin + '|  ' + ' ' * text_width    + '  |'
print ' ' * left_margin + '+'   + '-' * (box_width-2) + '+'
print
```
运行结果
```
Sentence: He's a very naughty boy!

                         +----------------------------+
                         |                            |
                         |  He's a very naughty boy!  |
                         |                            |
                         +----------------------------+
```
###检查成员###
检查元素是否在序列中，使用`in`布尔操作符。`in`操作符返回布尔值：`True`为真，`False`为假。
```python
>>> permissions = 'rw'
>>> 'w' in permissions
True
>>> 'x' in permissions
False
>>> users = ['mlh', 'foo', 'bar']
>>> raw_input('Enter your user name: ') in users
Enter your user name: mlh
True
>>> subject = '$$$ Get rich now!!!$$$'
>>> '$$$' in subject
True
```
*Demo: 检查用户名和PIN码*
```python
# Check a user name and PIN code

database = [
    ['albert', '1234'],
    ['dilbert', '4242'],
    ['smith', '7524'],
    ['jones', '9843']
]

username = raw_input('User name: ')
pin = raw_input('PIN code: ')

if [username, pin] in database: print 'Access granted'
```
###其他###
此外，python还支持内建函数如返回序列长度和查找最大或最小元素。对应的函数分别为`len`、`max`和`min`。
```python
>>> numbers = [100, 34, 678]
>>> len(numbers)
3
>>> max(numbers)
678
>>> min(numbers)
34
>>> max(2, 3)
3
>>> min(9, 3, 2, 5)
2
```
列表：Python的主力
------------------
###`list`函数###
因为字符串不像列表可以修改，有时可以采用`list`函数从字符串创建一个列表。
```python
>>> list('Hello')
['H', 'e', 'l', 'l', 'o']
```
> *注意`list`可以处理所有种类的序列，不仅仅只是字符串。*
>
> 转换字符列表为字符串，可以使用下面这个表达式：
> ''.join(somelist)  

###列表基本操作###
列表除了可以进行基本的序列操作之外，还可以支持其他操作，比如列表项赋值、删除列表项、分片赋值和列表方法。
####修改列表：列表项赋值####
```python
>>> x = [1, 1, 1]
>>> x[1] = 2
>>> x
[1, 2, 1]
```
> 需要注意的是，你不可以给不存在的列表项赋值。  

####删除元素####
可以使用`del`删除元素
```python
>>> names = ['Alice', 'Beth', 'Cecil', 'Dee-Dee', 'Earl']
>>> del names[2]
>>> names
['Alice', 'Beth', 'Dee-Dee', 'Earl']
```
####分片赋值####
分片是一个非常强大的特性，而分片赋值显得更为强大。
```python
>>> name = list('Perl')
>>> name
['P', 'e', 'r', 'l']
>>> name[2:] = list('ar')
>>> name
['P', 'e', 'a', 'r']
```
当然，你也可以使用长度不同的序列来替换分片。
```python
>>> name = list('Perl')
>>> name[1:] = list('ython')
>>> name
['P', 'y', 't', 'h', 'o', 'n']
```
分片赋值也可以用来插入元素，即不会替换其他原有元素。
```python
>>> numbers = [1, 5]
>>> numbers[1:1] = [2, 3, 4]
>>> numbers
[1, 2, 3, 4, 5]
```
相反，我们也可以删除分片。
```python
>>> numbers
[1, 2, 3, 4, 5]
>>> numbers[1:4] = []
>>> nubmers
[1, 5]
```
####列表方法####
这里涉及一个新的概念：方法。如果学习过面向对象编程，你会明白方法和函数是不同的概念。
列表包含一些方法，可以用来检验或修改列表内容。
- **append**  

`append`方法用来追加一个对象到列表末尾。
```python
>>> lst = [1, 2, 3]
>>> lst.append(4)
>>> lst
[1, 2, 3, 4]
```
- **count**  

`count`方法计算元素在列表中的个数。
```python
>>> ['to', 'be', 'or', 'not', 'to', 'be'].count('to')
2
>>> x = [[1, 2], 1, 1, [2, 1, [1, 2]]]
>>> x.count(1)
2
>>> x.count([1, 2])
1
```
- **extend**  

`extend`方法用来一次追加多个值，即一个序列。
```python
>>> a = [1, 2, 3]
>>> b = [4, 5, 6]
>>> a.extend(b)
>>> a
[1, 2, 3, 4, 5, 6]
```
和加法操作不同的是，加法操作不修改序列的值，而是返回一个新的序列。
```python
>>> a = [1, 2, 3]
>>> b = [4, 5, 6]
>>> a + b
[1, 2, 3, 4, 5, 6]
>>> a
[1, 2, 3]
# 下面才和extend方法类似，不同的是extend方法修改原来的序列而不是重新赋值
>>> a = a + b
```
- **index**  

`index`方法用来搜索列表中元素第一次出现的索引。
```python
>>> knights = ['We', 'are', 'the', 'knights', 'who', 'say', 'ni']
>>> knights.index('who')
4
>>> knights.index('herring')
Traceback (innermost last):
File "<pyshell#76>", line 1, in ?
knights.index('herring')
ValueError: list.index(x): x not in list
```
- **insert**  

`insert`方法用来向列表插入对象。
```python
>>> numbers = [1, 2, 3, 5, 6, 7]
>>> numbers.insert(3, 'four')
>>> numbers
[1, 2, 3, 'four', 5, 6, 7]
```
- **pop**  

`pop`方法从列表中删除一个元素（默认最后一个），并返回删除的元素。
```python
>>> x = [1, 2, 3]
>>> x.pop()
3
>>> x
[1, 2]
>>> x.pop(0)
1
>>> x
[2]
```
> `pop`方法是list方法中唯一一个既修改列表又返回值的方法。  
> 利用`pop`和`append`方法可以实现栈操作，而利用`pop`和`insert`方法可以实现队列操作。  

- **remove**  

`remove`方法用来删除第一次出现的值。
```python
>>> x = ['to', 'be', 'or', 'not', 'to', 'be']
>>> x.remove('be')
>>> x
['to', 'or', 'not', 'to', 'be']
>>> x.remove('bee')
Traceback (innermost last):
File "<pyshell#3>", line 1, in ?
x.remove('bee')
ValueError: list.remove(x): x not in list
```
- **reverse**  

`reverse`方法用来反转列表中的元素。
```python
>>> x = [1, 2, 3]
>>> x.reverse()
>>> x
[3, 2, 1]
```
> 建议：如果你希望对序列进行反向迭代，可以使用`reversed`函数。该函数不返回列表，而是返回一个迭代器。你可以用`list`函数转换返回的对象。
```python
>>> x = [1, 2, 3]
>>> list(reversed(x))
[3, 2, 1]
```   

- **sort**  

`sort`方法用来对列表进行排序。
```python
>>> x = [4, 6, 2, 1, 7, 9]
>>> x.sort()
>>> x
[1, 2, 4, 6, 7, 9]
```
由于`sort`方法修改`x`但不返回值，如果要保留排序后的结果到另一个变量，可以按照如下方法。
```python
>>> x = [4, 6, 2, 1, 7, 9]
>>> y = x[:]
>>> y.sort()
>>> x
[4, 6, 2, 1, 7, 9]
>>> y
[1, 2, 4, 6, 7, 9]
```
需要指出一点的是`x[:]`分片操作返回列表的一份拷贝，而如果直接将`x`赋值给`y`则指向同一个列表。
```python
>>> y = x
>>> y.sort()
>>> x
[1, 2, 4, 6, 7, 9]
>>> y
[1, 2, 4, 6, 7, 9]
```
另外一种获取列表排序后的拷贝可以使用`sorted`函数。
```python
>>> x = [4, 6, 2, 1, 7, 9]
>>> y = sorted(x)
>>> x
[4, 6, 2, 1, 7, 9]
>>> y
[1, 2, 4, 6, 7, 9]
```
- **高级排序**  

所谓高级排序，就是可以按照你指定的排序方式来对列表进行排序。  
你可以自定义排序函数，按照`compare(x, y)`的格式，当`x < y`时返回负数，当`x > y`时返回正数，当`x == y`是返回0.接下来，你便可以将函数名作为参数提供给`sort`方法。  
内建的`cmp`函数支持默认的排序行为。
```python
>>> cmp(42, 32)
1
>>> cmp(99, 100)
-1
>>> cmp(10, 10)
0
>>> numbers = [5, 2, 9, 7]
>>> numbers.sort(cmp)
>>> numbers
[2, 5, 7, 9]
```
`sort`方法有两个额外的参数：`key`和`reverse`，正常你可以通过名称来指定它们。
```python
>>> x = ['aardvark', 'abalone', 'acme', 'add', 'aerate']
>>> x.sort(key=len)
>>> x
['add', 'acme', 'aerate', 'abalone', 'aardvark']
```
另外一个关键字参数`reverse`只是一个真值（True或False），用来表示列表是否以逆序排序。
```python
>>> x = [4, 6, 2, 1, 7, 9]
>>> x.sort(reverse=True)
>>> x
[9, 7, 6, 4, 2, 1]
```
