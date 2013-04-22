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
一个利用索引打印日期的例子
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
- **添加**
- **乘**
- **检查成员**
- 此外，python还支持内建函数如返回序列长度和查找最大或最小元素。
