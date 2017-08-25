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

* 扩展整型数据类型。

  C++11增加了`long long`和`unsigned long long`类型，其实在这之前有些编译器实现和一些第三方库也都提供了类似的类型。在C++11中`long long`类型在不同平台具有不同的长度，但至少是64位（8字节）。在写常数字面量时，可以使用`LL`后缀（或是`ll`）标识一个`long long`类型的字面量，相对地，用`ULL`（或`ull`， `Ull`， `uLL`）标识一个`unsigned long long`类型字面量，比如：

  ```c++
  long long lli = -1000000000000000LL;
  long long int lli2 = -1000000000000000LL;
  unsigned long long ulli = -1000000000000000ULL;
  unsigned long long int ulli2 = -1000000000000000ULL;
  ```

  C++11中有一些与`long long`等价的类型定义，比如`long long int`，`signed long long`和`signed long long int`。相对的，`unsigned long long`与`unsigned long long int`也是等价的。

  在`<climits>`或`<limits.h>`中定义了`LLONG_MAX`， `LLONG_MIN`和`ULLONG_MAX`三个宏，分别代表在该编译器支持的平台上`long long`的最大值和最小值以及`unsigned long long`的最大值。

  在用标准库函数`printf`进行格式化输出时，则分别使用`%lld`和`%llu`来标识`long long`和`unsigned long long`类型的变量。

* 混合不同字符串字面量的拼接。

* 诊断头文件和包含的名字。

* 增加`#line`预处理指令的上限。

* 诊断对象类的宏定义。

* `_Pragma`操作符。

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

* 变长参数宏定义和空宏参数。

* 预定义宏。

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