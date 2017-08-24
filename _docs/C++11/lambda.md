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
* `(parameters)`：参数列表，同普通函数相同的参数列表定义，如果没有参数，可以留空括号`()`，甚至连空括号都省略。在C++11中lambda参数类型不能自动推导，即不能为auto，在[C++14中已经支持](../../C++14/generic-plymorphic-lambda/)。
* `mutable`：默认情况下lambda表达式是一个`const`函数，但可以显式指定`mutable`取消其常量性，这时不能省略空参数列表的空括号`()`。
* `-> return-value`：返回类型，如果没有返回类型（即`void`），可以全部省略。如果返回类型明确，也可以省略，让编译器进行自动类型推导。
* `{ statement }`：函数体，跟普通函数相同的用法。

所以一个最简单的lambda表达式是这样的：

```c++
[]{}
```

尽管它什么事都没干，也没什么作用，但确实是一个合法的lambda表达式。

捕捉列表可以有0个，或1个，或多个捕捉项，以逗号分隔，可以有以下几种形式：

1. `[var]`，表示值传递方式捕捉变量`var`。
2. `[&var]`，表示引用传递方式捕捉变量`var`。
3. `[=]`，表示值传递方式捕捉所有父作用域的变量。
4. `[&]`，表示引用传递方式捕捉所有父作用域的变量。
5. `[this]`，表示捕捉当前`this`指针，但不能捕捉`*this`，但[在C++17中已经可以捕捉](../../C++17/lambda-capture-of-star-this/)。`this`也包括在`[=]`和`[&]`中。

然后可以多个捕捉项组合：

```
[=, &a, &b]
[&, a, b]
```

但是多个捕捉项不能重复：

```
[a, a]
[=, a]
[&, &a]
```

捕捉列表的不同，效果也会不同：

* 按值传递的捕捉项在lambda表达式被定义时就已经决定。
* 按引用传递的捕捉项在lambda表达式被调用时决定。
* 按值传递的捕捉变量不能被lambda表达式内修改，按引用传递的可以。
* 从代码生成角度看，如果是内建数据类型（int，short，long之类的）如果以引用传递方式会比以值传递方式多一条获取变量地址的指令。

Lambda表达式在功能上跟仿函数（functor，也称函数对象，function object）非常相似：可以保存外部变量的状态，可以传入参数，可以被调用。编译器在实现lambda表达式时也采用了与仿函数相似的方法。

每个lambda表达式都有自己特有的类型，也就是说不能仅仅因为捕捉列表、参数列表、返回值类型相同而把一个lambda表达式赋给另一个保存着lambda表达式的变量，却可以把一个保存了lambda表达式的变量赋给另一个变量：

```c++
auto f = [](int n)->int { return n;};
decltype(f) f2 = f; // 正确
decltype(f) f3 = [](int n)->int { return n;};  // 编译错误
```

Lambda表达式在C++中最典型的应用场景是作为被回调体，比如STL诸多算法需要提供谓词（predicate），简短的lambda比函数指针和仿函数都要更适合承担这份工作。

#### 相关链接

- [Lambda表达式可以捕获*this](../../C++17/lambda-capture-of-star-this/)
- [Lambda表达式的泛型和多态](../../C++14/generic-plymorphic-lambda/)

