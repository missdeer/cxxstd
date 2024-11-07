---
title: 常量表达式
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 4.6
> * MSVC: 19.0
> * Clang: 3.1
>
> 提案: [N2235](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2007/n2235.pdf)

## 背景

在C++11之前，程序中的常量表达式主要依赖于宏定义和const变量。但这些方式都存在一些局限性：
- 宏定义不具备类型安全性，且容易导致代码可维护性降低
- const变量虽然不可修改，但并不保证在编译期可计算
- 一些需要编译期常量的场景（如数组大小、模板参数等）无法使用更灵活的表达式

为了解决这些问题，C++11引入了constexpr关键字，用于声明可以在编译期计算的常量表达式。

## 基本用法

### constexpr变量

constexpr变量必须用常量表达式初始化：

```cpp
constexpr int size = 42; // OK
constexpr int double_size = size 2; // OK
constexpr int random = rand(); // 错误：rand()不是常量表达式
```

### constexpr函数

constexpr函数在编译期可计算，但也可以在运行时使用：

```cpp
constexpr int square(int x) {
    return x x;
}
// 编译期计算
constexpr int sq_5 = square(5); // sq_5 = 25，编译期确定
// 运行时计算
int some_value = get_value();
int sq_val = square(some_value); // 运行时计算
```

## 使用场景

### 1. 数组维度声明

```cpp
constexpr int get_array_size() { return 42; }
int array[get_array_size()]; // OK，编译期可确定大小
```

### 2. 模板参数

```cpp
template<int N>
struct Array {
    int data[N];
};
constexpr int size = 10;
Array<square(size)> arr; // OK，square(size)在编译期计算
```

### 3. 编译期计算提升性能

```cpp
constexpr int fibonacci(int n) {
    return (n <= 1) ? n : fibonacci(n-1) + fibonacci(n-2);
}
// 编译期就计算出结果
constexpr int fib_10 = fibonacci(10);
```

## 限制条件

C++11中的constexpr函数有较多限制：
- 函数体只能包含一条return语句
- 只能调用其他constexpr函数
- 不能包含循环或分支语句
- 不能有副作用（如修改静态或全局变量）

注意：这些限制在C++14中得到了很大放松。

## 最佳实践

1. 当需要编译期常量时，优先使用constexpr而不是宏定义
2. 设计编译期可计算的函数时，尽量使用constexpr声明
3. constexpr函数应保持简单，避免复杂的逻辑
4. 记住constexpr函数可以在运行时使用，这提供了很好的灵活性

## 与其他特性的对比

### constexpr vs const

1. 计算时机
- const：仅表示运行时常量，不可修改
- constexpr：表示编译期常量，编译器会在编译期计算其值

2. 使用场景

```cpp
// const示例
const int val1 = 42; // OK
const int val2 = get_value(); // OK，运行时初始化
int arr1[val1]; // 错误：数组大小需要编译期常量
int arr2[val2]; // 错误：数组大小需要编译期常量
// constexpr示例
constexpr int val3 = 42; // OK
constexpr int val4 = get_value(); // 错误：需要编译期常量表达式
int arr3[val3]; // OK，val3是编译期常量
```

3. 函数声明

```cpp
// const成员函数：表示不会修改对象状态
class MyClass {
    int value;
    public:
    int getValue() const { return value; } // 运行时执行
};
// constexpr函数：可在编译期执行
constexpr int square(int x) { return x x; } // 编译期执行
```

4. 主要区别总结：
- const主要用于：
  * 声明运行时不可修改的变量
  * 声明不修改对象状态的成员函数
  * 声明指向常量的指针或引用
  
- constexpr主要用于：
  * 声明编译期可计算的常量
  * 声明编译期可执行的函数
  * 用在需要编译期常量的场景（如数组大小、模板参数等）

### constexpr vs #define

1. 类型安全性

```cpp
#define MAX_SIZE 100 // 无类型检查
constexpr int MaxSize = 100; // 有类型检查
void foo(short s) {
    #define SQUARE(x) ((x) (x))
    short s1 = SQUARE(s); // 可能溢出，无警告
    constexpr auto square = {
        return x x;
    };
    short s2 = square(s); // 编译器会进行类型检查
}
```

2. 作用域
- #define：无作用域概念，全局生效
- constexpr：遵循正常的作用域规则

3. 调试友好性
- #define：预处理阶段处理，调试时看不到原始值
- constexpr：是正常的变量和函数，调试时可以看到具体值

4. 主要优势对比：

- #define：
  * 简单的文本替换
  * 可用于条件编译
  * 预处理阶段处理
  
- constexpr：
  * 类型安全
  * 支持作用域
  * 可以使用复杂表达式
  * 调试友好
  * 支持函数和类成员

## 使用建议

1. 优先级顺序：
   - 需要编译期常量时：首选constexpr
   - 仅需要运行时常量时：使用const
   - 仅在确实需要预处理器功能时：使用#define

2. 场景选择：
   - 模板参数：使用constexpr
   - 数组大小：使用constexpr
   - 运行时只读变量：使用const
   - 条件编译：使用#define

## 总结

constexpr是C++11引入的重要特性，它为编译期计算提供了类型安全和更好的可维护性。通过合理使用constexpr，我们可以：
- 在编译期进行更多计算，提升运行时性能
- 实现更灵活的编译期常量表达式
- 保证类型安全性
- 提高代码的可维护性

随着C++14、C++17的发展，constexpr的功能更加强大，使用场景也更加广泛。