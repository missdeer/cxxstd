---
title: 自动类型推导
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 4.4(v1.0)
> * MSVC: 16.0(v0.9)
> * Clang: Yes
>
> 提案: [N1984(v1.0)](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2006/n1984.pdf)

自动类型推导是降低编码负担的一种手段，在其他编程语言比如[Go中也有类似的功能](https://go.dev/ref/spec#Short_variable_declarations)。C++11修改了关键字`auto`原有的作用，使其为自动类型推导所用。在C++11之前，template就已经具有类型推导能力，而C++11中增加的`auto`通常情况下就是使用template的类型推导方式：

```c++
int main(int argc, char *argv[]) 
{
  auto x = 17; // x = int
  auto& y = x; // y = int&
  const auto& i = x; // i = const int&
  auto z = y; // z = int
  const auto f = y; // f = const int
  auto& m = i; // m = const int&
  auto&& t = i; // t = int&
  auto&& t1 = 10; // t1 = int&& 
  return 0;
}
```

```c++
int main(int argc, char *argv[]) 
{
  int A[3] = {1, 2, 3};
  auto a = A; // a = int *
  auto& a1 = A; // a1 = int (&)[3]
  return 0;
}
```

还有了新式的函数声明形式：

```c++
auto get_fun(int arg) -> double (*)(double) // same as: double (*get_fun(int))(double)
{
    switch (arg)
    {
        case 1: return std::fabs;
        case 2: return std::sin;
        default: return std::cos;
    }
}
```

`auto`代替了声明变量时的强制类型信息如`int`，`int&`，`const int&`，`int&&`等等，但是在使用`auto`时：

1. 必须初始化变量，当然好的编码实践即使不用`auto`也会要求声明变量时立即初始化
2. 函数返回值不能是`auto`（仅C++11而言，[C++14中已经修改](../../C++14/decltype(auto)-return-type-deduction-for-normal-functions/)）
3. 函数的形参类型不能是`auto`，即使指定了默认参数也不行（仅C++11而言，[C++14中lambda表达式部分已经修改](../../C++14/generic-plymorphic-lambda/)）
4. `auto`不能声明为模板的参数（仅C++11而言？）
5. 非静态成员变量不能是`auto`的，即使指定了初始值也不行
6. 不能声明`auto`数组

另外，由于C++11新增了[初始化列表](../initializer-lists/)功能，使用`auto`自动推导出来的类型可能会有出乎意料的情况：

```c++
int main(int argc, char *argv[]) 
{
  int a0 = (1); // a0 = int
  auto a1 = (1); // a1 = int
  int a2 = {1}; // a0 = int
  auto a3 = {1}; // a3 = std::initializer_list<int>
  return 0;
}
```

`a1`如愿得到`int`类型，而`a3`却跟`a2`是不同的类型，因为初始化列表拥有特有的类型`std::initializer_list<int>`。

一般而言，在以下场合使用`auto`比较合适：

1. 类型名比较长，比如`std::vector<std::string>::iterator`， `std::shared_ptr<std::vector>`等等诸如此类的要声明变量可以使用`auto`

2. 类型名比较难书写，比如临时定义一个[lambda表达式](../lambda/)，又要把它保存到一个变量中以便之后调用，就可以用`auto`：

   ```c++
   auto f = [&](int n)->int { return n+1; };
   auto i = f(10);
   ```

3. [Scott Meyers](https://en.wikipedia.org/wiki/Scott_Meyers)在[《Effective Modern C++》](https://www.amazon.com/Effective-Modern-Specific-Ways-Improve/dp/1491903996)中提议优先使用`auto`，这也许是一种趋势。

## 相关链接

* [decltype](../decltype/)
* [Lambda表达式](../lambda/)
* [初始化列表](../initializer-lists/)
* [普通函数返回值类型推导](../../C++14/decltype(auto)-return-type-deduction-for-normal-functions/)
* [Lambda表达式的泛型和多态](../../C++14/generic-plymorphic-lambda/)
* [获取变量精确类型的示例（GCC，Clang可用）](http://cpp.sh/6puaz)

