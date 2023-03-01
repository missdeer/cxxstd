---
title: 继承构造函数
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 4.8
> * MSVC: 19.0
> * Clang: 3.3
>
> 提案: [N2540](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2008/n2540.htm)

C++11引入了一项新特性，称为“继承构造函数”（Inheriting Constructors），允许派生类继承基类的构造函数，从而可以简化派生类的构造函数实现。

当派生类没有自己的构造函数时，可以使用继承构造函数来继承基类的构造函数。例如，考虑下面这个示例：

```cpp
class Base {
public:
    Base(int x) : m_x(x) {}
private:
    int m_x;
};

class Derived : public Base {
public:
    using Base::Base;
};
```

这里，派生类`Derived`继承了基类`Base`的构造函数。在`Derived`的定义中，使用了`using`声明来继承基类的构造函数。这意味着，在使用`Derived`的构造函数时，可以像使用`Base`的构造函数一样，传递一个`int`类型的参数。

例如，我们可以这样创建一个`Derived`对象：

```cpp
Derived d(42);
```

这里，调用的是`Base`的构造函数，传递了一个`int`类型的参数`42`。由于`Derive`d没有自己的构造函数，因此编译器会自动合成一个默认构造函数。

需要注意的是，继承构造函数只能继承基类的构造函数，不能继承其它成员函数。并且，如果基类的构造函数有重载版本，需要使用`using`声明来显式指定继承哪一个版本的构造函数。

例如，如果`Base`有一个默认构造函数和一个带两个参数的构造函数：

```cpp
class Base {
public:
    Base() : m_x(0) {}
    Base(int x) : m_x(x) {}
private:
    int m_x;
};
```

那么，在`Derived`中，需要使用`using`声明来指定继承哪一个版本的构造函数。例如，我们可以这样来继承带一个`int`类型参数的构造函数：

```cpp
class Derived : public Base {
public:
    using Base::Base;
};
```

这样，`Derived`就继承了`Base`的带一个`int`类型参数的构造函数，而不是默认构造函数。