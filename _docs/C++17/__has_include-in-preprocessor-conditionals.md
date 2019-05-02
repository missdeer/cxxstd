---
title: "预定义处理器增加__has_include"
category: C++17
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 5.0
> * MSVC: 19.1
> * Clang: Yes
>
> 提案: [P0061R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0061r1.html)

可用于在预处理阶段检测某个头文件是否存在并可被include：

```c++
#if __has_include(<optional>)
#  include <optional>
#  define have_optional 1
#elif __has_include(<experimental/optional>)
#  include <experimental/optional>
#  define have_optional 1
#  define experimental_optional 1
#else
#  define have_optional 0
#endif
```