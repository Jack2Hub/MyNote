

# C++基础

## 变量和基本类型

### 复合类型
- &
  - **C语言和C++** 中的 **&** 实际不是同一个东西
  - C语言中 **&** 是取地址
  - C++中的 **&** 是在C++11中新增的**右值引用**
    > 故有时候用**gcc**编译C语言时会出现`error: expected identifier or ‘(’ before ‘&’ token`  
    > 需要换成**g++**编译

### const限定符
- extern const int a = add(); // add() 返回常量
  - C语言不允许这种写法 `error: initializer element is not constant`
  - C++可以这么写
    ``` C++
    extern const int a = add(); // 包含了初始值，故该语句是定义
    extern const int a;         // 无初始值，表示声明，定义在其他地方
    ```

- const int * 和 int * const
  > 从右往左阅读，先跟const结合表示指针指向的位置不变，先跟*结合表示不能通过指针修改指向的值  
  > **注意是不能通过指针去修改，并不是不能修改原先变量的值**
  - const int *: 指针常量，即不能通过指针修改指针指向的值，但可以修改指针的值，即修改指针指向的位置
  - int * const: 常量指针，即不能修改指针指向的位置，但可以修改指针指向的值
  ``` C++
    int a = 100;
    int b = 1000;
    const int* a_ = &a;                 // err: *a_ = 999; OK: a_ = &b;
    // int * const a_ = &a;             // err: a_ = &b;   OK: *a_ = 999;
    // const int * const a_ = &a;       // err: *a_ = 999; a_ = &b;
    cout << "a = " << a << "  *a_ = " << *a_ << endl;

    a = 1000;
    cout << "a = " << a << "  *a_ = " << *a_ << endl;
  ```

- 顶层const 和 底层const
  - 顶层const表示 **任意对象是常量，对任何数据类型均适用**， **对指针来说表示指针本身是一个常量**
  - 底层const表示 **指针所指的对象是一个常量**
  ``` C++
    int i = 101;
    int* const p1 = &i;         // 不能改变p1的值，是顶层const
    const int ci = 42;          // 不能改变ci的值，是顶层const
    const int* p2 = &ci;        // 允许改变p2的值，是底层const
    const int* const p3 = p2;   // 从右往左，依次是顶层const和底层const
    const int& r = ci;          // **用于声明引用的const都是底层const，即不能用其去改变原先变量的值**
  ```

### 处理类型
#### auto类型说明符
> auto可以让编译器通过 **初始值** 来推算变量的类型，故auto定义的变量必须要有初始值
- auto 一般会忽略顶层const，保留底层const属性
  ``` C++
  int i = 0;
  const int ci = i, &cr = ci;
  auto b = ci;      // b是一个整数，ci的顶层const特性被忽略掉了
  auto c = cr;      // c是一个整数，cr是ci的别名
  auto d = &i;      // d是一个整型指针，&在这是取地址
  auto e = &ci;     // e是一个指向常量整数的指针（对常量对象取地址是一种底层const）
  
  // 若希望推断出的auto类型是一个顶层const，则需要明确指出
  const auto f = ci; // ci是int，推演出f是const int
  ```

#### decltype类型指示符
> 仅希望从表达式中推断出定义类型，但不想使用表达式的值，C++11新标准引入了decltype  
> 其作用是选择并返回操作数的数据类型，但不具体计算表达式的值  
- `decltype(func()) sum = x;` // sum的类型就是func返回值的类型


## 字符串、向量和数组
> string:   支持可变长字符串  
> vector:   支持可变长的集合  
> 迭代器:   是string和vector的配套类型，常被用于访问string中的字符或vector中的元素  
> 内置数组: 一种更基础的类型，string和vector都是对它的抽象类型  

### 命名空间的using声明
> `std::cin` 表示编译器应从操作符左侧名字所示的作用域中寻找右侧那个名字，即要使用命名空间std中的名字cin

- using namespace::name; // 有了using声明就无须专门的前缀（形如命名空间::）
  - `using namespace::cin;`
  - `using namespace::cout;`
  - `using namespace::endl;`
  * **头文件一般不包含using声明**

### 标准库类型 string
``` C++
#include <string>
using namespace std;
```

#### string对象的定义/初始化
``` C++
string s1;
string s2(s1);
string s2 = s1; // 同上
string s3("value");
string s3 = "value"; // 同上
string s4(10, 'c');
```
- 直接初始化/拷贝初始化
``` C++
string s5 = "hello";  // 拷贝初始化
string s6("hello");   // 直接初始化
string s7(10, 'c');   // 直接初始化，这种初始化多个值的情况只能用直接初始化，除非专门显示创建一个(临时)对象来用于拷贝，如下：
string s8 = string(10, 'c');
```

#### string对象的操作
- 


















# C++标准库
# 类设计者的工具
# 高级主题