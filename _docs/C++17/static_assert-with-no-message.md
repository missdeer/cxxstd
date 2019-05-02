---
title: 无消息的静态断言
category: C++17
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 6
> * MSVC: 19.1
> * Clang: 2.5
>
> 提案: [N3928](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n3928.pdf)

C++11或C++14有如下代码：

```c++
static_assert(sizeof(short) == 2, "sizeof(short) == 2")
```

在C++17时可以这样写：

```c++
static_assert(sizeof(short) == 2)
```

两者达到相同的效果，都会在编译时静态断言失败时输出`static assertion failure: sizeof(short) == 2`，一个小小的语法糖，代码更简洁，少冗余。