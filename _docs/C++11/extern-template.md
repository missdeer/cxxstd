---
title: 外部模板
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 3.3
> * MSVC: 12.0
> * Clang: Yes
>
> 提案: [N1987](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2006/n1987.htm)

C++11引入了一个新特性——`extern template`。这个特性可以用于显式实例化一个模板，然后在另一个编译单元中使用它，以避免在每个编译单元中都生成一个新的实例。

在C++中，模板通常在头文件中定义。当头文件被包含到多个编译单元中时，每个编译单元都会生成一个新的模板实例。这会导致编译时间增加，并且可能会导致代码大小变大。

`extern template`可以避免这个问题。它的语法形式为：

```cpp
extern template class template-name<template-arguments>;
```

这里，`template-name`是模板的名称，`template-arguments`是模板参数。通过使用`extern template`，我们可以显式地实例化一个模板，并告诉编译器在另一个编译单元中使用它。例如：

```cpp
// 在A.cpp中显式实例化一个模板
template class std::vector<int>;

// 在B.cpp中使用该模板
void foo() {
    std::vector<int> v; // 不会在B.cpp中生成新的实例
    // ...
}
```

这里，在A.cpp中显式实例化了一个`std::vector<int>`模板，并告诉编译器在另一个编译单元中使用它。在B.cpp中使用该模板时，不会生成新的实例，而是使用在A.cpp中已经实例化的版本。

需要注意的是，使用`extern template`并不会完全替代模板的头文件定义。如果一个模板被多个编译单元使用，并且它的实现依赖于某些特定的编译器选项，那么仍然需要在头文件中定义模板。此外，`extern template`只能用于类模板，不能用于函数模板。