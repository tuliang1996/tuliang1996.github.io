---
title: Python学习笔记
date: 2018-05-09 10:24:33
tags:
---

------

每日鸡汤：
有只黄鼠狼，在养鸡场的山崖顶上立了块碑，上面写着：“摆脱禁锢！不勇敢跳下去，你怎么知道自己不是一只老鹰？”于是，它每天就在崖底等着吃摔死的鸡。
        ——这个故事告诉我们，阅读心灵鸡汤时需要智商，大多鸡汤都是黄鼠狼写的。

以下学习没有任何顺序可言，完全是想记什么就加上去了，可别称为Python知识大杂烩。

<!-- more -->

------

### Python中的逻辑控制

在其他语言中可能会用到{}来控制代码块的逻辑，而Python中不使用{}，通过缩进的方式来控制逻辑。所以你需要严格注意你的缩进。约定俗成，每次缩进使用4个空格。

### Python中的变量

在c语言中，如果想要使用一个变量，需要先定义这个变量，声明它的类型，然后才可以给这个变量赋值。
在python中，我们可以直接给一个数/对象绑定一个名称。Python是动态语言。而JAVA是静态语言，静态语言在定义变量时必须指定变量类型，如果赋值的时候类型不匹配，就会报错。如下:

```java
    int a = 1;
    a = "ABC"; //error:不能把字符串赋给整型变量
```

### Python中的除法

2.X 版本中 整数相除得整数，有浮点数参与的除法结果是浮点数。
3.0以后的版本，除法都是以浮点数的形式来计算的。即10/3=3.3333333

### Python中的单引号(')、双引号(") 和三引号(""")

与其他语言（如JAVA）不同，一般情况下Python中单引号和双引号是通用的，而并非是单引号表示单个字符，双引号表示字符串的模式。
python中三引号的形式用来输出多行文本，也就是说在三引号之间输入的内容将被原样保留，其中的单号和双引号不用转义，其中的不可见字符比如\n和\t都会被保留。三引号中的所有字符被解释为纯文本。如下:
![alt python中的三引号](http://p6bx8q1l3.bkt.clouddn.com//18-5-9/85280703.jpg)

### Python中的基本数据结构

有序序列：字符串（string）、列表（lists）、元祖(tuples)
无序序列：字典（dictionary）、集合（sets）

#### 字符串

字符串是不可变类型。
在最新的Python 3版本中，字符串是以Unicode编码的。

- 对于单个字符的编码，Python提供了ord()函数获取字符的整数表示，chr()函数把编码转换为对应的字符：

![alt chr和ord](http://p6bx8q1l3.bkt.clouddn.com//18-5-9/70363111.jpg)

由于Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。
Python对bytes类型的数据用带b前缀表示。以Unicode表示的str通过encode()方法可以编码为指定的bytes。

![alt encode](http://p6bx8q1l3.bkt.clouddn.com//18-5-9/9512211.jpg)

纯英文的str可以用ASCII（或UTF-8）编码为bytes，内容是一样的（前缀为b），含有中文的str可以用UTF-8编码为bytes。含有中文的str无法用ASCII编码，因为中文编码的范围超过了ASCII编码的范围，Python会报错。

反过来，如果我们从网络或磁盘上读取了字节流，那么读到的数据就是bytes。要把bytes变为str，就需要用decode()方法。
如果bytes中包含无法解码的字节，decode()方法会报错，但可以通过传入errors='ignore'忽略错误的字节。

![alt decode](http://p6bx8q1l3.bkt.clouddn.com//18-5-9/160647.jpg)

1个中文字符经过UTF-8编码后通常会占用3个字节，而1个英文字符只占用1个字节。

![alt 编码后字节数](http://p6bx8q1l3.bkt.clouddn.com//18-5-9/75026549.jpg)

由于Python源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行：

```bash
    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
```

第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释。
第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码。申明了UTF-8编码并不意味着你的.py文件就是UTF-8编码的，文件的编码由文本编辑器决定，必须并且要确保文本编辑器正在使用UTF-8 without BOM编码。(without BOM是告诉编辑器不要在开头加上UTF-8 BOM字符，如在windows记事本中txt文件通常会在开头加上UTF-8 BOM(3个固定字节)已代表该文本是以UTF-8编码的)

在Python中，字符串的格式化方式和C语言是一致的，用%占位符实现，但你仍需注意他们的不同：

![alt c与python中的字符串格式化](http://p6bx8q1l3.bkt.clouddn.com//18-5-9/42523609.jpg)

你还可以使用字符串的.format()方法实现上述效果，它会用传入的参数依次替换字符串内的占位符{0}、{1}。另外，在占位符后使用x.x的形式可以定义整数与小数的位数。

![alt str.format](http://p6bx8q1l3.bkt.clouddn.com//18-5-9/64510794.jpg)

- string.lower() 返回一个新的字符串，将所有字母小写化。
- string.upper() 返回一个新的字符串，将所有字母大写化。
- string.replace(old,new[,count]) 返回一个新的字符串，将原字符串中的old子串替换为new，带中括号的意思是参数可缺省，缺省时全部替代。
- string. count (sub[,start[,end]]) 计算sub子串的个数，全部缺省，则从头到尾。缺省end，从start到尾。
- string.find(sub[,start[,end]]) 和 string.index(sub[,start[,end]]) 两者本质上一样的，机制也是一样的，查找第一个匹配的子串，返回其位置。但是find找不到时，返回-1。而index返回一个状态。有的时候-1可能代表的是一个有效值，所以index更不容易引起歧义。
- string.split([sep [,maxsplit]]) 当maxsplit缺省时，按sep(缺省则为空格)全部分割，否则分割maxsplit次。分割后得到一个列表。

![alt str.split](http://p6bx8q1l3.bkt.clouddn.com//18-5-9/65965831.jpg)

#### 列表

列表是可变元素，一般来说对列表所做的操作均会改变原列表。

未完待续......