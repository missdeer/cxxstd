---
title: Lambda表达式
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 4.5(v1.1)
> * MSVC: 16.0
> * Clang: 3.1
>
> 提案: v0.9 - [N2550](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2008/n2550.pdf)/v1.0 - [N2658](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2008/n2658.pdf)/v1.1 - [N2927](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2009/n2927.pdf)

Lambda表达式就是匿名函数，在C++11之前Boost凭借C++语言强大的template和预处理宏，以及库作者强悍的奇技淫巧实现了[Boost.lambda](http://www.boost.org/doc/libs/1_65_0/doc/html/lambda.html)和更高级用法[Boost.phoenix](http://www.boost.org/doc/libs/1_65_0/libs/phoenix/)，但没有语言层面的支持，完全用库实现不但稍显累赘，而且代码观感不佳。C++11在语言层面实现了lambda，语法定义如下：

```c++
[capture](parameters)mutable -> return-value { statement }
```

* `[capture]`：捕捉列表，可以捕捉上下文中的变量以供lambda表达式使用。
* `(parameters)`：参数列表，同普通函数相同的参数列表定义，如果没有参数，可以留空括号`()`，甚至连空括号都省略。
* `mutable`：默认情况下lambda表达式是一个`const`函数，但可以显式指定`mutable`取消其常量性，这时不能省略空参数列表的空括号`()`。
* `-> return-value`：返回类型，如果没有返回类型（即`void`），可以全部省略。如果返回类型明确，也可以省略，让编译器进行自动类型推导。
* `{ statement }`：函数体，跟普通函数相同的用法。

所以一个最简单的lambda表达式是这样的：

```c++
[]{}
```

尽管它什么事都没干，也没什么作用，但确实是一个合法的lambda表达式。