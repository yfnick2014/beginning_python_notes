Instant Hacking: The basics
===========================
---
Hello, world!
------------
输出`Hello, world!`的python程序
``` print "Hello, world!"
```

Say Hello to yourself
---------------------
```# input your name
name = raw_input("What is your name? ")
print "Hello, " + name + "!"
```
保存为hello.py，打开终端执行`python hello.py`
> `#`为行注释

`str`和`repr`
------------
`str`只是转换值为人能理解的字符串；而`repr`会创建一个等值的字符串，是合法的Python表达式，可以被解释器识别。

`input` vs. `raw_input`
-----------------------
`input`输入为合法的Python表达式，如`"Gumby"`；而`raw_input`输入为原始数据，并自动转换为字符串。

长字符串，原始字符串和Unicode字符串
-----------------------------
长字符串用`` ``` ``符号，可以包含多行内容。例如：
``print ```This is a very long String.
It continues here.
And it's not over yet.
"Hello, world!"
Still here.``` ``
原始字符串不会将`\`作为特殊字符处理，例如：
``print r`C:\nowhere` ``输出`C:\nowhere`
Unicode字符串存储为16位Unicode，表示方法为``u`Hello, world!` ``。