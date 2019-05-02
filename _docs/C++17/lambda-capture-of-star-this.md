---
title: Lambda表达式可以捕获*this
category: C++17
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 7
> * MSVC: 19.1
> * Clang: 3.9
>
> 提案: [P0018R3](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0018r3.html)

C++11和C++14中，lambda只能捕获`this`的引用，这在并行程序或异步程序中可能会很难处理好`this`指向的对象的生命周期，C++17中允许捕获`*this`，也就是以值类型捕获，lambda就能访问到`this`指向的对象的另一份拷贝：

```c++
struct MyObj {
  int value {123};
  auto getValueCopy() {
    return [*this] { return value; };
  }
  auto getValueRef() {
    return [this] { return value; };
  }
};
MyObj mo;
auto valueCopy = mo.getValueCopy();
auto valueRef = mo.getValueRef();
mo.value = 321;
valueCopy(); // 123
valueRef(); // 321
```

要注意的是`[&,this]`这样的写法也是可以的，目的是兼容C++11和C++14的代码，但这样写是多余的，因为`[&]`和`[=]`隐含捕获了`this`。新的写法可能是`[=, *this]`。