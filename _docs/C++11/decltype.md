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

括号是必不可少的，但也是需要慎重使用的，因为表达式如果被括号括着，会被当做通常的左值表达式，所以`decltype(x)`和`decltype((x))`通常会得到不同的结果：

```c++
int i;
decltype(i) a;  // a: int
decltype((i)) b; // b: int &，无法编译通过
```

一般说来，C++11按以下规则来返回`decltype(x)`的类型：

1. 如果`x`是一个没有带括号的标记符表达式（id-expression）或者类成员访问表达式，那么`decltype(x)`就是`x`所命名的实体的类型。如果`x`是一个被重载的函数，则会导致编译错误。
2. 否则，假设`x`的类型是`T`，如果`x`是一个将亡值（xvalue），那么`decltype(x)`结果为`T&&`。
3. 否则，假设`x`的类型是`T`，如果`x`是一个左值（lvalue），那么`decltype(x)`结果为`T&`。
4. 否则，假设`x`的类型是`T`，`x`是一个纯右值（prvalue），那么`decltype(x)`结果为`T`。

## 左值、右值（将亡值、纯右值）

C++11之前，程序内的值分为左值（lvalue）和右值（rvalue）两种：

1. 可以取地址的，有名字的就是左值
2. 不能取地址的，没有名字的就是右值

比如`a = b + c`中，`a`就是左值，`b + c`就是右值，粗略地可以通过看它是在赋值表达式中赋值符的左边还是右边。

C++11中，右值（rvalue）又细分为将亡值（xvalue，eXpiring value）和纯右值（prvalue，pure rvalue）：

1. 纯右值就是C++11之前标准中的右值
2. 将亡值是C++11新增的跟右值引用相关的表达式，比如返回右值引用`T&&`的函数返回值，`std::move`的返回值，转换为`T&&`的类型转换函数的返回值