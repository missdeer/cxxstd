---
title: C99兼容预处理器
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 4.3
> * MSVC: 19.0
> * Clang: Yes
>
> 提案: [N1653](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2004/n1653.htm)

预处理器的改进包括几个方面。

## 扩展整型数据类型

C++11标准只定义了5种有符号整型：

1. `signed char`
2. `short int`
3. `int`
4. `long int`
5. `long long int`，[此类型为C++11中新引入并标准化](../long-long/)

同时每种有符号整型分别对应一种无符号整型，且对应的两种类型具有相同的存储空间大小。

为了满足实际编程中更多的需求，C++11标准允许编译器增加自有的扩展整型数据类型（即使不允许，人家编译器厂商这么多年来也已经这么干了），标准对扩展的类型的名称没有规定，但要求扩展的有符号整型和对应的无符号整型具有相同的存储空间大小。

另外，为了能可预期地应对隐式类型转换（类型提升），一般有如下原则的认知：

* 长度越大的整型等级越高
* 长度相同的情况下，标准整型等级高于扩展类型
* 相同大小的有符号类型和无符号类型的等级相同
* 隐式转换时一般按低等级向高等级类型转换，有符号向无符号类型转换

##  混合不同类型字符串字面量的拼接

C++11一共有5种字符串字面量：

```c++
const char * sz = "hello"; // 窄字符串
const wchar_t * wsz = L"hello"; // 传统宽字符串
const char * u8sz = u8"hello";  // C++11新增UTF-8 编码字符串
const char16_t * u16sz = u"hello";  // C++11新增UTF-16 编码字符串
const char32_t * u32sz = U"hello";  // C++11新增UTF-32 编码字符串
```

除了第一种是窄字符串外，另外几种都算宽字符串，后三种是对第二种的扩展。

在C++中两个连续的同类型字符串字面量会自动拼接成一个：

```c++
const char * sz = "hello" " world"; // 相当于赋值"hello world";
```

但是在C++11之前，宽窄不同类型字符串字面量放在一起，则是未定义行为：

```c++
const wchar_t * sz = "hello" L" world"; // C++98/03未定义行为，C++11则以宽字符串字面量拼接
```

上式在C++11中则以宽字符串字面量拼接，相当于赋值`L"hello world"`。C++11中多个字符串字面量连续定义的，不带前缀的字符串字面量全部转为带前缀的字符串字面量，比如：

```c++
const char * sz1 = "hello" u" world"; // 相当于赋值u"hello world"
const char * sz2 = u"hello" " world"; // 相当于赋值u"hello world"
```

不同类型字符串字面量的拼接也有些限制：

1. UTF-8和宽字符串字面量同时声明会有冲突
2. 其他带前缀的多个字符串字面量连接定义行为未标准化，由编译器自行定义实现

## 诊断`#include`包含的头文件的名字

是个奇怪的特性，如果`#include`语句后面包含的头文件（路径）第一个字符是个数字，则编译器会警告，比如：

```c++
// inc.cpp

#include "0x/header.h"
```

这个代码编译时大概会输出如下的警告：

```
"inc.cpp", line 1.10: 1540-0893 (W) The header file name "0x/header.h" 
in #include directive shall not start with a digit.
```

好像没什么用。

## 增加`#line`预处理指令的上限

`#line`可以重新设置当前源代码文件的行号和文件名（用于编译器warning，error之类消息输出定位）。在C++11之前，`#line`可设置的行号最大值是`32767`，在C++11中调整为`2147483647`。

## 诊断对象类的宏定义



## `_Pragma`操作符

C++标准中`#pragma`预处理器指令可以用来向编译器传递一些语言标准以外的信息，比如：

```c++
#pragma once   // 该文件只被编译一遍，一般用于头文件中
#pragma comment(lib, "user32") // 本文件代码依赖user32.lib，链接时会自动寻找并链接
#pragma warning(disable: 4996) // 禁止上报编号为4996的编译器警告
```

C++11增加了`_Pragma`**操作符**，提供了跟`#pragma`**几乎相同**的功能，格式为：`_Pragma(字符串字面量)`。所以上面的`#pragma`示例可以用`_Pragma`改写为：

```c++
_Pragma("once");
_Pragma("comment(lib, \"user32\")");
_Pragma("warning(disable: 4996)");
```

用`_Pragma`而不用`#pragma`的好处是`_Pragma`是个操作符，所以可以用于宏定义中，而`#pragma`不行：

```c++
#define COMPILED_ONCE _Pragma("once")
```
## 变长参数宏定义

C++11增加了`__VA_ARGS__`用于处理变长参数的宏定义：

```c++
#define PR(...) printf(__VA_ARGS__)
```

看来唯一的作用就是把宏定义的变长参数转为函数的变长参数。

## 预定义宏

C++11新增4个与C99兼容的预定义宏：

|                  宏名称 | 功能                                       |
| -------------------: | ---------------------------------------- |
|           `__STDC__` | 当前编译器是否和C标准实现一致，是否定义以及定义成什么值由编译器决定。      |
|   `__STDC_VERSION__` | 当前编译器支持的C标准的版本，比如`1999mmL`，是否定义以及定义成什么值由编译器决定。 |
| `__STDC_ISO_10646__` | 当前编译器是否符合某个版本的ISO/IEC 10646标准，比如`199712L`，是否定义以及定义成什么值由编译器决定。 |
|    `__STDC_HOSTED__` | 当前编译器环境是否带有完整的标准C库，非0即1。                 |

除此之外，C++11例行更新了`__cplusplus`的值为`201103L`，因此可以根据这个宏来判断当前编译环境是否支持C++11:

```c++
#if __cplusplus < 201103L
    #error "C++11 is needed"
#endif
```

另外，C++11标准化了预定义标识符`__func__`，尽管有些编译器老就支持了。`__func__`代表所在函数的名字字符串，因此如果用在函数未定义的位置（比如在函数声明的参数列表处作为参数缺少值）是编译不通过的。

常见编译器（MSVC除外，求告知！）缺省预定义宏可以通过命令行查看：

| 编译器                   | 查看C++预定义宏命令(注意添加相对应的C++标准版命令行参数)  |
| --------------------- | --------------------------------- |
| Clang/LLVM            | clang++ -dM -E -x c++ /dev/null   |
| GCC                   | g++     -dM -E -x c++ /dev/null   |
| HP C/aC++             | aCC     -dM -E -x c++ /dev/null   |
| IBM XL C/C++          | xlc++   -qshowmacros -E /dev/null |
| Intel C++             | icpc    -dM -E -x c++ /dev/null   |
| Oracle Solaris Studio | CC      -xdumpmacros -E /dev/null |

​