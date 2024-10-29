---
title: 前言
---

这是一份学习笔记，记录了我学习C++11/14/17过程中遇到的各种新概念、技术、思想等，仅供有限的参考，如有任何错误或其他意见和建议，欢迎留言指出，万分感谢。

## C++03

标准文档可阅读[ISO/IEC 14882](https://cdn.jsdelivr.net/gh/missdeer/cxxstd@master/_docs/C++03/c++2003std.pdf)。

## C++11

标准文档可参考最终版草稿[N3337](https://cdn.jsdelivr.net/gh/missdeer/cxxstd@master/_docs/C++11/n3337.pdf)，它与正式版标准文档(N3338)仅有微小的[差异](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2012/n3338.html)。官方标准文档[ISO/IEC 14882:2014 (2016)](https://webstore.ansi.org/RecordDetail.aspx?sku=INCITS/ISO/IEC+14882:2014+\(2016\))已合入C++14的内容。

MSVC可以直接支持编译C++11代码，GCC和Clang需要在命令行参数指定`-std=c++11`，如：

```shell
$gcc -std=c++11 -o test main.cpp
$clang -std=c++11 -o test main.cpp
```

## C++14

标准文档可参考最终版草稿[N4140](https://cdn.jsdelivr.net/gh/missdeer/cxxstd@master/_docs/C++14/n4140.pdf)，它与正式版标准文档(N4141)仅有微小的[差异](https://github.com/cplusplus/draft/compare/n4140...n4141)。也可购买[ISO/IEC 14882:2014 (2016)](https://webstore.ansi.org/RecordDetail.aspx?sku=INCITS/ISO/IEC+14882:2014+\(2016\))。

MSVC可以直接支持编译C++14代码，GCC和Clang需要在命令行参数指定`-std=c++14`，如：

```shell
$gcc -std=c++14 -o test main.cpp
$clang -std=c++14 -o test main.cpp
```

## C++17

标准文档可参考最终版草稿（即DIS，Draft International Standard）[N4660](https://cdn.jsdelivr.net/gh/missdeer/cxxstd@master/_docs/C++17/n4660.pdf)，它与正式版标准文档（N4661）只有微小的[差异](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2017/n4661.html)。也可以购买[ISO/IEC 14882:2017](https://www.iso.org/standard/68564.html)。

MSVC 2017 15.7可以直接编译C++17代码，支持绝大多数C++17特性，GCC 8和Clang 5及更高版本支持全部C++17特性，需要在命令行参数指定`-std=c++17`，如：

```shell
$gcc -std=c++17 -o test main.cpp
$clang -std=c++17 -o test main.cpp
```

## C++20

标准文档可参考最终版草稿（即DIS，Draft International Standard）[N4860](https://cdn.jsdelivr.net/gh/missdeer/cxxstd@master/_docs/C++20/N4860.pdf)。也可以购买[ISO/IEC 14882:2020](https://www.iso.org/standard/79358.html)。

GCC和Clang需要在命令行参数指定`-std=c++20`，如：

```shell
$gcc -std=c++20 -o test main.cpp
$clang -std=c++20 -o test main.cpp
```

## C++23

标准文档可参考最终版草稿（即DIS，Draft International Standard）[N4950](https://open-std.org/jtc1/sc22/wg21/docs/papers/2023/n4950.pdf)。也可以购买[ISO/IEC 14882:2024](https://www.iso.org/standard/83626.html)。

GCC和Clang需要在命令行参数指定`-std=c++23`，如：

```shell
$gcc -std=c++23 -o test main.cpp
$clang -std=c++23 -o test main.cpp
```

## 注意

GCC和Clang除了以上`-std=c++11`, `-std=c++14`, `-std=c++17`和`-std=c++20`选项外，还有对应的GNU版本`-std=gnu++11`, `-std=gnu++14`, `-std=gnu++17`和`-std=gnu++20`，两者的区别在于后者多支持了GNU对C++语言的[扩展](https://gcc.gnu.org/onlinedocs/gcc/C_002b_002b-Extensions.html)，如果程序注意可移植性的话，比如需要用MSVC进行编译，则不应该使用[GNU扩展](https://gcc.gnu.org/onlinedocs/gcc/C_002b_002b-Extensions.html)。

