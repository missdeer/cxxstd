---
title: 基于范围的for循环
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 4.6
> * MSVC: 17.0
> * Clang: 3.0
>
> 提案: [N2930](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2009/n2930.html)

遍历容器是种广泛的需求，在C++11之前，有些库提供了遍历容器内所有元素的封装方法，比如[Boost](http://www.boost.org)中的[BOOST_FOREACH](http://www.boost.org/doc/libs/1_64_0/doc/html/foreach.html)，[Qt](https://www.qt.io/)中的[foreach](http://doc.qt.io/qt-5/containers.html#the-foreach-keyword)关键字等等，甚至C++自己也提供了一个[std::for_each](http://en.cppreference.com/w/cpp/algorithm/for_each)算法。

C++11提供了基于范围的for循环：

```c++
std::vector<int> coll = { 1, 2, 3};
for( int i : coll ) {
    std::cout << i << std::endl;
}
```

以上代码以值拷贝方式访问到容器coll中的每个元素，这里可以使用`auto`来自动推导容器内元素的类型：

```c++
std::vector<int> coll = { 1, 2, 3};
for( auto i : coll ) {
    std::cout << i << std::endl;
}
```

如果需要修改容器内元素的内容，则需要声明引用类型`int& i`或`auto& i`。所以当容器内元素类型是复杂数据类型时，为运行效率考虑计，一般推荐引用或常量引用方式访问：

```c++
std::vector<std::string> coll = { "element1", "element2", "element3"};
for (const auto& s : coll) {
    std::cout << s << std::endl;
}
```

一般而言，如下一组基于范围的for循环：

```c++
for ( for-range-declaration : expression ) 
  statement
```

等价于如下一组老式的for循环：

```c++
{
    auto && __range = ( expression );
    for (auto __begin = begin-expr, __end = end-expr; __begin != __end; ++__begin ) {
      for-range-declaration = *__begin;
      statement
    }
}
```

其中`__range`， `__begin`和`__end`仅用于说明，`__RangeT`是`expression`的类型，`begin-expr`和`end-expr`则依据以下规则决定：

1. 如果`__RangeT`是数组类型，则`begin-expr`和`end-expr`分别等于`__range`和`__range + __bound`，相应的`__bound`是数组边界。因此如此`__RangeT`是不知大小的数组，或者不完整类型（有声明没定义）的数组，那么程序就不合法。
2. 否则，`begin-expr`和`end-expr`分别等于`begin(__range)`和`end(__range)`，相应的`begin`和`end`会根据参数进行查找，其实基本上就是`std::begin()`和`std::end()`。
3. `__begin`和`__end`具有相同的类型，在C++17中[放宽了这个限制](../../C++17/differing-begin-and-end-types-in-range-based-for/)。



#### 相关链接

* C++17 [基于范围的for循环可以拥有不同类型的begin和end](../../C++17/differing-begin-and-end-types-in-range-based-for/)