Lists and Tuples
===========================
---
Python有6种内建的序列类型。除了本章要讲到的列表(Lists)和元组(Tuples)外，还有字符串、Unicode字符串、缓存对象和xrange对象。  

**列表和元组的主要区别在于，列表是可以修改的，而元组是不能修改的。**  

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
- **索引**  

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
**Demo: 一个利用索引打印日期的例子**
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

- **分片**  

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
**需要注意的一点是对某个序列做分片操作时，第一个索引元素一定要在第二个索引元素前面。（不指定步长的情况下）**  
另外，这两个索引是可以省略不写的。
```python
>>> numbers[-3:]
[8, 9, 10]
>>> numbers[:3]
[1, 2, 3]
>>> numbers[:]
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
**Demo: 利用分片从URL中提取域名**
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
- **添加**  

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
- **乘**  

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
**Demo: 在处于中心的盒子中打印句子**  

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
- **检查成员**
- 此外，python还支持内建函数如返回序列长度和查找最大或最小元素。
