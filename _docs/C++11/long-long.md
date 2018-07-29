---
title: long long类型
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: Yes
> * MSVC: Yes
> * Clang: Yes
>
> 提案: [N1811](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2005/n1811.pdf)

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