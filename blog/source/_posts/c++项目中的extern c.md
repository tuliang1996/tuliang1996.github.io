---
title: 'c++项目中的extern c{}'
date: 2018-04-16 09:44:13
tags:
---

------

在用C++的项目源码中，经常会不可避免的会看到下面的代码：

```C++
    #ifdef __cplusplus
    extern "C" {
    #endif

    /*...*/

    #ifdef __cplusplus
    }
    #endif
```

<!-- more -->

下面以几个方面介绍一下它的作用：
1、#ifdef _cplusplus/#endif _cplusplus及发散
2、extern "C"
    2.1、extern关键字
    2.2、"C"
    2.3、小结extern "C"
3、C和C++互相调用
    3.1、C++的编译和连接
    3.2、C的编译和连接
    3.3、C++中调用C的代码
    3.4、C中调用C++的代码
4、C和C++混合调用特别之处函数指针

------

在用C++的项目源码中，经常会不可避免的会看到下面的代码：

```C++
    #ifdef __cplusplus
    extern "C" {
    #endif

    /*...*/

    #ifdef __cplusplus
    }
    #endif
```

### 1、#ifdef _cplusplus/#endif _cplusplus及发散

\#ifdef/#endif、#ifndef/#endif用于条件编译，#ifdef _cplusplus/#endif _cplusplus表示如果定义了宏_cplusplus，就执行#ifdef/#endif之间的语句，否则就不执行。

为什么需要#ifdef _cplusplus/#endif _cplusplus呢？因为C语言中不支持extern "C"声明，在.c文件中包含了extern "C"时会出现编译时错误。

条件编译的另外一个作用就是避免重复包含头文件。看下面的一段代码：

```C++
    #ifndef MONGOOSE_HEADER_INCLUDED
    #define MONGOOSE_HEADER_INCLUDED

    #ifdef __cplusplus
    extern "C" {
    #endif /* __cplusplus */

    /*.................................
    * do something here
    *.................................
    */

    #ifdef __cplusplus
    }
    #endif /* __cplusplus */

    #endif /* MONGOOSE_HEADER_INCLUDED */
```

这个头文件mongoose.h可能在项目中被多个源文件包含（#include "mongoose.h"），而对于一个大型项目来说，这些冗余可能导致错误，因为一个头文件可能包含类定义或inline内联函数，在一个源文件中mongoose.h可能会被#include两次（如，a.h头文件包含了mongoose.h，而在b.c文件中#include a.h和mongoose.h），就可能会出错（在同一个源文件中一个结构体、类等被定义了两次）。

从逻辑观点和减少编译时间上，都要求去除这些冗余。然而让程序员去分析和去掉这些冗余，不仅枯燥且不太实际，最重要的是有时候又需要这种冗余来保证各个模块的独立。

为了解决这个问题，条件编译就起作用了，如果定义了MONGOOSE_HEADER_INCLUDED，#ifndef/#endif之间的内容就被忽略掉。因此，编译时第一次看到mongoose.h头文件，它的内容会被读取且给定MONGOOSE_HEADER_INCLUDED一个值。之后再次看到mongoose.h头文件时，MONGOOSE_HEADER_INCLUDED就已经定义了，mongoose.h的内容就不会再次被读取了。

### 2、extern "C"

分为两部分讨论：extern关键字、"C"。

#### 2.1、extern关键字

extern是C/C++语言中表明函数和全局变量作用范围（可见性）的关键字，该关键字告诉编译器，其声明的函数和变量可以在本模块或其它模块中使用。通常，在模块的头文件中对本模块提供给其它模块引用的函数和全局变量以关键字extern声明。

例如，如果模块B欲引用该模块A中定义的全局变量和函数时只需包含模块A的头文件即可。这样，模块B中调用模块A中的函数时，在编译阶段，模块B虽然找不到该函数，但是并不会报错；它会在连接阶段中从模块A编译生成的目标代码中找到此函数。

与extern对应的关键字是static，被它修饰的全局变量和函数只能在本模块中使用。因此，一个函数或变量只可能被本模块使用时，其不可能被extern修饰。

#### 2.2、"C"

