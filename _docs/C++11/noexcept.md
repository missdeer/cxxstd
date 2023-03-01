---
title: noexcept关键字
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 4.6
> * MSVC: 19.0
> * Clang: 3.0
>
> 提案: [N3050](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2010/n3050.html)

C++11引入了一个新的异常说明符关键字——`noexcept`。它可以用于声明一个函数不会抛出任何异常，这对于一些性能敏感的代码非常有用。

在函数声明中使用`noexcept`关键字，可以显式地指定该函数不会抛出异常。例如：

```cpp
void foo() noexcept {
    // ...
}
```

这里，函数`foo`声明为`noexcept`，表示它不会抛出任何异常。在函数体中，我们可以放心地使用一些不安全的操作，比如使用裸指针或者直接访问硬件资源等。

需要注意的是，`noexcept`并不能保证函数不会抛出异常。如果函数在运行时仍然抛出了异常，那么程序会调用`std::terminate()`函数来终止程序的执行。

`noexcept`关键字也可以用于表达式、函数指针和模板参数。例如：

```cpp
void bar() {
    int* p = new int[10](); // 分配10个int并初始化为0
    // ...
    delete[] p noexcept; // 在delete[]操作中使用noexcept
}
```

这里，在`delete[]`操作中使用`noexcept`关键字，表示如果分配内存时出现了异常，不会抛出异常而是直接调用`std::terminate()`。

需要注意的是，`noexcept`并不会影响函数的调用方式。也就是说，即使函数声明为`noexcept`，它也可以被普通的方式调用，而不是需要使用`std::noexcept()`进行调用。然而，在某些情况下，使用`noexcept`关键字可以帮助编译器进行优化，从而提高程序的性能。