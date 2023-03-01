---
title: "std::result_of和SFINAE"
category: C++14
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 5.0
> * MSVC: 19.0
> * Clang: Yes
>
> 提案: [N3462](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2012/n3462.html)

`std::result_of`是C++11标准中提供的一种类型萃取工具，用于获取函数调用结果的类型，它的定义如下：

```cpp
template <class F, class... Args>
class result_of<F(Args...)>;
```

其中，`F`是函数类型，`Args`是参数类型。`std::result_of<F(Args...)>`将返回函数调用表达式`f(args...)`的返回类型。例如：

```cpp
int func(double d, int i) { return d * i; }
using result_type = std::result_of<decltype(func)&(double, int)>::type;
static_assert(std::is_same<result_type, int>::value, "result type should be int");
```

上述代码中，`std::result_of<decltype(func)&(double, int)>::type`返回了函数`func`在调用时的返回类型，即`int`。这样我们就可以在编译时获取函数调用表达式的返回类型，进而实现更加灵活和复杂的模板元编程。

`std::result_of`的实现依赖于SFINAE技术。在调用`std::result_of<F(Args...)>`时，编译器会先尝试调用一个辅助函数，该函数接收一个参数类型为F，参数类型为`Args...`的参数列表，然后根据调用结果的类型，推导出`std::result_of<F(Args...)>`的返回类型。如果调用辅助函数失败，编译器会尝试使用其他函数重载，直到找到匹配的函数。

SFINAE技术是通过函数重载和模板参数推导失败来实现的。在调用一个函数时，编译器会首先进行函数匹配，尝试找到与函数调用参数类型和个数最匹配的函数。如果找到了匹配的函数，则调用该函数。否则，编译器会尝试使用其他函数重载，如果所有的函数重载都无法匹配，则编译器将会报错。

SFINAE技术通过在模板参数推导时失败来实现函数重载的选择。例如，如果在模板参数推导时某个参数类型无法匹配，编译器会认为这个模板实例化不可行，并尝试使用其他模板实例化。这样就可以根据不同的模板实例化，选择不同的函数重载。

因此，在使用`std::result_of`和SFINAE技术时，需要注意模板参数推导的情况，以避免出现不必要的错误。同时，需要了解模板元编程的相关知识，以便更好地理解这些技术的实现原理。

## SFINAE

SFINAE是C++模板元编程中的一种技术，全称为Substitution Failure Is Not An Error，即“替换失败不是一个错误”。这个技术是指在编译时，如果模板实例化的过程中发生了替换失败的情况，编译器不会报错，而是会尝试使用其他的模板实例化，这样就可以通过一系列判断选择最合适的模板实例。

SFINAE通常用于解决函数重载中的一些问题。在C++中，函数重载是通过函数的参数类型和个数来区分的。当函数调用时，编译器会尝试匹配所有可行的重载函数，如果找到了匹配的函数，就会调用该函数。但是，当函数重载的参数类型过于复杂，或者函数模板的参数类型不确定时，编译器可能无法确定最佳匹配的函数。这时，SFINAE技术就可以派上用场了。

SFINAE的实现通常是通过模板参数推导失败来实现的。例如，假设我们有一个模板函数，用于检查某个类型是否具有一个成员函数：

```cpp
template <typename T>
struct has_member_function_foo {
    template <typename U>
    static auto test(U* p) -> decltype(p->foo(), std::true_type{});
    static auto test(...) -> std::false_type;
    static constexpr bool value = decltype(test((T*)nullptr))::value;
};
```

这个模板函数中，我们使用了两个重载的test函数。第一个`test`函数尝试访问T类型的一个名为foo的成员函数，如果可以访问，则返回一个`std::true_type`类型的值。第二个`test`函数是一个`catch-all`函数，用于处理访问T类型的`foo`成员函数失败的情况。在实际调用时，我们可以使用类似下面的方式来判断某个类型是否具有一个名为`foo`的成员函数：

```cpp
struct A { void foo() {} };
struct B {};

static_assert(has_member_function_foo<A>::value == true, "A has foo");
static_assert(has_member_function_foo<B>::value == false, "B doesn't have foo");
```

在上面的示例中，我们使用`static_assert`语句来检查是否能够访问某个类型的`foo`成员函数。如果访问成功，则`has_member_function_foo`的value成员将被设置为`true`，否则将被设置为`false`。

需要注意的是，SFINAE技术需要谨慎使用，因为它可能会导致模板实例化的过程变得非常复杂。在实际使用中，建议使用简单的技巧，比如函数重载和`if constexpr`语句，来实现类似的功能。