一个典型的C++程序可能包含其它语言编写的部分代码。类似的，C++编写的代码片段可能被使用在其它语言编写的代码中。不同语言编写的代码互相调用是困难的，甚至是同一种语言编写但不同的编译器编译的代码。例如，不同语言和同种语言的不同实现可能会在注册变量、保持参数和参数在栈上的布局等不一样。

为了使它们遵守统一规则，可以使用extern指定一个编译和连接规约。例如，声明C和C++标准库函数strcyp()，并指定它应该根据C的编译和连接规约来链接：

```C++
    extern "C" char* strcpy(char*,const char*);
```

extern "C"指令中的C，表示的一种编译和连接规约，而不是一种语言。C表示符合C语言的编译和连接规约的任何语言，如Fortran、assembler等。

另外，extern "C"指令仅指定编译和连接规约，但不影响语义。例如在函数声明中，指定了extern "C"，但仍然要遵守C++的类型检测、参数转换规则。

#### 2.3、小结extern "C"

通过上面两节的分析，我们知道extern "C"的真实目的是实现类C和C++的混合编程。在C++源文件中的语句前面加上extern "C"，表明它按照类C的编译和连接规约来编译和连接，而不是C++的编译和连接规约。这样在类C的代码中就可以调用C++的函数或变量等。（注：我在这里所说的类C，代表的是跟C语言的编译和连接方式一致的所有语言）

### 3、C和C++互相调用

既然知道了extern "C"是实现的类C和C++的混合编程。下面我们就分别介绍如何在C++中调用C的代码、C中调用C++的代码。首先要明白，C和C++互相调用，得知道它们之间的编译和连接差异，及如何利用extern "C"来实现相互调用。

#### 3.1、C++的编译和连接

C++是一个面向对象的语言，它支持函数的重载，重载这个特性给我们带来了很大的便利。为了支持函数重载的这个特性，C++编译器实际上将下面这些重载函数：

```C++
    void print(int i);
    void print(char c);
    void print(float f);
    void print(char* s);
```

编译为：

```C++
_print_int
_print_char
_print_float
_pirnt_string
```

这样的函数名，来唯一标识每个函数。注意，不同的编译器实现可能不一样，但是都是利用这种机制。所以当连接是调用print(3)时，它会去查找_print_int(3)这样的函数。

也正是因为这点，重载被认为不是多态，多态是运行时动态绑定（“一种接口多种实现”），如果硬要认为重载是多态，它顶多是编译时“多态”。

C++中的变量，编译时也类似，如全局变量可能编译g_xx，类变量编译为c_xx等。连接是也是按照这种机制去查找相应的变量。

#### 3.2、C的编译和连接

C语言中并没有重载和类这些特性，故并不像C++那样print(int i)，会被编译为_print_int，而是直接编译为_print等。因此如果直接在C++中调用C的函数会失败，因为连接需要调用C中的print(3)时，它会去找_print_int(3)。因此extern "C"的作用就体现出来了。

#### 3.3、C++中调用C的代码

假设一个C的头文件cHeader.h中包含一个函数print(int i)，为了在C++中能够调用它。

头文件代码如下

```C
    #ifndef C_HEADER
    #define C_HEADER

    extern void print(int i);

    #endif C_HEADER
```

源文件cHeader.c的代码为

```C
    #include <stdio.h>
    #include "cHeader.h"
    void print(int i)
    {
        printf("cHeader %d\n",i);
    }
```

在C++.cpp文件中引用cHeader.c中的print(int i)函数,代码为

```C++
    extern "C"{
    #include "cHeader.h"
    }

    int main()
    {
        print(3);
        return 0;
    }
```

#### 3.4、C中调用C++的代码

cppHeader.h头文件中定义了下面的代码

```C++
    #ifndef CPP_HEADER
    #define CPP_HEADER

    extern "C" void print(int i);

    #endif CPP_HEADER
```

源文件cppHeader.cpp文件中代码如下

```C++
    #include "cppHeader.h"

    #include <iostream>
    using namespace std;
    void print(int i)
    {
        cout<<"cppHeader "<<i<<endl;
    }
```

在C的代码文件c.c中调用print(int i)函数

```C
    extern void print(int i);
    int main()
    {
        print(3);
        return 0;
    }
```

注意在C的代码文件中的extern void print(int i)。