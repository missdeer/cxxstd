---
title: decltype
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 4.3(v1.0), 4.8.1(v1.1)
> * MSVC: 16.0(v1.1)
> * Clang: 2.9
>
> 提案: v1.0 - [N2343](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2007/n2343.pdf)/v1.1 - [N3276](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2011/n3276.pdf)

大概是为了更好地配合`auto`进行自动类型推导，C++11引入了`decltype`操作符，用于检查某个实体（比如变量）的声明类型或表达式的类型及值分类，所以有以下用法：

```c++
decltype(entity)
```

```c++
decltype(expression)
```

括号是必不可少的，但也是需要慎重使用的，因为表达式如果被括号括着，会被当做通常的左值表达式，所以`decltype(x)`和`decltype((x))`通常会得到不同的结果。