---
title: C++ 与面向对象 (缩减版)
date: 2021-7-10 12:00:00 +0800
categories: [Program, Language]
tags: [code, program, c plus plus]     # TAG names should always be lowercase
math: true
toc: true
---
> 本博客是基于中国科学技术大学的马建辉老师的 PPT 修改而成

# 1. 函数

## 1.1. 函数的定义和声明

- 函数：本质上是一段参数化的共享代码

- 函数的定义：``<类型><函数名>(<参数表>){<若干条语句>}``

  ```c++
  #include <iostream>
  using namespace std;
  
  //函数的定义
  double sum(double x, double y)
  {
      return x + y;
  }
  
  int main()
  {
      double a, b;
      cout << "Input double a and b :";
      cin >> a >> b;
      double s = sum(a, b);
      cout << "sum =" << s << endl;
  }
  ```

  

- 函数的声明

  - “一遍扫描”的需要。原则：
    - 定义在先，调用在后，调用前可以不必声明
    - 定义在后，调用在前，调用前必须声明
  - ``<类型> <函数名>(<参数表>);``

```cpp
int add(int, int);	//函数的声明
int main()
{ 
	int i = 10, j = 20;
    // ....;
    j = add(i, j);
    // ....;
}
int add(int a, int b){   
    return(a + b);
}
```

## 1.2. 函数的调用与参数传递

函数调用语句中的函数名经编译后变成函数的入口地址！

### 1.2.1. 函数的调用方法

- 语法：``<函数名>(<实参表>)`` 

  ```c
  c = sum(a, b);
  swap(&a, &b);
  ```

- 关键：“虚实结合”的问题，即：

  - 实在参数的值如何传递给形式参数？
  - 对形式参数的值的改变是否影响实在参数？

- 调用序列：
  1. 分配函数局部空间(活动记录)
     2. **参数传递**
     3. 保存返回地址
     4. 控制转移
     5. 执行函数
   - 返回序列：
     1. 返回值传递
     2. 控制转移
     3. 返回主调函数

### 1.2.2. 参数传递

**实参<->形参**

- 值传递：把实际参数的值复制给形式参数，也就是说，<u>形参和实参是两个不同的内存空间</u>。函数所处理的仅仅是实际参数的一个拷贝。

  ```cpp
  #include <iostream>
  using namespace std;
  void swap1(int x, int y)
  {
      int temp;
      temp = x;
      x = y;
      y = temp;
      cout << "x=" << x << ",y=" << y << endl;
  }
  int main()
  {
      int a = 5, b = 9;
      swap1(a, b);
      cout << "a=" << a << ",b=" << b << endl;
  }
  /* 
   * output:
   * x=9,y=5
   * a=5,b=9
   *
   * a,b未改变
   */
  ```

  

- 引用传递：把实际参数的地址传递给形式参数，也就是说，<u>形参和实参实际上是同一个内存空间</u>。函数通过对形式参数的处理来达到对实际参数进行加工的目的。

  ```cpp
  #include <iostream>
  using namespace std;
  void swap2(int *x, int *y)
  {
      int temp;
      temp = *x;
      *x = *y;
      *y = temp;
      cout << "x=" << *x << ",y=" << *y << endl;
  }
  
  int main()
  {
      int a = 10, b = 20;
      swap2(&a, &b);
      cout << "a=" << a << ",b=" << b << endl;
  }
  
  /*
   * output:
   * x=20,y=10
   * a=20,b=10
   *
   * a,b的值被交换
   */
  ```



传值:vs:传指针：

- 本质上都是值传递
- 两者分别适用于：
  - 当需要将一个类的大的对象作为参数传递给函数时
  - 当参数值必须被修改后返回时



另一种引用传递：

- 函数定义：
  - 形参声明为引用量
  - 函数体直接对引用量操作
- 函数调用：
  - 形式上像值传递，实际上是引用传递
  - 形参是实参的别名，并没有另外开辟空间
  - 对形参的修改直接影响到实参

```cpp
void swap3(int &v1, int &v2)
{
    int temp = v2;
    v2 = v1;
    v1 = temp;
}
int main()
{
    int a = 5, b = 9;
    swap3(a, b);
    cout << "a=" << a << ",b=" << b << endl; 
}

/*
 * output:
 * a=9,b=5
 *
 * a,b的值被交换
 */
```

- 优点：

  - 形式参数只是实参的别名，因此函数调用时不需要为它分配空间，尤其是传递大的对象。

  - 在函数内对参数值的改变不再作用于局部拷贝，而是直接针对实参；但是相对于指针传递而言，引用传递的函数体定义和函数调用的形式更为简洁。

    ```cpp
    void swap2(int *x, int *y)
    {
        int temp = *x;
        *x = *y;
        *y = temp;
    }
    void swap3(int &v1, int &v2)
    {
        int temp = v2;
        v2 = v1;
        v1 = temp;
    }
    ```

    

使用数组作函数参数：

1. 形参和实参都用数组
2. 形参和实参都用对应数组的指针
3. 实参用数组名，形参用引用

### 1.2.3. 函数参数的求值顺序

例如下面的程序：（不想被打死在实际中就不要写这样的代码）

```cpp
#include <iostream>
using namespace std;

int add_int(int x, int y)
{
    return x + y;
}

int main()
{
    int x = 4, y = 6;
    int z = add_int(++x, x + y);
    cout << z << endl;
}
```

若函数参数的求值顺序为**从右向左**：z为15

若函数参数的求值顺序为**从左向右**：z为16

程序的输出为：

```
15
```

说明<u>函数参数的求值顺序为从右向左</u>。

### 1.2.4. ^*^返回语句的实现过程

```cpp
type fun(arg_list)
{
    statements;
return <expression>;
}
```

- 返回语句：``return <表达式>``
  1. 求``<表达式>``的值
  2. 如果``<表达式>``的类型与函数类型不相同，将表达式的类型**自动转换**为函数的类型。(注意：这种转换是**强制性的**，可能出现信息丢失的情况)
  3. 将求出的表达式值返回给调用函数作为调用函数的值
  4. 将控制权由被调用函数返回主调用函数，执行主调用函数下一条语句。

## 1.3. 作用域

### 1.3.1. 作用域

- 作用域：标识符的可见范围
- 种类：
  - 程序级（全局）
  - 文件级（``code.cpp``）
  - 函数级（``func(){...}``）
  - 块级（``{...}``）

### 1.3.2. 作用域的屏蔽规则

在某个作用范内定义的标识符在该范围内的子范围内可以定义定义新的同名标识符。这时原定义的标识符在子范围内是**不可见**的，但是它还是存在的，只是在子范围内由于出现了同名的标识符而被暂时隐藏起来。过了子范围后，它又是可见的。

```cpp
int x;
int main()
{
    int a;
    {
        int a;
        a = 10;
        ::a = 20;	// ::全局作用域分辨符
    }
}
```



- 全局作用域分辨符::

  ```cpp
  #include <iostream>
  using namespace std;
  
  const int max = 65000;
  const int lineLength = 10;
  
  void fibonacci(int max)
  {
      if (max < 2)
          return;
      for (int i = 3, v1 = 0, v2 = 1, cur; i <= max; ++i)
      {
          cur = v1 + v2;
          if (cur > ::max) // 全局作用域分辨符
              break;
          cout << cur << " ";
          v1 = v2;
          v2 = cur;
          if (i % lineLength == 0)
              cout << endl;
      }
  }
  
  int main()
  {
      int max = 100;
      fibonacci(max);
  }
  ```

  输出为：

  ```
  1 2 3 5 8 13 21 34
  55 89 144 233 377 610 987 1597 2584 4181
  6765 10946 17711 28657 46368
  ```

  显然没有计算Fibonacci数列的第100位。

### 1.3.3. 局部变量与全局变量

|          | 局部变量                 | 全局变量               |
| -------- | ------------------------ | ---------------------- |
| 作用域   | 函数级/块级              | 程序级/文件级          |
| 生命周期 | 自动类变量，内部静态变量 | 外部变量，外部静态变量 |

```cpp
#include <iostream>
using namespace std;
int a = 10; // 全局变量
void other();
void f()
{
    int a = 3;
    register int b = 5; //寄存器、局部变量
    static int c;       // 静态局部变量
    cout << "a =" << a << ","
         << "b =" << b << ","
         << "c = " << c;
    other();
    other();
}

void other()
{
    int a = 5;          //局部变量
    static int b = 12;  // 静态局部变量
    a += 10;
    b += 20;
    cout << "a=" << a << ","
         << "b=" << b << endl;
}

```

- 文件级作用域

  ```cpp
  #include <iostream>
  using namespace std;
  void fun1(), fun2(), fun3();
  int i = 5;
  int main()
  {
      i = 20;
      fun1();
      cout << "main():i=" << i << endl;
      fun2();
      cout << "main():i=" << i << endl;
      fun3();
      cout << "main():i=" << i << endl;
  }
  ```

  `file1.cpp`

  ```cpp
  #include <iostream>
  using namespace std;
  int i;
  void func1()
  {
      i = 50;
      cout << "func1:i(static)=" << i << endl;
  }
  ```

  `file2.cpp`

  ```cpp
  #include <iostream>
  using namespace std;
  void func2()
  {
      int i = 15;
      cout << "func2:i(auto)=" << i << endl;
      if (i)
      {
          extern int i;
          cout << "func2:i(extern)=" << i << endl;
      }
  }
  extern int i;
  void fun3()
  {
      int i = 30;
      cout << "func3:i(extern)=" << i << endl;
      if (i)
      {
          int i = 10;
          cout << "func3:i(auto)=" << i << endl;
      }
  }
  ```

### 1.3.4. 内部静态变量

- 初始化一次
- 作用域是<u>局部于函数的</u>
- 生命期是<u>全局的（程序级）</u>

```cpp
#include <iostream>
using namespace std;
int traceGcd(int v1, int v2)
{
    int i;
    static int depth = 1;   //内部静态变量
    cout << "depth#" << depth++ << endl;
    if (v2 == 0)
        return v1;
    return traceGcd(v2, v1 % v2);
}
```

### 1.3.5. 寄存器变量

在函数中频繁使用的局部变量可以定义为寄存器变量(用`register`修饰)，只要还有寄存器可用，编译器会尽可能将其放入寄存器中，提高执行速度。

```cpp
for(register int i = 0; i < s2; i++)
{
    ia[i] = i;
}
```

### 1.3.6. 小结

- 同名的标识符可在不同域中重复出现，分别具有不同含义和用途，而不会引发任何不良效果。

  ```cpp
  #include <iostream>
  using namespace std;
  void swap(int *ia, int, int);
  void sort(int *ia, int sz);
  void putValue(int *ia, int sz);
  int ia[] = {4, 7, 0, 9, 2, 5, 8, 3, 6, 0};
  const int sz = 10;
  int main()
  {
      int i, j;
      //…
      swap(ia, i, j);
      sort(ia, sz);
      putValue(ia, sz);
  }
  ```

  

- 在某个文件中要引用其他文件中定义的外部变量，用`extern`加以说明；

- 在不同的程序文件中定义了同名的外部变量，假若它们具有不同的含义及用途。在单独编译每一个文件时都是正确的，而连接时编译器会认为是“重复定义”。

  - 解决方法：定义为静态连接的，用`static`修改全局量。

## 1.4. 内联函数

**对宏替换的改进！**

### 1.4.1. 内联函数（inline function）

- 内联函数：告诉编译器，在编译时用函数代码的“拷贝“替换函数调用。

  ```cpp
  inline int power_int(int x)
  {
      return x * x;
  }
  
  int main()
  {
      for (int i = 1; i <= 10; i++)
      {
          int p = power_int(i);   //编译时换成 (i*i)
          cout << i << "*" << i << "=" << p << endl;
      }
  }
  ```

- 优点：

  - 提高代码效率
  - 保持源代码可读性及易维护性

- 限制：

  - 在内联函数内不允许用<u>循环语句</u>和<u>开关语句</u>
  - 由于要进行源代码替换，内联函数的定义必须出现在**每一个**调用该函数的源文件之中；
  - 编译器不保证所有被定义为`inline`的函数编译成内联函数，例如递归函数

### 1.4.2. 内联函数:vs:宏替换

| 内联函数                                 | 宏替换                     |
| ---------------------------------------- | -------------------------- |
| 内联函数由编译器处理                     | 宏由预处理器处理           |
| 编译器会对内联函数调用的实参进行类型检查 | 编译器不会对宏替换进行检查 |
| 内联函数不会有副作用                     | 宏可能会有副作用           |

```cpp
#define abs(n) ((n) < 0 ? -(n) : (n))
i = 1;
j = abs(--i); // j = ((--i) < 0 ? -(--i) : (--i));

#define power(n) (n * n)
k = power(1 + 2); // k = (1 + 2 * 1 + 2);
```

### 1.4.3. 内联函数:vs:直接代码

|          | 内联函数                       | 直接代码                                   |
| -------- | ------------------------------ | ------------------------------------------ |
| 可读性强 | `max(a, b);`                   | `(a > b ? a : b);`                         |
| 复用性好 | `max(a, b);`<br />`max(c, d);` | `(a > b ? a : b);`<br />`(c > d ? c : d);` |

内联函数便于修改，易于维护。

## 1.5. 默认参数

**C++函数的高级机制之一**

### 1.5.1. 设置函数参数的默认值

`DefaultArgument.h`

```cpp
typedef struct time
{
    int h; // hour
    int m; // minute
    int s; // second
} * TIME, CLOCK;

int setclock(TIME t, int hour = 12, int minute = 0, int second = 0)
{
    t->h = (hour   <= 23 && hour   >= 0) ? hour : 12;
    t->m = (minute <= 59 && minute >= 0) ? minute : 0;
    t->s = (second <= 59 && second >= 0) ? second : 0;
}
```

`DefaultArgument.cpp`

```cpp
#include <iostream>
using namespace std;

int main()
{
    CLOCK t1, t2, t3, t4, t5;

    setclock(&t1);            // t1=12:00:00
    setclock(&t2, 7);         // t2=07:00:00
    setclock(&t3, 14, 20);    // t3=14:20:00
    setclock(&t4, 18, 30, 0); // t4=18:30:00
    setclock(&t5, 19, , 0);   // 非法
}
```

说明：

- 若有实际参数值，则缺省值无效；否则，参数值就采用缺省值

  ```cpp
  setclock(&t1);			// t1=12:00:00
  setclock(&t2, 7);		// t2=07:00:00
  setclock(&t3,14,20);	// t3=14:20:00
  ```

- 带有缺省值的参数必须全部放置在参数的最后，即在带有缺省值的参数的右边不再出现**无**缺省值的参数；

  ```cpp
  int setclock(TIME t, int hour = 12, int minute, int second) //非法
  {
      //statements
  }
  ```

- 函数的参数既可以在定义又可以在声明中指明，<u>一旦定义了缺省参数，就不能再定义它</u>，但可以添加一个或多个缺省参数。

  ```cpp
  void f(int a, int b, int c =0);	// 定义缺省参数c 
  void f(int a, int b=1, int c=0);	// 增加缺省参数b
  void f(int a, int b=2, int c=1);	// 错误，企图重定义b和c的缺省参数   
  ```

### 1.5.2. 占位符参数

- 函数声明时，参数可以没有标识符，称这种参数为“**占位符参数**”，或称“**哑元**”。

  ```cpp
  // 声明
  void f(int x, int = 0 , float = 1.1);
  
  // 定义
  void f(int x, int , float flt)
  {
      //statements
  }
  
  // 调用时必须为占位符参数提供一个值
  f(1); 
  f(1,2,3.0);
  ```

- 优点：增强代码的可维护性

  将来修改函数功能需要增加一个形式参数时，可以利用该哑元，保证函数的接口不发生变化，从而不需要修改程序中的函数调用。例如：

  ```cpp
  long min(long a, long b, long )
  {
      //statements
  }
  int main(){
  	long a = 100, b=200;       
      long c = min(a, b, MAXNUMBER); 
  }
  ```

  修改后：

  ```cpp
  long min(long a, long b, long c) 
  {
      //statements
  }
  int main(){
      long a = 100, b=200;       
      long c = min(a, b, MAXNUMBER);
      long d = min(a, b, c);
  }
  ```

## 1.6. 函数重载

**优于占位符，实际上是多个同名函数**

### 1.6.1. 重载的概念

- 重载：把多个功能类似的函数定义成同名函数

  ```cpp
  int main()
  {
      int a, b, c;
      cout << "max(a,b)=" << max(a, b) << endl;
      cout << "max(a,b,c)=" << max(a, b, c) << endl;
  }
  ```

### 1.6.2. 重载的实现

核心问题：

- 由于是静态关联，编译器怎么确定应执行哪一个函数。 

- ```cpp
  c = max(a, b);
  d = max(a, b, c);
  ```

  <u>编译器根据什么来区分调用哪个函数?</u>

  - 函数名：相同，无法区分
  - 返回值类型：语句中不出现返回值类型，也无法区分
  - 参数：参数的个数和类型不同，可以区分。

1. 参数类型不同的重载函数

   ```cpp
   #include <iostream>
   using namespace std;
   
   int add(int, int);
   double add(double, double);
   
   int main()
   {
       cout << add(5, 10) << endl;
       cout << add(5.1, 10.5) << endl;
   }
   
   int add(int x, int y)
   {
       return x + y;
   }
   
   double add(double a, double b)
   {
       return a + b;
   }
   ```

   输出为

   ```
   15
   15.6
   ```

2. 参数个数不同的重载函数

   ```cpp
   #include <iostream>
   using namespace std;
   
   int min(int a, int b);
   int min(int a, int b, int c);
   int min(int a, int b, int c, int d);
   
   int main()
   {
       cout << min(7, 5) << endl;
       cout << min(-2, 8, 0) << endl;
       cout << min(13, 5, 4, 9) << endl;
   }
   
   int min(int a, int b)
   {
       return a < b ? a : b;
   }
   
   int min(int a, int b, int c)
   {
       int t = min(a, b);
       return min(t, c);
   }
   
   int min(int a, int b, int c, int d)
   {
       int t1 = min(a, b);
       int t2 = min(c, d);
       return min(t1, t2);
   }
   ```

   输出为

   ```
   5
   -2
   4
   ```

3. 参数次序不同的重载函数

   其实是参数类型不同的一种特例

   ```cpp
   char * get(char *s, char ch);
   char * get(char ch, char *s);
   ```

### 1.6.3. 重载函数的“二义性”错误

- 返回值类型不同，参数相同

  ```cpp
  int print(int);
  double print(int);
  
  int main()
  {
      int a = 10, b = 20;
      print(a); // 二义性
      print(b); // 二义性
      //调用哪个print？
  }
  ```

- 仅用了const或引用造成的参数类型不同

  ```cpp
  int print(const int &);
  int print(int);
  
  int main()
  {
      int a = 10, b = 20;
      print(a); // 二义性
      print(b); // 二义性
      //调用哪个print？
  }
  ```

- 使用了修饰符造成的的参数类型不同的函数，可能引起二义

  ```cpp
  void print(unsigned int);
  void print(int);
  
  int main()
  {
      print(1l); // 二义性,1l既可转换成unsigned int，又可转换成int。
      print(1u); // OK,无二义
  }
  ```

- 缺省参数引起的二义性

  ```cpp
  void print(int a, int b = 1);
  void print(int a);
  
  int main()
  {
      print(1); // 二义性
  }
  ```

### 1.6.4. 编译器如何处理重载函数

- C的函数编译后通常为`_cdecl_fun();`

  如：

  `int max(int, int);`

  编译后为：

  `int _cdecl_max()(0040260);`

- 为了支持重载，C++的函数编译后通常为`_cppdecl_fun_parameter_...();`

  如：

  `int max(int, int);` 
  `int max(int, int, int);`

  编译后为：
  `int _cppdecl_max_int_int()(0080330);` 
  `int _cppdecl_max_int_int_int()(0100280);` 

- 关于extern “C” 声明：强制C++编译器按C的函数命名规则去连接相应的目标代码。

  ```cpp
  extern "C"
  max(a, b);
  ```

  编译为：

  ```
  call _cdecl_max()(0040260);
  ```

---

# 2. 类：数据抽象

## 2.1. 数据抽象

- C语言的数据描述方法存在的问题
- 改进：从`struct`->`class`的转变
- 对象与抽象数据类型
- 对象的细节
- 头文件的形式
- 嵌套结构

### 2.1.1. 选择语言的若干因素

- 效率：该语言实现的程序运行得比较快
- 安全性：该语言有助于确信程序实现了我们的意图，同时具有很强的纠错能力。
- 可维护性：该语言能帮助我们创建易理解、易修改和易扩展的代码。
- **成本**：该语言能让我们投入较少的人、在较短的时间内编写出较复杂的、较重要的及无bug的代码。（<u>核心</u>）



- 对于C语言来说，降低开发成本，提高生产效率的最好的方法是使用“库”（`Lib`）
- C++旨在提供更加易于使用的高效的“库”

### 2.1.2. C库

**一组`struct`+一组作用在这些`struct`上的函数**

- 一个袖珍的C库：`stash`

  - 该库提供一种数据结构`stash`，对`stash`的操作像数组，但`stash`的空间动态申请、动态释放，其长度在运行时确定，并且可以动态扩充。

  - 需要定义的结构体和函数集如下：

    ```c
    struct Stash;		// 结构体Stash
    
    void initialize();	// 初始化Stash;
    void cleanup();		// 撤销Stash;
    int add();			// 往Stash 添加元素  
    void* fetch();		// 访问Stash中的元素
    int count();		// 返回Stash中的元素个数 
    void inflate();		// 当Stash满时，动态扩充Stash
    ```

    

  - 具体文件：

    <u>库的接口</u>`CLib.h`

    ```cpp
    //结构体CStash
    typedef struct CStashTag
    {
        int size;     // Size of each space
        int quantity; // Number of storage spaces
        int next;     // Next empty space
        // Dynamically allocated array of bytes:
        unsigned char *storage;
    } CStash;
    
    void initialize(CStash *s, int size);
    void cleanup(CStash *s);
    int add(CStash *s, const void *element);
    void *fetch(CStash *s, int index);
    int count(CStash *s);
    void inflate(CStash *s, int increase);
    ```

    <u>库的实现</u>`CLib.cpp`

    ```cpp
    #include "CLib.h"
    #include <iostream>
    #include <cassert>
    using namespace std;
    
    // Quantity of elements to add when increasing storage
    const int increment = 100;
    
    void initialize(CStash *s, int sz)
    { //初始化函数，很重要！
        s->size = sz;
        s->quantity = 0;
        s->storage = NULL;
        s->next = 0;
    }
    int add(CStash *s, const void *element)
    { //插入元素
        if (s->next >= s->quantity)
            inflate(s, increment); //若空间不足，则动态扩充
    
        // Copy element into storage,
        // starting at next empty space:
        int startBytes = s->next * s->size;
        unsigned char *e = (unsigned char *)element;
        for (int i = 0; i < s->size; i++)
            s->storage[startBytes + i] = e[i];
        s->next++;
        return (s->next - 1); // Index number
    }
    int count(CStash *s)
    {                   // 隐藏了实现，便于修改
        return s->next; // Elements in CStash
    }
    void *fetch(CStash *s, int index)
    { // 返回所需元素的地址
        // Check index boundaries:
        assert(0 <= index);
        if (index >= s->next)
            return 0; // To indicate the end
        // Produce pointer to desired element:
        return &(s->storage[index * s->size]);
    }
    
    void cleanup(CStash *s)
    { // 清除，回收空间
        if (s->storage != NULL)
        {
            cout << "freeing storage" << endl;
            delete[] s->storage;
        }
    } ///:~
    void inflate(CStash *s, int increase)
    {
        //当插入发生溢出时，扩展空间
        assert(increase > 0);
        int newQuantity = s->quantity + increase;
        int newBytes = newQuantity * s->size;
        int oldBytes = s->quantity * s->size;
        unsigned char *b = new unsigned char[newBytes];
        for (int i = 0; i < oldBytes; i++)
            b[i] = s->storage[i]; // Copy old to new
        delete[](s->storage);     // Old storage
        s->storage = b;           // Point to new memory
        s->quantity = newQuantity;
    }
    ```

    <u>库的测试</u>`main.cpp`

    ```cpp
    //  创建两个Stash：
    // 一个用于存放int；一个用于存放char[80]
    
    #include "CLib.h" // 包含库的接口
    #include <fstream>
    #include <iostream>
    #include <string>
    #include <cassert>
    using namespace std;
    
    int main()
    {
        // Define variables at the beginning
        // of the block, as in C:
        CStash intStash, stringStash;
        int i;
        char *cp;
        ifstream in;
        string line;
        const int bufsize = 80;
    
        // Now remember to initialize the variables:
        initialize(&intStash, sizeof(int)); //显式初始化
        for (i = 0; i < 100; i++)
            add(&intStash, &i);
        for (i = 0; i < count(&intStash); i++)
            cout << "fetch(&intStash, " << i << ") = "
                 << *(int *)fetch(&intStash, i) << endl;
    
        // Holds 80-character strings:
        initialize(&stringStash, sizeof(char) * bufsize);
    
        in.open("CLibTest.cpp");
        assert(in);
        while (getline(in, line))
            add(&stringStash, line.c_str());
        i = 0;
        while ((cp = (char *)fetch(&stringStash, i++)) != 0)
            cout << "fetch(&stringStash, " << i << ") = "
                 << cp << endl;
    
        cleanup(&intStash);    //显式清除
        cleanup(&stringStash); //显式清除
    } ///:~
    ```

    

:grey_question: 思考：

假设你现在需要创建以下几个`stash`：

- 一个用于存放`double`的`stash`
- 一个用于存放`long`的`stash`
- 还有一个用于存放`objA`的`stash`

你需要在`main()`中添加大量代码！

### 2.1.3. C库的缺陷与问题

1. 用户必须显式初始化和清除，否则：
   - 内存误用
   - 内存泄漏
2. 所有涉及到`CStash`的文件中必须包含`CLib.h`，否则：
   - 编译器不认识`Struct Stash`
   - 编译器把未声明的函数调用猜测为一个其他的外部函数调用，往往会“巧合”。
3. 不同的库供应厂商由于未协商导致函数名冲突。

:grey_question: ​问题出在：不同结构内的同名变量不会冲突，而<u>同名函数会冲突</u>！

- 同名变量被“封装”在结构体内部，而函数没有“封装”。
- 为什么不把这一优点扩充到在特定结构体上运算的函数上呢？也就是说，让函数也作为`struct`的成员。

![image-20210709150606236](/assets/img/CPlusPlus/image-20210709150606236.png)

### 2.1.4. C++的结构

**C的`stash`->C++的`stash`**：将函数封装在结构中

- 库的接口`CppLib.h`

  ```cpp
  struct Stash
  {
      int size;     // Size of each space
      int quantity; // Number of storage spaces
      int next;     // Next empty space
                    // Dynamically allocated array of bytes:
      unsigned char *storage;
  
      // Functions!
      void initialize(int size);
      void cleanup();
      int add(const void *element);
      void *fetch(int index);
      int count();
      void inflate(int increase);
  }; ///:~
  ```

- 库的实现`CppLib.cpp`

  ```cpp
  // C library converted to C++
  // Declare structure and functions:
  #include "CppLib.h"
  #include <iostream>
  #include <cassert>
  using namespace std;
  
  // Quantity of elements to add when increasing storage:
  const int increment = 100;
  
  void Stash::initialize(int sz)
  { // 注意：函数有明确的从属关系；并且不需要显式地传递结构体变量地址（下同）
      size = sz;
      quantity = 0;
      storage = 0;
      next = 0;
  }
  int Stash::add(const void *element)
  {
      if (next >= quantity) // Enough space left?
          inflate(increment);
      // Copy element into storage,
      // starting at next empty space:
      int startBytes = next * size;
      unsigned char *e = (unsigned char *)element;
      for (int i = 0; i < size; i++)
          storage[startBytes + i] = e[i];
      next++;
      return (next - 1); // Index number
  }
  
  int Stash::count()
  {
      return next; // Number of elements in CStash
  }
  void *Stash::fetch(int index)
  {
      // Check index boundaries:
      assert(0 <= index);
      if (index >= next)
          return 0; // To indicate the end
      // Produce pointer to desired element:
      return &(storage[index * size]);
  }
  
  void Stash::cleanup()
  {
      if (storage != 0)
      {
          cout << "freeing storage" << endl;
          delete[] storage;
      }
  }
  
  void Stash::inflate(int increase)
  {
      assert(increase > 0);
      int newQuantity = quantity + increase;
      int newBytes = newQuantity * size;
      int oldBytes = quantity * size;
      unsigned char *b = new unsigned char[newBytes];
      for (int i = 0; i < oldBytes; i++)
          b[i] = storage[i]; // Copy old to new
      delete[] storage;      // Old storage
      storage = b;           // Point to new memory
      quantity = newQuantity;
  }
  ```

- 库的使用`main.cpp`

  ```cpp
  #include "CppLib.h"
  #include "../require.h"
  #include <fstream>
  #include <iostream>
  #include <string>
  using namespace std;
  
  int main()
  {
      Stash intStash;
      intStash.initialize(sizeof(int));
      for (int i = 0; i < 100; i++)
          intStash.add(&i);
      for (int j = 0; j < intStash.count(); j++)
          cout << "intStash.fetch(" << j << ") = "
               << *(int *)intStash.fetch(j) << endl;
  
      // Holds 80-character strings:
      Stash stringStash;
      const int bufsize = 80;
      stringStash.initialize(sizeof(char) * bufsize);
      ifstream in("CppLibTest.cpp");
      assure(in, "CppLibTest.cpp"); // assure函数在require.h中
      string line;
      while (getline(in, line))
          stringStash.add(line.c_str());
      int k = 0;
      char *cp;
      while ((cp = (char *)stringStash.fetch(k++)) != 0)
          cout << "stringStash.fetch(" << k << ") = "
               << cp << endl;
      intStash.cleanup();
      stringStash.cleanup();
  } ///:~
  ```

#### C++结构的特点

1. 函数成了结构的内部成员

   - 实现了封装，不能任意地增、删结构体内的库函数和数据成员

   ```cpp
   struct Stash
   {
       type data;	// 数据成员
       
       type function(type parameters);	// 成员函数
   }
   ```

2. 用作用域解析运算符”`::`”指示成员的从属关系

   - 这样函数有明确的归属

   ```cpp
   type Stash::function(type parameters)
   {
       //statemants
   }
   ```

3. 调用成员函数时，用成员选择符“`.`”指示从属关系

   - 由被动转变成主动

   ```cpp
   Stash S;
   S.function(80);
   ```

4. 对象创建时，只是分配数据成员的存储空间；成员函数的目标代码仍然只有一个拷贝。

   ```cpp
   Stash A,B,C ;   //有三个数据区
   A.function(80);
   B.function(10);
   C.function(4);  // 共享同一个函数体
   ```

**在`OOP`领域中，具有上述特征的结构体变量称为“`对象 object`”！**

### 2.1.5. OOP术语

#### 对象

- C中的结构(`struct`)将数据捆绑在一起，是数据的聚集 ；如果将专门作用在这些结构上的函数也捆绑到结构中，得到的结构就变成了（基本）对象。
- 对象既能描述属性(`attribute`)，又能描述行为(`behavior`)。是一个独立的捆绑的实体(`entity`)，有自己的记忆(`state`)和活动(`event`)。
- 在C++中，对象是”一块存储区”。 

![image-20210709152923995](/assets/img/CPlusPlus/image-20210709152923995.png)

#### 封装

将数据连同函数捆绑在一起的能力可以用于创建新的数据类型，通常称这种捆绑为“**封装**”。

```cpp
struct  CStack
{
	char * p;				// 属性
	char * v;				// 属性
	int size;				// 属性

	char pop();				// 操作    
	void push(char * c);	// 操作
	char top();				// 操作
    int size();				// 操作
 }

```

#### 抽象数据类型

通过“封装”可以创建一个新的类型；通常是对现实世界的物体的计算机描述，称为“**抽象数据类型**”。

![image-20210709153425460](/assets/img/CPlusPlus/image-20210709153425460.png)->

```cpp
struct truck {
	// 属性
	char brand[10];
	int speed;
	......;
	// 行为
	start();		// 启动
	stop();			// 熄火
	speed_up(); 	// 加速
	speed_down(); 	// 减速
	turn_left();   	// 左转向
}
```

#### 消息

对象“调用自己的一个成员函数”，如：`object.member_function(arglist)`，是通过对象的使用者完成的，又称之为：使用者“向一个对象发送**消息**”。当对象收到一个消息后，执行相应的操作。

```cpp
CTV haier;
// 向对象haier发送消息“turn_on”
haier.turn_on();
```

![image-20210709153733849](/assets/img/CPlusPlus/image-20210709153733849.png)

### 2.1.6. 关于对象的一些细节

1. 对象的大小==各数据成员大小的和

   ```cpp
   #include <iostream>
   using namespace std;
   struct A
   {
       int i[100];
       int j;
       void f();
   };
   int main()
   {
       cout << sizeof(A) << endl;
       cout << sizeof(int) * 101 << endl;
   }
   //sizeof(A)=sizeof(int)*100+sizeof(int)
   /*
    * output:
    * 404
    * 404
    */
   ```

2. :grey_question: 当struct不含数据成员时，对象大小==?

   ```cpp
   #include <iostream>
   using namespace std;
   struct B
   {
       void f();
   };
   
   void B::f() {}
   
   int main()
   {
       cout << "sizeof B = " << sizeof(B) << endl;
   } 
   // output: sizeof B = 1
   ```

   <u>当struct不含数据成员时，对象大小==1</u>

### 2.1.7. 关于头文件

[^]: 从软件工程的角度来考察

- 头文件中应只限于声明（接口）

  ​	若包含定义，由于多个文件均包含头文件，可能引起重复定义的问题

- 多次声明的问题：如何避免因多次包含头文件，造成重复声明结构的问题？

  `包含守卫 include guard`

  ```cpp
  #ifndef SIMPLE_H
  #define SIMPLE_H
  struct Simple {
  	int i,j,k;
  	initialize();
  };
  #endif  //包含守卫 include guard
  
  ```

- 头文件中的名字空间：不要在头文件中使用`using`指令（后面章节详解）

- 多个头文件的问题：将头文件、实现和`main()`函数放置在不同的文件中是一种好的习惯。

### 2.1.8. 嵌套的结构

嵌套：结构的数据成员也可以是一个结构。

```cpp
struct Stack {
    struct Link {
		void* data;
		Link* next;
		void initialize(void* dat, Link* nxt);
    }* head;
	void initialize();
	void push(void* dat);
	void* peek();
	void* pop();
	void cleanup();
};
```


## 2.2. 隐藏实现

1. 例子

   ![image-20210709203001775](/assets/img/CPlusPlus/image-20210709203001775.png)

   内部电路（实现）有不同，但操作界面(接口)总是相同的。

2. 必要性：

   - 让库的使用者远离一些他们不需要知道的内部实现
   - 允许库的设计者改变内部实现，而不必担心对客户程序员造成什么影响

3. 原则：

   - 将类的功能描述部分作为共有部分接口提供给用户
   - 将数据的内部表示、功能的内部实现作为私有部分隐藏起来

### 2.2.1. C++的访问控制

`public`/`private`/`protected`

- `public`：公有成员，其后声明的所有成员可以被所有的人访问
- `private`：私有成员，除了该类型的创建者和类的内部成员函数之外，任何人不能访问
- `protected`：保护成员，与`private`基本相同，区别是继承的结构可以访问`protected`成员，但不可以访问`private`成员

**设置访问控制**

```cpp
struct B {
private:
	char j;
	float f;
public:
	int i;
	void func();
}; 
void B::func() {
	B tempb;     
	tempb.j=‘a’;// OK
	i = 0;
	j = '0';
	f = 0.0;
};
int main() {
	B b;
	b.i = 1;  // OK, public
	// b.j = '1';  // illegal, 
	// b.f = 1.0;  // illegal,
	b.func(); // OK,public
} 
```

[^]: `protected`修饰符将在后续章节解释

**OOP的观点**

- 访问控制在库的设计者(实现者)和库的使用者之间划了一道明显的界线
  - `j`和`f`需要保护
  - `i`和`func()`可以公开
- 若试图访问一个私有成员，就会产生一个<u>编译时错误 `Compile-time Error`</u>

### 2.2.2. 友元

**友元**：显示地声明哪些人“可以访问我的私有实现部分”，表明的是一种“信任”的关系。

```cpp
struct Cfather
{
    // 赋予CMother访问Cfather的salary的权利
    friend void CMother::inspect();
private:
    long salary;
    …;
};

struct CMother
{
private:
    void inspect()
    {
        Cfather obj_TaDie(8000);
        cout << "月收入 :" << obj_TaDie.salary;
    }
};
```

- 友元不是类的成员，只是一个声明
- 友元可以是外部全局函数、类或类的成员函数
- 友元是一个“特权”函数，破坏了封装。当类的实现发生变化后，友元也要跟着变动。

**OOP的观点**

- 友员不是OOP的特征，破坏了封装性，这也是C++不是“纯”面向对象语言的原因之一。

### 2.2.3. 从struct到class

- 封装：数据成员+成员函数->抽象数据类型
- 访问控制：接口与实现的分离

$$
\text{C的struct}+\text{封装}+\text{访问控制}=\text{类}
$$

:grey_question:：`CStash`和`CStack`中，哪些成员应该设置为私有的？哪些成员应该设置为公有的？

- 例1: 带访问控制的类`Stash`

  ```cpp
  class Stash
  {
  private:
      int size;     // Size of each space
      int quantity; // Number of storage spaces
      int next;     // Next empty space
      unsigned char *storage;
      void inflate(int increase);
  public:
      void initialize(int size);
      void cleanup();
      int add(void *element);
      void *fetch(int index);
      int count();
  };
  
  ```

- 例2:有访问控制的类`Stack`

  ```cpp
  class Stack
  {
  private:
      struct Link
      { // 私有的
          void *data;
          Link *next;
          void initialize(void *dat, Link *nxt);
      } * head;
  
  public:
      void initialize();
      void push(void *dat);
      void *peek();
      void *pop();
      void cleanup();
  };
  ```

#### class和struct的区别

- `class`的成员默认为`private`
- `struct`的成员默认为`public`

```cpp
struct X
{
    int i;    // 公有成员
    int y;    // 公有成员
    void f(); // 公有成员
};
class X
{
    int i;    // 私有成员
    int y;    // 私有成员
    void f(); // 私有成员
};
```

### 2.2.4. 访问控制的一个陷阱

返回`private`数据成员的非`const`引用：

​	当类的`public`成员函数`f`返回对该类`private`成员`m`的非`const`引用时，如果`f`的调用作为赋值语句的左值，则访问控制不起作用，对象的使用者可以直接存取`m`的值
​	称这种情形为访问控制的“陷阱”。

例如：

```cpp
#include <iostream>
using namespace std;
class X
{
private:
    int i;

public:
    int &badseti(int j)
    {
        i = (j > 0) ? j : (-j);
        return i; // 返回私有成员i的引用
    }
};

int main()
{
    X objx;
    cout << objx.badseti(10); // i的值为10；
    objx.badseti(-5) = -20;   // 此时i的值为-20；
}
```

---

```cpp
#include <iostream>
using namespace std;
class Time
{
public:
    Time(int = 0, int = 0, int = 0);
    void setTime(int, int, int);
    int getHour();
    int &badSetHour(int); // 危险的返回值
private:
    int hour;
    int minute;
    int second;
};

Time::Time(int hr, int min, int sec)
{
    setTime(hr, min, sec);
}

void Time::setTime(int h, int m, int s)
{
    hour = (h >= 0 && h < 24) ? h : 0;
    minute = (m >= 0 && m < 60) ? m : 0;
    second = (s >= 0 && s < 60) ? s : 0;
}

int Time::getHour() { return hour; }

// 极其危险的做法:
// Returning a reference to a private data member.
int &Time::badSetHour(int hh)
{
    hour = (hh >= 0 && hh < 24) ? hh : 0;
    return hour; // DANGEROUS reference return
}

int main()
{
    Time t;

    int &hourRef = t.badSetHour(20);
    cout << "Hour before modification: "
         << hourRef << endl;
    hourRef = 30; // modification with invalid value
    cout << "Hour after modification: "
         << t.getHour() << endl;

    // Dangerous: Function call that returns
    // a reference can be used as an lvalue!
    t.badSetHour(12) = 74;
    cout << "badSetHour as an lvalue, Hour: "
         << t.getHour() << endl;
    return 0;
}
/*
 * output:
 * Hour before modification: 20
 * Hour after modification: 30
 * badSetHour as an lvalue, Hour: 74
 */
```

**如何填补这个陷阱?**

<u>利用`const`!</u>

```cpp
#include <string.h>
#include <iostream>
using namespace std;
class Ctest
{
    char *str;

public:
    Ctest(int sz)
    {
        str = new char[sz];
        strcpy(str, "ustc is a famouse uni.");
    }
    ~Ctest() { delete[] str; }
    char const *getstr() const
    {
        return str;
    }
};
int main(int argc, char *argv[])
{
    Ctest t1(100);
    strcpy(t1.getstr(), "TEST!!"); // ERROR
    cout << t1.getstr();
    return 0;
}
```

编译时错误（`Compile-time Error:`）：

```
test.cpp:23:21: error: invalid conversion from 'const char*' to 'char*' [-fpermissive]
     strcpy(t1.getstr(), "TEST!!"); // ERROR
            ~~~~~~~~~^~
In file included from d:\VSCode_C\test.cpp:1:
```

### 2.2.5. 句柄类

:tired_face: 类的实现者的烦恼：

- 尽管客户程序员(类的使用者)不能访问私有实现部分，然而由于必须给他们提供`.h`文件，他们可以轻易地“看到”类是如何实现的。
  - 如何使得他们“看不见”实现？
- 如果改变了类的实现，即使没有改变类的接口，也可能需要重新编译所有使用了该类的客户程序。
  - 如何尽可能地避免不必要的重复编译？
- 解决办法：
  - 利用 “句柄类”。所谓句柄类，是一个特殊的类，该类中含有一个特殊的成员|>一个指向被隐藏的类的指针

---

- 接口：公开的部分 `Handle.h`

  ```cpp
  // Handle classes
  #ifndef HANDLE_H
  #define HANDLE_H
  
  class Handle
  {
      struct Cheshire; // 类cheshire的声明 ；
      Cheshire *smile; //  smile 指针指向具体的实现
  public:
      void initialize();
      void cleanup();
      int read();
      void change(int);
  };
  #endif // HANDLE_H 
  ```

  编译器从以上定义完全知道如何安排类`Handle`的存储，因此`Handle`无需知道`Cheshire`的具体实现，从而达到隐藏实现的目的。

- 实现：需要隐藏的部分 `Handle.cpp`

  ```cpp
  // Handle implementation
  #include "Handle.h"
  #include "../require.h"
  
  // 句柄类的实现: cheshire
  struct Handle::Cheshire
  {
      int i;
  };
  
  void Handle::initialize()
  {
      smile = new Cheshire;
      smile->i = 0;
  }
  void Handle::cleanup()
  {
      delete smile;
  }
  
  int Handle::read()
  {
      return smile->i;
  }
  
  void Handle::change(int x)
  {
      smile->i = x;
  }
  ```

**优点**

- 由于实现部分并没有暴露在`.h`中，保密性好
- 因为实现的细节均隐藏在句柄类的背后，因此当实现发生变化时，句柄类并没有改变，也就不必重新编译客户程序，只需重新编译隐藏的实现这一小部分代码，然后连接(link)就行了,减少了大量的重复编译。

## 2.3.初始化和清除

|>构造函数
|>析构函数
|>何时被调用
|>默认构造函数与重载构造函数

### 2.3.1. 安全性要求

正确地初始化和清除对象是保证程序安全性的关键

```cpp
int main()
{
    Stash intStash;
    intStash.initialize(sizeof(int)); //初始化

    for (int i = 0; i < 100; i++)
    {
        intStash.add(&i);
    }
    ...;
    intStash.cleanup(); // 清除
}
```

### 2.3.2. 构造函数

**构造函数：确保初始化**

- 类的特殊成员函数，编译器在创建对象时自动调用该函数。通常做一些初始化动作。以保证同一个类的对象具有一致性。
- 构造函数跟类同名，可以带参数，没有返回值。
  - 跟别的成员函数没有名字冲突
  - 编译器总能知道调用哪一个函数

**例子：带构造函数的`Stash`**

```cpp
class Stash
{
    int size; // Size of each space
    ...;

public:
    Stash(int size); // 构造函数，跟类同名,无返回值
    int add(void *element);
    ...;
    void cleanup();
};

Stash::Stash(int sz)
{
    size = sz;
    ...;
}
```

自动初始化`Stash`对象

```cpp
int main()
{
    //创建对象时自动调用构造函数
    Stash intStash1(2), intStash2(2);

    for (int i = 0; i < 100; i++)
    {
        intStash1.add(&i);
    }
    ...;
    intStash1.cleanup(); // 清除
}
```

**说明**

- 构造函数的名字必须与类的名字相同

- 构造函数不允许指明返回类型，也不允许返回一个值；

- 构造函数应声明为`public`（但不是必须），否则无法创建对象

  ```cpp
  class Stash
  {
  private:
      Stash(int sz){...;};
      ...;
  };
  
  void main()
  {
      Stash s(10); // Error! can‘t access private member
  }
  ```

- 可以重载构造函数

  ```cpp
  class Stash
  {
  public:
      Stash()
      {
          size = 1;
          ...;
      }
      Stash(int sz) {...;}
      ...;
  };
  
  void main()
  {
      Stash charStash;
      Stash intStash(2);
      Stash intStash(10, 2); // error,参数不匹配!
  }
  ```

### 2.3.3. 析构函数

**析构函数：确保清除**

- 析构函数：类的特殊成员函数，在撤销对象时自动调用该函数，通常做一些撤销对象前的回收工作
- 析构函数不带参数，没有返回值，不能够重载
- 析构函数必须是`public`函数

**例子：`String`类**

```cpp
#include <string.h>
class String
{
private:
    char *str;

public:
    String(char *s);         // 构造函数，跟类同名
    String(unsigned int sz); // 重载构造函数
    ...;
    ~String();               // 析构函数，类名前面加上一个~
};

String::~String()
{
    delete str;
}

String::String(unsigned int sz)
{ // 不能有返回值
    if (sz > 0)
        str = new char[sz];
    else
        str = new char[100];
}

String::String(char *s)
{ // 不能有返回值
    int len;
    len = strlen(s);
    if (len < 100)
        len = 100;
    str = new char[len];
    strcpy(str, s);
}

int main()
{
    String Paper_Tiger("U.S.A."); //这马老师太会了hhh
} //退出主程序前撤销对象，调用析构函数
```

### 2.3.4. 何时被执行？

- 构造函数：当对象被创建时，调用构造函数
- 析构函数：当对象被撤销时，调用析构函数

**例子**

```cpp
// 此例说明何时调用构造函数和析构函数

#include <iostream>
using namespace std;
// 类的定义
class Tree
{
    int height;

public:
    Tree(int initialHeight); // 构造函数
    ~Tree();                 // 析构函数
    void grow(int years);
    void printsize();
};
//类的实现

Tree::Tree(int initialHeight)
{
    height = initialHeight;
    printsize();
}
Tree::~Tree()
{
    cout << "inside Tree destructor" << endl;
}

void Tree::grow(int years)
{
    height += years;
}

void Tree::printsize()
{
    cout << "Tree height is " << height << endl;
}
int main()
{
    cout << "before opening brace" << endl;
    {
        Tree t(12); //生成 t
        cout << "after Tree creation" << endl;
        t.grow(4);
        cout << "before closing brace" << endl;
    } // 此时撤销t;
    cout << "after closing brace" << endl;
} ///:~

```

输出为

```
before opening brace
Tree height is 12
after Tree creation
before closing brace
inside Tree destructor ##析构函数
after closing brace
```

### 2.3.5. 完整的类

完整的类通常具有的特征：

- [x] 数据抽象：数据成员&成员函数
- [x] 实现隐藏：访问控制
- [x] 自动初始化和清除：构造函数和析构函数

**例子：类`Stash`**

- Stash.h

  ```cpp
  // With constructors & destructors
  #ifndef STASH_H
  #define STASH_H
  
  class Stash
  {
      int size;     // Size of each space
      int quantity; // Number of storage spaces
      int next;     // Next empty space
  
      // Dynamically allocated array of bytes:
      unsigned char *storage;
      void inflate(int increase);
  
  public:
      Stash(int size);
      ~Stash();
      int add(void *element);
      void *fetch(int index);
      int count();
  };
  #endif // STASH_H ///:~
  ```

- Stash.cpp

  ```cpp
  // Constructors & destructors
  #include "Stash2.h"
  #include "../require.h"
  #include <iostream>
  #include <cassert>
  using namespace std;
  const int increment = 100;
  Stash::Stash(int sz)
  { //构造函数
      size = sz;
      quantity = 0;
      storage = 0;
      next = 0;
  }
  
  int Stash::add(void *element)
  {
      if (next >= quantity)
          inflate(increment);
      // Copy element into storage,
      // starting at next empty space:
      int startBytes = next * size;
      unsigned char *e = (unsigned char *)element;
      for (int i = 0; i < size; i++)
          storage[startBytes + i] = e[i];
      next++;
      return (next - 1); // Index number
  }
  
  void *Stash::fetch(int index)
  {
      require(0 <= index, "Stash::fetch (-)index");
      if (index >= next)
          return 0; // To indicate the end
  
      // Produce pointer to desired element:
      return &(storage[index * size]);
  }
  
  int Stash::count()
  {
      return next; // Number of elements in CStash
  }
  
  void Stash::inflate(int increase)
  {
      require(increase > 0, "Stash::inflate zero or negative increase");
      int newQuantity = quantity + increase;
      int newBytes = newQuantity * size;
      int oldBytes = quantity * size;
      unsigned char *b = new unsigned char[newBytes];
      for (int i = 0; i < oldBytes; i++)
          b[i] = storage[i]; // Copy old to new
      delete[](storage);     // Old storage
      storage = b;           // Point to new memory
      quantity = newQuantity;
  }
  
  Stash::~Stash()
  {
      if (storage != 0)
      {
          cout << "freeing storage" << endl;
          delete[] storage;
      }
  } ///:~
  ```

- StashTest.cpp

  ```cpp
  #include "Stash2.h"
  #include "../require.h"
  #include <fstream>
  #include <iostream>
  #include <string>
  using namespace std;
  
  int main()
  {
      Stash intStash(sizeof(int));
      for (int i = 0; i < 100; i++)
          intStash.add(&i);
      for (int j = 0; j < intStash.count(); j++)
          cout << "intStash.fetch(" << j << ") = "
               << *(int *)intStash.fetch(j) << endl;
      const int bufsize = 80;
      Stash stringStash(sizeof(char) * bufsize);
      ifstream in("StashTest.cpp");
      assure(in, "StashTest.cpp"); // 同 2.1.4.main.cpp
      string line;
      while (getline(in, line))
          stringStash.add((char *)line.c_str());
      int k = 0;
      char *cp;
      while ((cp = (char *)stringStash.fetch(k++)) != 0)
          cout << "stringStash.fetch(" << k << ") = "
               << cp << endl;
  } ///:~
  
  ```

### 2.3.6. 默认的构造函数

- 如果没有提供任何构造函数，编译器会创建一个默认的构造函数，但这个<u>函数所实现的功能很少是我们所期望的</u>。因此，应明确定义默认构造函数。

  ```cpp
  class Y{
  	...;
  }
  Y objy; // OK, Why?
  ```

  

- 如果定义了构造函数，但没有无参构造函数，则创建对象时，若不带参数，将会出错。

    ```cpp
    class Y{
        Y(int sz){…}
        ...;
    }
    Y objy; // error,参数类型不匹配
    ```

### 2.3.7. 显式调用析构函数

如果只想执行析构函数中的执行的操作，而不释放对象的空间，则可以显式调用析构函数。

```cpp
//执行系统的析构函数，不释放对象的空间。 
pb->~String();
```

---


# 3. 类成员

## 3.1. 成员变量与成员函数

### 3.1.1. 成员变量

- **成员变量**：可以是基本类型、构造类型，甚至是一个另一个类的对象、对象指针或引用

  ```cpp
  class X
  {
  private:
      int basetypemem;  // 基本类型
      char str[20];     // 构造类型
      Cstack obj_stack; // 另一个类的对象,stack必须在X之前定义
      CTime *obj_time;  // 另一个类的对象指针
      CTime &obj_time;  // 另一个类的对象引用
      ...;
  }
  ```

- **提前声明**：一个类提前声明之后，这个类的对象指针、引用可以作为另一个类的成员变量。但这个类的对象不能作另一个类的成员变量。

  ```cpp
  class Stack;     
  class StackofStack{ 
  	int topStack;
  	Stack *stackstorage;
  };
  ```

### 3.1.2. 成员函数

- 在类的定义体外定义：

  ```cpp
  class Stack
  {
      int num_elements;
  
      int empty();
      void pop();
      int top();
      void push(int);
  };
  
  int Stack::empty()
  {
      return (num_elements == 0);
  }
  ```

- 在定义体内定义：

  ```cpp
  class Stack
  {
      int num_elements;
  
      int empty() { return (num_elements == 0); }
      // 在类的定义体内定义时，自动成为内联函数。
      void pop();
      int top();
      void push(int);
  };
  ```

### 3.1.3. 对象与类

- 创建对象

  `类 对象1,对象2,...,对象n;`

  ```cpp
  Stack int_stack(10), string_stack;
  ```

- 存储分配

  一个类所有的对象的成员变量(除了静态变量)都有独立空间，但同一个类的所有对象成员*共享*同一组成员函数。

## 3.2. this指针

:grey_question: 当类的多个实例同时存在时，在一个类的成员函数内部引用一个数据成员，并没有指明一个类的具体实例。编译器如何决定引用哪个对象的数据成员呢？

```cpp
class CTest
{
private:
    int n;

public:
    int getn() { return n; }
    //对哪个对象的n进行操作呢？
};
```

### 3.2.1. 隐藏的this指针

:grey_exclamation: 解决办法：编译器给成员函数传递一个隐藏的对象指针参数，该指针总是指向当前要引用的对象。称为“`this指针`”

实现方法：类`Ctest`中的每个成员函数都隐式地声明`this`指针

```cpp
class CTest
{
private:
    int n;

public:
    int getn();
};
int CTest::getn()
 
{
    return n;
}
/* 
 * 上面的getn()函数等价于
 * int CTest::getn(const CTest *const this)
 * {
 *     return this->n;
 * }
 */
```

---

例子：

```cpp
#include <iostream>
using namespace std;

class Test
{
public:
    Test(int = 0); // default constructor
    void print() const;

private:
    int x;
};

Test::Test(int a) { x = a; } // constructor
void Test::print() const
{
    cout << "        x = " << x << endl
         << "  this->x = " << this->x << endl
         << "(*this).x = " << (*this).x << endl;
}
int main()
{
    Test testObject(12);
    testObject.print();
}

/*
 * output:
 *         x = 12
 *   this->x = 12
 * (*this).x = 12
 */
```

### 3.2.2. this是一个常量指针

|>`const X *this;`：不能修改this指针的值

```cpp
int Ctest::setn()
{
    Ctest t1;
    this = new t1; //非法操作，不能改变this指针
}
```

### 3.2.3. this指针的用途

#### 3.2.3.1. 实现成员函数的链式调用

通过返回`*this`的引用，可以实现成员函数的链式调用

```cpp
class X
{
    X &assign()
    {
        …;
        return (*this);
    }
    X &setvalue()
    {
        …;
        return (*this);
    }
    …;
};

X objX;
objX.assign().setvalue().assign(); // .从左向右结合
```

**例子**

- `time.h`

  ```cpp
  class Time
  {
  public:
      Time(int = 0, int = 0, int = 0); // default constructor
      Time &setTime(int, int, int);    // set hour, minute, second
      Time &setHour(int);              // set hour
      Time &setMinute(int);            // set minute
      Time &setSecond(int);            // set second
      
      int getHour() const;             // return hour
      int getMinute() const;           // return minute
      int getSecond() const;           // return second
      void printMilitary() const;      // print military time
      void printStandard() const;      // print standard time
  private:
      int hour;   // 0 - 23
      int minute; // 0 - 59
      int second; // 0 - 59
  };
  ```

- `time.cpp`

  ```cpp
  #include <iostream>
  #include "time.h"
  using namespace std;
  
  Time::Time(int hr, int min, int sec)
  {
      setTime(hr, min, sec);
  }
  
  Time Time::setTime(int h, int m, int s)
  {
      setHour(h);
      setMinute(m);
      setSecond(s);
      return *this; // enables cascading
  }
  
  Time Time::setHour(int h)
  {
      hour = (h >= 0 && h < 24) ? h : 0;
      return *this; // enables cascading
  }
  
  Time Time::setMinute(int m)
  {
      minute = (m >= 0 && m < 60) ? m : 0;
      return *this; // enables cascading
  }
  Time &Time::setSecond(int s)
  {
      second = (s >= 0 && s < 60) ? s : 0;
      return *this; // enables cascading
  }
  
  int Time::getHour() const { return hour; }
  int Time::getMinute() const { return minute; }
  int Time::getSecond() const { return second; }
  
  void Time::printMilitary() const
  {
      cout << (hour < 10 ? "0" : "") << hour << ":"
           << (minute < 10 ? "0" : "") << minute;
  }
  void Time::printStandard() const
  {
      cout << ((hour == 0 || hour == 12) ? 12 : hour % 12)
           << ":" << (minute < 10 ? "0" : "") << minute
           << ":" << (second < 10 ? "0" : "") << second
           << (hour < 12 ? " AM" : " PM") << endl;
  }
  
  ```

- `main.cpp`

  ```cpp
  #include <iostream>
  #include "time.h"
  using namespace std;
  int main()
  {
      Time t;
      t.setHour(18).setMinute(30).setSecond(22);
      cout << "Military time: " << endl;
      t.printMilitary();
      cout << "Standard time: " << endl;
      t.printStandard();
      cout << endl;
      cout << "\nNew standard time: " << endl;
      t.setTime(20, 20, 20).printStandard();
      return 0;
  }
  
  ```

- 运行结果为

  ```
  Military time: 18:30
  Standard time: 6:30:22 PM
  
  New standard time: 8:20:20 PM
  ```

:grey_question: 假如按值返回`*this`，问程序的输出是什么？

```
Military time: 18:00
Standard time: 6:00:00 PM    

New standard time: 8:20:20 PM
```

#### 6.2.3.1. 防止自身赋值

```cpp
class String
{
private:
    char *str;

public:
    String &assign(const String &s)
    {
        int tmplen;
        if (this == &s)
        {
            error("自身赋值");
            return (*this);
        }
        else
        {
            tmplen = strlen(s.str);
            delete (str)
                str = new char[tmplen + 1];
            strcpy(str, s.str);
        }
    }
    ...; //
};

String s1("How are you?"), s2("Fine");
s1.assign(s2);
s2.assign(s2);
```

## 3.3. 成员变量

### 3.3.1. 成员对象（对象成员）

类的数据成员可以是另一个类的对象，这种数据成员称为“成员对象”

```cpp
class Employee
{
private:
    char firstName[25];
    char lastName[25];
    const Date birthdate;
    const Date hireDate;
    ...; // 以下省略
};
```

### 3.3.2. 初始化次序

**构造函数的执行次序：**

- 对象成员的构造函数先初始化，然后才是包含它的类的构造函数。有多个对象成员时，按照在类的定义中声明的次序初始化。

如上`Employee`类，先执行`birthDate.Date(…),hireDate.Date(…)`，再执行`e.Employee(...)`

### 3.3.3. 成员初始化表

如何初始化？成员初始化表

`类的构造函数名(类构造函数参数列表+成员构造函数参数列表):成员对象1(参数列表),成员对象2(参数列表)`

```cpp
Employee::Employee(char *fname, char *lname,
                   int bmonth, int bday, int byear,
                   int hmonth, int hday, int hyear)
    : birthDate(bmonth, bday, byear),
      hireDate(hmonth, hday, hyear)
{
}
```

**例子:成员对象(对象成员)的初始化**

```cpp
#include "date1.h"
#include <iostream>
using namespace std;
class Employee
{
public:
    Employee(char *, char *, int, int, int, int, int, int);
    void print() const;
    ~Employee(); // provided to confirm destruction order
private:
    char firstName[25];
    char lastName[25];
    const Date birthDate;
    const Date hireDate;
};
Employee::Employee(char *fname, char *lname,
                   int bmonth, int bday, int byear,
                   int hmonth, int hday, int hyear)
    : birthDate(bmonth, bday, byear),
      hireDate(hmonth, hday, hyear)
{
    ...; // 构造函数体；
}

void Employee::print() const
{
    ...; // 略
}

// Destructor: provided to confirm destruction order
Employee::~Employee()
{
    cout << "Employee object destructor: "
         << lastName << ", " << firstName << endl;
}
int main()
{
    Employee e("Bob", "Jones", 7, 24, 1949, 3, 12, 1988);
    return 0;
}
```

输出

```cpp
Date object constructor for date 7/24/1949
Date object constructor for date 3/12/1988
Employee object constructor Bob Jones
….
Employee object destructor Bob Jones
Date object destructor for date 3/12/1988
Date object destructor for date 7/24/1949
```

<u>由内向外建立，由外向内删除</u>

**注意**

- 初始化表只能出现在构造函数的定义体中，不能出现在构造函数的声明中。（MS VC++没有这个限制。）
- 若不提供成员初始化值，则编译器隐式调用成员对象的默认构造函数。
- `const`成员必须在成员初始化表中初始化

### 3.3.4. 析构次序

**析构函数的执行次序：**

- 先执行对象的析构函数，然后才执行对象成员的析构函数。有多个对象成员时，按照在类的定义中声明的次序相反的次序析构对象成员。

如上`Employee`类，依次删除：

1. `Employee`对象
2. `hireDate`对象
3. `birthDate`对象

## 3.4. const量（常量）

### 3.4.1. const的意义

- **最低权限原则**：软件工程的基本原则之一。

- **`const`的意义**：在可更改与不可更改之间画一条明确的界线，提高程序的安全性和可控性。

  ```cpp
  const int i = 100;
  i++;// 编译错误
  ```

  

### 3.4.2. C中的const（常量）

“一个不能被改变的普通变量”

- 总是占用存储
- 名字是全局的。也就是说，默认情况下，`const`是外部连接的(容易引起“名字冲突”)

```c
const int bufsize; // 无需初始化

const int bufsize = 100;
char buf[bufsize]; // error!! Why??
```

编译时报错：

```
test.c:4:17: error: size of array 'buf' is not an integral constant-expression
 char buf[bufsize]; // error!! Why??
```

<u>在编译时，编译器并不知道`const`的值，它只是一个“运行时常量”。</u>

### 3.4.3. C++的const

- 通常，C++编译器不为`const`创建存储空间，而是把它保存在“符号表”里，即“<u>编译时常量</u>”。

  ```cpp
  const int bufsize1; // 非法，未赋初值
  const int bufsize = 100;
  char strbuf[bufsize]; // OK, Why?
  ```

- 默认情况下，C++中的`const`是内部连接的，也就是说，`const`仅在`const`被定义过的文件里才是可见的。因此，不用担心名字冲突

- 当定义一个`const`时，必须赋一个值给它，除非用`extern`做出了清楚的说明。当用`extern`说明了`const`时，编译器会强制为`const`分配空间，而不是保存在符号表中。

  ```cpp
  extern const int bufsize; 
  // 未赋初值，但extern声明了bufsize
  // 在另一个文件中定义及赋初值。
  ```

- `const`用于集合，必须为其分配内存，因为编译器“不愿意”把集合保存到符号表中，太复杂。

  ```cpp
  const int i[] = {1, 2, 3, 4};
  float f[i[3]]; // 非法，编译期间无法知道存储空间的值。
  
  struct S
  {
      int i, j;
  };
  const S s[] = { {1, 2}, {3, 4}};
  double d[s[1].j]; // 非法，理由同上
  
  int main()
  {
  } ///:~
  ```

### 3.4.4. C++中const的作用

1. **值替代**：C++的`const`​ :vs: ​C的宏替换

   ```cpp
   // C++的const
   const int bufsize = 100;
   char str[bufsize]; 
   
   // 宏替换
   #define BUFSIZE 100; // 宏替换
   char str[BUFSIZE];
   ```

   - :tired_face: 在宏替换中，BUFSIZE没有类型信息，不能进行类型检查；
   - :tired_face: 宏定义是全局的，容易名字冲突。

2. 安全性

   如果<u>想用运行期间产生的值初始化一个变量</u>，并且知道在该变量的生命期内其值不变，则可用`const`限定该变量，达到最大限度地保证改变量安全性的目的。

   ```cpp
   #include <iostream>
   using namespace std;
   int main()
   {
       cout << "type a character & CR:";
       const char c = cin.get(); //用运行期间产生的值初始化，之后不变
       const char c2 = c + 'a';
       cout << c2;
   } ///:~
   ```

### 3.4.5. const的应用：const指针

1. **指向`const`的指针**

   - 可以指向常变量，也可以指向未被声明为`const`的变量
   - 此时只能修改指向地址，不能修改值，可以修改指向的那个变量的值，不能用本身对指针变量地址修改的方式来修改值
   - 如果一个变量已经被声明为常变量，不能用它进行初始化别的变量，而且只能用常变量的指针去指向它，而不能用一般的非const类型指针变量去指向它。

   ```cpp
   const int *p1;
   int const *p2;
   // p1, p2都是指向const的指针
   ```

2. **`const`指针**

   - 指向变量的常指针
   - 只能修改值，不能修改指向地址
   - C++中，`const`指针必须赋初值

   ```cpp
   int i = 1;
   int *const p = &i;
   ```

3. **`const`指针指向`const`对象**

   - 不能再指向别的常量
   - 指向的值也不能修改

   ```cpp
   int i = 1;
   const int *const p1 = &i;
   int const *const p2 = &i;
   i++; // OK
   // p1, p2, *p1, *p2都不能修改
   ```

**:warning: 注意**

- 非`const`对象的地址可以赋给`const`指针

- `const`对象的地址绝不可以赋给非`const`指针,因为这样做可能导致<u>通过非`const`指针改变`const`对象的值的后果</u>

  ```cpp
  int d = 1;
  const int e = 2;
  int *u = &d; 		// OK, d not const
  //! int* v = &e;    // illegal, e const
  int *w = (int *)&e; // legal but bad practice
  ```

### 3.4.6. const的应用：const参数，const返回值

1. 传递`const`值

   ```cpp
   void fun(const int i)
   {
       i++; // 编译时错误，i不能改变
   }
   ```

   :grey_question: <u>“形参”不能被改变</u> or “实参”不能被改变？

2. 按值返回内部`const`常量

   ```cpp
   int f3() { return 1; }
   const int f4() { return 1; } // 返回const int
   
   int main()
   {
       const int j = f3(); // Works fine
       int k = f4();       // But this works fine too!
   } ///:~
   ```

   <u>对内部类型来说，按值返回`const`量并没有什么特别的意义。</u> 

3. 按值返回自定义类型的`const`

   <u>实际上阻止了返回值作为左值出现。</u> 

   ```cpp
   class X
   {
       int i;
   
   public:
       X(int ii = 0);
       void modify();
   };
   
   X::X(int ii) { i = ii; }
   void X::modify() { i++; }
   X f5()
   {
       X x(2);
       return x; // 返回变量
   }
   
   const X f6()
   {
       return X(); // 按值返回const;
   }
   
   void f7(X &x)
   { //  按值传递非const引用
       x.modify();
   }
   
   int main()
   {
       f5() = X(1);   // 正确，f5()返回非const量；
       f5().modify(); // 正确
   
       f7(f5()); //可能会有Warning,跟编译选项有关
   
       f6() = X(1);   // Error: f6()是常量，不能作左值
       f6().modify(); // Error: f6()是常量，不能被修改
       f7(f6());      // Error: Why??
       			   // 将"X &"类型的引用绑定到"const X"
       			   // 类型的初始值设定项时，限定符被丢弃
   } ///:~
   
   ```

4. 传递和返回`const`指针

   ```cpp
   char *strcpy(char *dest, const char *src);
   
   void t(int *)
   {
   }
   void u(const int *cip)
   {
       *cip = 2;       // Error:  试图改变值
       int i = *cip;   // OK -- copies value
       int *ip2 = cip; // Error: 试图让非const *指向const *
   }
   const int *const w()
   {
       static int i;
       return &i; // 返回静态局部量的地址
   }
   int main()
   {
       int x = 0;
       int *ip = &x;
       const int *cip = &x;
       t(ip);  // OK
       t(cip); // Not OK
       u(ip);  // OK
       u(cip); // Also OK
   
       int *ip2 = w();              // Not OK
       const int *const ccip = w(); // OK
       const int *cip2 = w();       // OK
       *w() = 1;                    // Not OK
   } ///:~
   ```

   <u>当传递一个或返回一个地址时(指针或引用)，设置为`const`可以阻止客户程序员修改其值。</u>

   对参数传递而言，C++建议用const引用传递替代值传递

   - 兼顾了**效率**和**易用性** 
     - 传递地址比传递整个对象更有效
     - 引用传递比指针传递形式上更简单

## 3.5. const对象与const成员函数

讨论const在类中的应用

|>const数据成员

|>const成员函数

|>const对象

### 3.5.1. const数据成员

```cpp
class Fred
{
    const int size; // const数据成员

public:
    Fred(int sz);
    void print();
};

Fred::Fred(int sz) : size(sz) {} // 构造函数初始化列表：
								 // 常量数据成员必须被初始化
void Fred::print() { cout << size << endl; }

int main()
{
    Fred a(1), b(2), c(3);
    // 1. 每个对象的const成员经初始化后都不能改变。
	// 2. 这些const成员相互独立，可以有不同的值。

    a.print(), b.print(), c.print();
} ///:~
```

- 构造函数初始化列表：常量数据成员必须被初始化
- 每个对象的`const`成员经初始化后都不能改变。
- 这些`const`成员相互独立，可以有不同的值。

#### `static const`：静态常量类成员，`enum`枚举常量

- `const`数据成员实际上是一个运行期间常量

- 如何获得编译期间整个类的恒定常量？

  1. `static const `静态常量(见后续章节)

  2. `enum `枚举常量

     ```cpp
     class A
     {
         ...;
         enum { SIZE1 = 100,
                SIZE2 = 200 }; // 枚举常量
         int array1[SIZE1];
         int array2[SIZE2];
     };
     ```

     :warning: 枚举常量并不是类成员，也不会占用对象的存储空间，它们在编译时被全部求值

### 3.5.2. const对象

**`const`对象**：对象被初始化后，它的数据成员在其生命期内不被改变

```cpp
const int i = 1; //const int
const blob b(2); //const对象

b.modify();
```

#### 如何保证const对象不被改变？

- 公有数据：只要用户不去改变，这些数据保持不变是很容易实现的
- 问题是：用户在调用成员函数时，也必须保证不改变数据
  - 对象如何知道哪些成员函数将会改变数据？
  - 对象如何知道哪些成员函数对于`const`对象来说是安全的
- 解决方法：强制声明和定义<u>`const`成员函数</u>，显式地告诉编译器这些函数对数据是安全的，可以被`const`对象调用。

### 3.5.3. const成员函数

在成员函数的声明和定义后面加上`const`使之成为`const`成员函数。

```cpp
class X
{
    int i;

public:
    X(int ii);
    int const_f() const;
    int f();
};

X::X(int ii) : i(ii) {}
int X::const_f() const { return i; }
int X::f() { ...; }
int main()
{
    X x1(10);
    const X x2(20);
    x1.f();       // OK
    x2.const_f(); // OK
    x2.f();       // Error
}
```

`const_f()`是`const`函数，保证不修改`x2`;

### 3.5.4. const对象与const成员函数

- 声明为`const`的对象是不能被赋值的

- 声明为`const`的对象不能随便调用任意的成员函数

  ```cpp
  x2.f();   // error， f()非const成员函数
  ```

- 声明为`const`的对象只能调用声明为`const`的成员函数

  ```cpp
  x2.const_f() ; //OK
  ```

- `const`的成员函数不能改变成员变量

**例子**：`const`成员函数与非`const`成员函数

```cpp
#include <iostream>
#include <stdlib.h>
#include <time.h>
using namespace std;
class Quoter
{
    int lastquote;

public:
    Quoter();
    int lastQuote() const;
    const char *quote();
};

Quoter::Quoter()
{
    lastquote = -1;
    srand(time(0)); // Seed random number generator
}
int Quoter::lastQuote() const
{
    return lastquote;
}
const char *Quoter::quote()
{
    static const char *quotes[] = {
        "Are we having fun yet?",
        "Doctors always know best",
        "Is it ... Atomic?",
        "Fear is obscene",
        "There is no scientific evidence "
        "to support the idea "
        "that life is serious",
        "Things that make us happy, make us wise",
    };
    const int qsize = sizeof(quotes) / sizeof(*quotes);
    int qnum = rand() % qsize;
    while (lastquote >= 0 && qnum == lastquote)
        qnum = rand() % qsize;
    return quotes[lastquote = qnum];
}
int main()
{
    Quoter q;
    const Quoter cq;
    cq.lastQuote(); // OK
    //!  cq.quote(); // Not OK; non const function
    for (int i = 0; i < 20; i++)
        cout << q.quote() << endl;
} ///:~
```

输出为

```
Is it ... Atomic?
Doctors always know best
Are we having fun yet?
There is no scientific evidence to support the idea that life is serious
Doctors always know best
Are we having fun yet?
Things that make us happy, make us wise
Are we having fun yet?
Fear is obscene
Is it ... Atomic?
There is no scientific evidence to support the idea that life is serious
Doctors always know best
Is it ... Atomic?
Things that make us happy, make us wise
Are we having fun yet?
Things that make us happy, make us wise
Is it ... Atomic?
Fear is obscene
Things that make us happy, make us wise
Are we having fun yet?
```

### 小结

- `const`能将对象、函数参数、返回值和成员函数定义为常量，还可以进行值替代。
- `const`为程序设计提供了又一种非常好的类型检查形式及安全性。
- `const`几乎成了程序正确性的“救命稻草”。

## 3.6. 静态成员变量与静态成员函数

[^]: 这部分好像不考，暑假再来整理:joy:

---


# 4. 引用和拷贝构造函数

## 4.1. C++的指针和引用

### 4.1.1. C++中的指针

与C的区别：类型要求更强

```cpp
bird *b;
rock *r;
void *v;
b = r; // error in C and C++
v = r;
b = v; // OK in C ,but not in C++
```

### 4.1.2. C++中的引用

```cpp
#include <iostream>
using namespace std;

int y;
int &r = y;        // 引用被创建时，必须联系到某个存储单元
const int &q = 12; // (1) 编译器分配一个存储单元，
                   //     然后q跟该单元相联系

int x = 0;  // (2)
int &a = x; // (3)

int main()
{
    cout << "x = " << x << ", a = " << a << endl;
    a++;
    cout << "x = " << x << ", a = " << a << endl;
} ///:~
```

**^*^使用引用的规则**

1. 当引用被创建时，它必须被初始化
2. 一个引用被初始化为“引用”某个对象，它就不能改变为另一个对象的引用
3. 不能有“空”引用，必须保证引用是和一块合法的存储单元相联系

引用：就像是能<u>自动</u>被编译器<u>间接引用</u>的`const`指针

### 4.1.3. 函数中的引用

1. 引用用作函数参数

   函数中任何对引用的更改将改变被引用的实参。

2. 引用用作返回值

   必须保证引用关联的内存是合法的。

**例子**：引用作参数和返回值

```cpp
int *f(int *x)
{
    (*x)++;
    return x; // Safe, x的作用范围在函数之外
}
int &g(int &x)
{
    x++;      // Same effect as in f()
    return x; // Safe, outside this scope
}
int &h()
{
    int q;
    //!  return q;  // Error
    static int x;
    return x; // Safe, x lives outside this scope
}
int main()
{
    int a = 0;
    f(&a); // Ugly (but explicit)
    g(a);  // Clean (but hidden)
} ///:~
```

问题：如何保护实参不被非法修改？

使用常量引用作为参数。

```cpp
void f(X &) { ...; }
void g(const X &) { ...; }

int main()
{
    X varxobj(1);
    const X cnstobj(10);
    //!  f(cnstobj); // Error
    g(cnstobj);
} ///:~

```

## 4.2. 拷贝构造函数

形如`X(X&)`的构造函数

### 4.2.1. ^*^活动记录（Active record）

### 4.2.2. 如何实现按值传递和返回值？

1. 传递和返回基本类型数据

   下面的代码

   ```cpp
   int f(int x, char c);
   int g=f(a,b);         
   ```

   编译为汇编后

   ```assembly
   push b
   push a		# 把参数直接压栈
   call f()
   add sp, 4
   mov g 
   register AX # 从寄存器中获得返回值
   ```

2. 传递和返回大对象

   ```cpp
   struct Big
   {
       char buf[50];
       int i;
       long d;
   } B, B2;
   
   Big bigfun(Big b)
   {
       b.i = 100; // Do something to the argument
       return b;
   }
   
   int main()
   {
       B2 = bigfun(B);
   } ///:~
   
   ```

   编译为

   ```assembly
   _main	proc	near
   	push	bp
   	mov	bp,sp
   	mov	ax,offset DGROUP:_B
   	mov	dx,ds
   	mov	cx,56
   	call near ptr N_SPUSH@ # N_SPUSH@是一个把B的地址压栈的辅助函数
   	push ds
   	push offset DGROUP:_B2 # B2的地址作为返回值也被压栈
   	call near ptr @bigfun$q3Big
   	add	sp,60
   	pop	bp
   	ret	
   _main	endp
   ```

#### 位拷贝

当按值传递(或返回)一个自定义对象时，编译器先把需要传递（或返回）的对象的地址压栈，然后由一个专门的辅助函数把值拷贝到目的地。拷贝的方式是按位一一对应拷贝，称为“**位拷贝**”

<img src="/assets/img/CPlusPlus/image-20210710173439131.png" alt="image-20210710173439131" style="zoom: 50%;" />

### 4.2.3. 对象的初始化

当用一个对象去生成（初始化）另一个对象时，同样地，编译器并不是调用普通的构造函数生成该对象，而是以位拷贝的方式生成该对象。

```cpp
X x1(0);
X x2 = x1; // 初始化, 等价于 X x2(x1);
           // 按位拷贝生成X2.
```

:grey_question: 位拷贝有什么问题？

尽管对象由一个内存区的比特位构成，但对象具有含义，因此对象远比一组比特位要复杂得多。而对象的这些含义往往不能由简单的位拷贝来很好地反映。

**例子**：位拷贝的缺陷

```cpp
#include <string>
#include <iostream>
using namespace std;
class HowMany
{
    static int objectCount;

public:
    HowMany() { objectCount++; }
    static void print(const string &msg = "")
    {
        if (msg.size() != 0)
            cout << msg << ": ";
        cout << "objectCount = " << objectCount
             << endl;
    }
    ~HowMany()
    {
        objectCount--;
        print("~HowMany()");
    }
};

int HowMany::objectCount = 0;

// 按值传递和返回
HowMany f(HowMany x)
{
    x.print("x argument inside f()");
    return x;
}

int main()
{
    HowMany h;
    HowMany::print("after construction of h");
    HowMany h2 = h;
    HowMany::print("after h2=h");

} ///:~
```

输出为

```
after construction of h: objectCount = 1
after h2=h: objectCount = 1
~HowMany(): objectCount = 0
~HowMany(): objectCount = -1
```

### 4.2.4. 问题&解决

- 由于使用简单的位拷贝初始化对象，没能正确地反映对象的含义，没有改变`objectCount`值
- 如何解决？
  - 定义自己的拷贝方法，而不是使用系统默认的位拷贝的方式。这个方法称为类的“拷贝构造函数”。当从一个对象拷贝生成一个新对象时该函数被自动执行。

### 4.2.5. 拷贝构造函数

因为拷贝生成对象的方式为：
`Howmany h2(h);  h2=h;`
所以拷贝构造函数的形式应该为：`X(X&)；`

例如：

```cpp
Howmany::Howmany(const Howmany &h)
{
    ...;
}
```

:warning: 注意：

1. 参数必须是类`howmany`的`const`引用(`const howmany &`),否则会引起无穷递归!
2. 没有返回值

**例子** howmany2

```cpp
#include <string>
#include <iostream>
using namespace std;

class HowMany2
{
    string name; // Object identifier
    static int objectCount;

public:
    HowMany2(const string &id = "") : name(id)
    {
        ++objectCount;
        print("HowMany2()");
    }
    ~HowMany2()
    {
        --objectCount;
        print("~HowMany2()");
    }
    // The copy-constructor:
    HowMany2(const HowMany2 &h) : name(h.name)
    {
        name += " copy";
        ++objectCount;
        print("HowMany2(const HowMany2&)");
    }

    void print(const string &msg = "") const
    {
        if (msg.size() != 0)
            cout << msg << endl;
        cout << '\t' << name << ": "
             << "objectCount = "
             << objectCount << endl;
    }
};

int HowMany2::objectCount = 0;
// Pass and return BY VALUE:
HowMany2 f(HowMany2 x)
{
    x.print("x argument inside f()");
    cout << "Returning from f()" << endl;
    return x;
}

int main()
{
    HowMany2 h("h");
    cout << "Entering f()" << endl;
    HowMany2 h2 = f(h);
    HowMany2 *p1;

    h2.print("h2 after call to f()");
    cout << "Call f(), no return value" << endl;
    f(h);
    cout << "After call to f()" << endl;
} ///:~
```

输出为

```
HowMany2()
        h: objectCount = 1
Entering f()
HowMany2(const HowMany2&)
        h copy: objectCount = 2
x argument inside f()
        h copy: objectCount = 2
Returning from f()
HowMany2(const HowMany2&)
        h copy copy: objectCount = 3
~HowMany2()
        h copy: objectCount = 2
h2 after call to f()
        h copy copy: objectCount = 2
Call f(), no return value
HowMany2(const HowMany2&)
        h copy: objectCount = 3
x argument inside f()
        h copy: objectCount = 3
Returning from f()
HowMany2(const HowMany2&)
        h copy copy: objectCount = 4
~HowMany2()
        h copy copy: objectCount = 3
~HowMany2()
        h copy: objectCount = 2
After call to f()
~HowMany2()
        h copy copy: objectCount = 1
~HowMany2()
        h: objectCount = 0
```

### 4.2.6. 默认拷贝构造函数

- 如果没有自定义拷贝构造函数，编译器生成一个默认的拷贝构造函数（位拷贝）。

- 如何替代拷贝构造函数?

  1. 防止按值传递: 声明一个私有的空拷贝构造函数

     ```cpp
     class X
     {
     private:
         X(const X &){...;};
     };
     X x1;
     X x2 = x1; // error , 不能调用私有成员函数
     
     ```

  2. 在函数中使用指针传递，而不使用引用传递

**例子** Cstring

```cpp
#include <string.h>
class Cstring
{
    char *str;
    int sz;

public:
    Cstring(int size)
    {
        sz = size;
        str = new char[sz];
    }
    Cstring(const Cstring &obj)
    {
        ...; //
    }
};
Cstring s1(10);
...;
Cstring s2(s1);
Cstring::Cstring(const Cstring &obj)
{
    sz = obj.sz;
    str = new char[sz];
    memcpy(str, obj.str, sz);
}
...;
```

## 4.3. 指向成员的指针（略）

`Type class_name::*pointertomember`

`Type (class_name::*pointertomember)(arguments_list)`

```cpp
#include <iostream>
using namespace std;

class Widget
{
    void f(int) const { cout << "Widget::f()\n"; }
    void g(int) const { cout << "Widget::g()\n"; }
    void h(int) const { cout << "Widget::h()\n"; }
    void i(int) const { cout << "Widget::i()\n"; }
    enum
    {
        cnt = 4
    };
    void (Widget::*fptr[cnt])(int) const;

public:
    Widget()
    {
        fptr[0] = &Widget::f; // Full spec required
        fptr[1] = &Widget::g;
        fptr[2] = &Widget::h;
        fptr[3] = &Widget::i;
    }
    void select(int i, int j)
    {
        if (i < 0 || i >= cnt)
            return;
        (this->*fptr[i])(j);
    }
    int count() { return cnt; }
};

int main()
{
    Widget w;
    for (int i = 0; i < w.count(); i++)
        w.select(i, 47);
} ///:~
```

输出为

```
Widget::f()
Widget::g()
Widget::h()
Widget::i()
```

---

# 5. 运算符重载

## 5.1. 什么是运算符重载？

一个常用的例子：

```cpp
int i = 10;
i << 2; // 这个<<是左移运算符；
```

```cpp
cout << "hello world"; // 这个“<<”是什么？
```

在`ostream.h`中，原形定义为

```cpp
inline ostream &ostream::operator<<(const unsigned char *_s)
{
    return operator<<((const char *)_s);
}
```

因此

`cout << "Hello world!";` <-> `cout.operator<<("Hello world!");`

**运算符重载**:

​	是一个按特殊格式（`operator@`）声明的函数，从而改变了内部运算符`@`的含义。

## 5.2. 运算符重载的语法

`type classname::operator@(arg_list)`

| `type`     | `classname` | `::` | `operator` | `@`            | `(arg_list)` |
| ---------- | ----------- | ---- | ---------- | -------------- | ------------ |
| 函数返回值 | 类名        |      | 保留字     | 被重载的运算符 | 形式参数     |

`type operator@(arg_list)`

| `type`     | `operator` | `@`            | `(arg_list)` |
| ---------- | ---------- | -------------- | ------------ |
| 函数返回值 | 保留字     | 被重载的运算符 | 形式参数     |

**例子** 复数相加

- 成员函数实现

  ```cpp
  class Complex
  {
  private:
      double rpart;
      double ipart;
  
  public:
      Complex() { rpart = ipart = 0.0; }
      Complex(double rp, double ip)
      {
          rpart = rp;
          ipart = ip;
      }
      Complex add(const Complex &com)
      {
          Complex temp;
          temp.rpart = com.rpart + rpart;
          temp.ipart = com.ipart + ipart;
          return temp;
      }
  };
  
  int main()
  {
      Complex C1(2.0, 3.0);
      Complex C2(5.0, 4.0);
      Complex C3;
      C3 = C2.add(C1); // 要是能写成 C3 = C1 +C2 就好了
  }
  ```

- 特殊成员函数/重载`+`实现

  ```cpp
  class Complex
  {
  private:
      double rpart;
      double ipart;
  
  public:
      Complex() { rpart = ipart = 0.0; }
      Complex(double rp, double ip)
      {
          rpart = rp;
          ipart = ip;
      }
      Complex operator+(const Complex &com)
      {
          Complex temp;
          temp.rpart = com.rpart + rpart;
          temp.ipart = com.ipart + ipart;
          return temp;
      }
  };
  int main()
  {
      Complex C1(2.0, 3.0);
      Complex C2(5.0, 4.0);
      Complex C3;
      C3 = C2 + C1;
  }
  // C3 = C2 + C1 <-> C3 = C2.operator+()
  ```

- 外部函数/重载`+`实现

  ```cpp
  class Complex
  {
  private:
      double rpart;
      double ipart;
  
  public:
      Complex()
      {
          rpart = ipart = 0.0;
      }
      Complex(double rp, double ip)
      {
          rpart = rp;
          ipart = ip;
      }
      friend Complex operator+(
          const Complex &com1,
          const Complex &com2);
  };
  Complex operator+(const Complex &com1, const Complex &com2)
  {
      Complex temp;
      temp.rpart = com1.rpart + com2.rpart;
      temp.ipart = com1.ipart + com2.ipart;
      return temp;
  }
  int main()
  {
      Complex C1(2.0, 3.0);
      Complex C2(5.0, 4.0);
      Complex C3;
      C3 = C2 + C1;
  }
  // C3 = C2 + C1 <-> C3 = operator+(C2, C1)
  ```

### 小结

1. 运算符重载只是一种“语法上的方便” (语法糖) ，实质上是另一种函数调用的方式。

   ```cpp
   C1 + C2 <-> C1.operator+(C2)
   ```

2. 参数列表中的参数个数取决于：

   1. 运算符是一元的还是二元的

   2. 被定义为全局函数还是成员函数

      ```cpp
      C2 + C1 <-> C2.operator+()
      C2 + C1 <-> operator+(C2, C1)
      ```

   3. 对单目后缀运算符，需传递一个`int`常量(哑元常量值)

      ```cpp
      C1++ <-> C1.operator++(1)
      ++C1 <-> C1.operator++()
      ```

3. 运算符重载的限制：

   1. 不能改变内部算符的优先级、结合顺序及运算符的目数

      ```cpp
      C1 + C2 * C3
      <-> C1 + (C2 * C3)
      <-> C1 + C2.operator*(C3)
      <-> C1.operator+(C2.operator*(C3))
      ```

   2. `.` ,`*`, `::`, `?:`, `sizeof`五种运算符不能重载

   3. 重载`=`, `[]`, `()`, `->`都必须是非静态的成员函数，目的是保证它们的第一个操作数是一个自定义对象，从而阻止改变这些运算符作用在基本数据类型上的含义的企图。

      ```cpp
      a[]; // 如果调用的是重载的[]，
           // 则a一定是自定义类型的对象
      ```

### 重载`++`

```cpp
class integer
{
    int i;

public:
    integer(int ii) { i = ii; }
    integer operator++()
    { // 前缀++
        i++;
        return *this;
    }
    integer operator++(int)
    { // 后缀++ ,添加一个哑元
        integer temp = *this;
        i++;
        return temp;
    }
};

int main()
{
    integer a(1), b(2);
    b = a++; // a.operator(1)
    b = ++a; // a.operator()
}
```

## 5.3. 自定义类型转换

### 5.3.1. 引言

:grey_question: 如何实现`complex + double`或`double + complex`？

- 方法一：

  - 内部函数：`complex complex::operator+(complex);`
  - 内部函数：`complex complex:: operator+(double);`
  - 外部函数：`complex operator+(double,complex);`

- 方法二：

  - 转换构造函数

    `T`->`X` 转换,如：`double`->`complex`
    则定义函数：
    ```cpp
    complex complex::complex(const double);
    ```


### 5.3.2. 转换构造函数

- 定义`double`->`Complex`的转换,实现:

  `double + Complex`,`Complex + double`,`Complex + Complex`

  ```cpp
  class Complex
  {
  private:
      double rpart;
      double ipart;
  
  public:
      Complex()
      {
          rpart = ipart = 0.0;
      }
      Complex(double rp, double ip)
      {
          rpart = rp;
          ipart = ip;
      }
      // 转换构造函数
      Complex(const double &d){
          rpart = d;
          ipart = 0.0;
      }
      friend Complex operator+(
          const Complex &com1,
          const Complex &com2);
  };
  Complex operator+(const Complex &com1, const Complex &com2)
  {
      Complex temp;
      temp.rpart = com1.rpart + com2.rpart;
      temp.ipart = com1.ipart + com2.ipart;
      return temp;
  }
  int main()
  {
      Complex C1(2.0, 3.0);
      Complex C2;
      C2 = 3.0 + C1;
  }
  // C2 = 3.0 + C1 
  // <-> C2 = operator+(3.0, C1)
  // <-> C2 = operator+(Complex(3.0), C1F)
  ```

### 5.3.3. 转换运算符

- 定义转换运算符函数`X::operator T(){ //…; };`实现`类X -> T(通常是内部类型)`的转换。 

  - 如：`complex`->`double`

    ```cpp
    double Complex::operator double()
    {
        return rpart;
    }
    
    Complex c1(5.5, 1.1);
    double temp = 1.0;
    // temp = temp + c1; <-> temp = temp + double(c1);
    ```

### 5.3.4. 二义性问题

若同时定义了转换运算符和转换构造函数，会引起二义性。

```cpp
class X
{
    int i;

public:
    friend X operator+(const X &, const X &);
    X(int ii) { i = ii; }        // 转换构造函数
    operator int() { return i; } // 转换运算符
};

X operator+(const X &, const X)
{
    return X(x1.i + x2.i);
}
int main()
{
    X x1(10), x2(12);
    int i = x1 + x2; // 二义性
    i = 12 + x1;     // 二义性
}
```

### 5.3.5. 重载运算符参数匹配规则

1. 如果正好匹配,则不必进行转换,或者简单的调整。如: 数组名转为指针,函数名转为函数指针,类型`T`转为`const T`
2. 如果不匹配,则试着将`char`转为`int`，`short`转为`int`，`float`转为`double`
3. 如果仍不匹配,则进行标准转换。例如,把`int`转为`double`,指向派生类的指针转为指向基类的指针，`unsigned int`  转为`int`
4. 如果还不匹配,则使用用户定义的转换(包括转换构造函数和转换算符)如果不匹配,且函数参数中有...,则用...来匹配任意参数
5. 否则,因类型不匹配而出错

<u>以上规则适用于所有的重载函数</u>

## 5.4. 重要应用举例

### 5.4.1. 重载赋值运算符

```cpp
#include <iostream>
#include <string.h>
using namespace std;

class Message
{
    char *buffer;
    int size;

public:
    Message(int sz = 256);
    Message(const Message &msg);
    Message &operator=(const Message &msg);
    void setmessage(const char *buf);
    const char *getmessage() const;
    virtual ~Message();
};
Message::Message(int sz)
{
    size = sz;
    buffer = new char[size];
    buffer[0] = '\0';
}

Message::~Message()
{
    delete[] buffer;
}

const char *Message::getmessage() const
{
    return buffer;
}
void Message::setmessage(const char *buf)
{
    strcpy(buffer, buf);
}

// 拷贝构造函数
Message::Message(const Message &msg)
{
    size = msg.size;
    buffer = new char[size];
    strcpy(buffer, msg.buffer);
}

Message &Message::operator=(const Message &msg)
{
    if (this != &msg)
    {
        delete[] buffer;
        size = msg.size;
        buffer = new char[size];
        strcpy(buffer, msg.buffer);
    }
    return *this; // 返回*this的引用目的是实现链式赋值
}

int main(int argc, char *argv[])
{
    Message msg(100);
    msg.setmessage("I am Rick");
    cout << msg.getmessage() << endl;

    Message msg2(msg);
    cout << msg2.getmessage() << endl;

    Message msg3;
    msg3 = msg2 = msg; // 链式赋值
    // msg3.operator=（msg2.operator=(msg))
    cout << msg3.getmessage() << endl;

    //	strcpy(msg.getmessage(), "I am Mike");    error!!
    return 0;
}
```

输出为

```cpp
I am Rick
I am Rick
I am Rick
```



:question:什么是安全的类?如何设计一个安全的类？

一个安全的类，通常须有几个特殊的成员函数：

- 构造函数：正确初始化对象
- 析构函数：正确回收对象的空间
- 拷贝构造函数：正确实现用对象去初始化对象
- 重载赋值运算符：正确实现对象间的赋值

```cpp
#include <iostream>
using namespace std;
class MyComplex
{
public:
    double rpart;
    double ipart;
    static int count;
    MyComplex()
    {
        rpart = ipart = 0.0;
        count++;
        cout << "生成对象：" << this << " 初始化构造函数 "
             << "rpart=" << rpart << " Count = " << count << endl;
    }
    MyComplex(const MyComplex &h)
    {
        ++count;
        rpart = h.rpart;
        ipart = h.ipart;
        cout << "生成对象：" << this << " 拷贝构造函数 "
             << "rpart=" << rpart << " Count = " << count << endl;
    }
    ~MyComplex()
    {
        --count;
        cout << "撤消对象：" << this << " 析构函数 "
             << "rpart=" << rpart << " Count = " << count << endl;
    }
    MyComplex operator+(const MyComplex &com)
    {
        cout << "进入重载+" << endl;
        MyComplex temp;
        temp.rpart = rpart + com.rpart;
        temp.ipart = ipart + com.ipart;
        return temp;
    }
    MyComplex &operator=(const MyComplex &com)
    {
        cout << "重载赋值运算符" << endl;
        ipart = com.ipart;
        rpart = com.rpart;
        return *this;
    }
    MyComplex(double rp, double ip)
    {
        count++;
        rpart = rp;
        ipart = ip;
        cout << "生成对象：" << this << " 初始化构造函数"
             << "rpart=" << rpart << " count = " << count << endl;
    }
};
int MyComplex::count = 0;
int main()
{
    MyComplex a(1, 1), b(2, 2), c(3, 3), d(4, 4);
    cout << " ================================" << endl;
    MyComplex e = c + d;
    MyComplex f = d;
    MyComplex g;
    cout << " ================================" << endl;
    g = d;
    return 0;
}
```

输出为

```
生成对象：0x61fe00 初始化构造函数rpart=1 count = 1
生成对象：0x61fdf0 初始化构造函数rpart=2 count = 2
生成对象：0x61fde0 初始化构造函数rpart=3 count = 3
生成对象：0x61fdd0 初始化构造函数rpart=4 count = 4
 ================================
进入重载+
生成对象：0x61fdc0 初始化构造函数 rpart=0 Count = 5
生成对象：0x61fdb0 拷贝构造函数 rpart=4 Count = 6
生成对象：0x61fda0 初始化构造函数 rpart=0 Count = 7
 ================================
重载赋值运算符
撤消对象：0x61fda0 析构函数 rpart=4 Count = 6
撤消对象：0x61fdb0 析构函数 rpart=4 Count = 5
撤消对象：0x61fdc0 析构函数 rpart=7 Count = 4
撤消对象：0x61fdd0 析构函数 rpart=4 Count = 3
撤消对象：0x61fde0 析构函数 rpart=3 Count = 2
撤消对象：0x61fdf0 析构函数 rpart=2 Count = 1
撤消对象：0x61fe00 析构函数 rpart=1 Count = 0
```

### 5.4.2. 重载[]实现关联数组

```cpp
#include <iostream>
using namespace std;
class Ctuition
{
    int total[5];

public:
    Ctuition()
    {
        total[0] = 5000; // “CS”
        total[1] = 5000; // “EE”
        …;
    }
    void SetTuition(const char *dept, int t)
    {
        int i;
        if (dept == "CS")
            i = 0;
        else if (dept == "EE")
            i = 1;
        …;
        total[i] = t;
    }
    int GetTuition(const char *dept)
    {
        int i;
        if (dept == "CS")
            i = 0;
        else if (dept == "EE")
            i = 1;
        …;
        return total[i];
    }
};
void main()
{
    Ctuition ustc2005;
    cout << ustc2005.GetTuition("EE");
    ustc2005.SetTuition("CS", 6000);
}
```

- `Ctuition`的缺点：实现不直观，对数组中的分量操作起来不方便。
- 改进：通过重载`[ ]`实现关联数组。使得对对象中数组分量的操作象内定义类型一样方便。

```cpp
#include <iostream>
using namespace std;
class Ctuition2
{
    int total[5];

public:
    Ctuition2()
    {
        total[0] = 5000; // "CS"
        total[1] = 5000; // "EE"
        …;
    }

    int &operator[](const char *dept)
    {
        switch (dept) // 实际上不能这样写，只是给出一个思路
        {
        case "CS":
            return total[0];
        case "EE":
            return total[1];
            …;
        }
    }
};

int main()
{
    Ctuition2 ustc2005;
    cout << ustc2005["EE"];
    // 等价于：ustc2005.operator[ ]("EE");
    ustc2005["CS"] = 6000;
}

```

### 5.4.3. 重载()实现迭代运算(迭代器)

略，在容器中详细叙述

---

# 6. 继承和派生类

## 6.1. 引言：代码重用的两种机制

1. 组合：创建一个新类，类中的一个或多个数据成员是（已经存在的）其他类的对象。

   例：
   
   $$
   \text{计算机}=\text{主板}+\text{CPU}+\text{内存}+\text{显示器}+\cdots
   $$

   ```cpp
   class CComputer
   {
       Cpower Foxconn200;    // 富士康
       CMainBoard Intel915G; // Intel主板
       CCPU Prescott;        // Prescott处理器
       CMem SEC400X72C3;     // Samsung内存条
       ...;
   };
   ```

2. 继承：在现有的类（称为“基类”）的基础上，创建一个新类（称为“派生类”）。派生类“共享”基类的所有数据成员和成员函数；也可以进一步添加自己的成员。

   例：`Dell C640DV`拥有`C640`的所有配置；此外，还增加了支持`DV`的`i-link 1394`接口。

   ```cpp
   class C640DV: public C640 {
   	C1394 canopusDvRex;
   }
   ```

   继承的例子：

   | Base class | Derived classes                                    |
   | ---------- | -------------------------------------------------- |
   | Student    | GraduateStudent<br />UndergraduateStudent          |
   | Shape      | Circle<br />Triangle<br />Rectangle                |
   | Loan       | CarLoan<br />HomeImprovementLoan<br />MortgageLoan |
   | Employee   | FacultyMember<br />StaffMember                     |
   | Account    | CheckingAccount<br />SavingsAccount                |

### 6.1.1. 组合

```cpp
class X
{
    int i;

public:
    X() { i = 0; }
    void set(int ii) { i = ii; }
    int read() const { return i; }
    int permute() { return i = i * 47; }
};

// new class
class Y
{
    int i;

public:
    X x; // Y的成员是X的对象
    Y() { i = 0; }
    void f(int ii) { i = ii; }
    int g() const { return i; }
};

int main()
{
    Y y;
    y.f(47);
    y.x.set(37); // 访问内嵌对象
} ///:~
```

### 6.1.2. 单继承

```cpp
class DerivedClassName : public/private/protected BaseClassName{
    // new data
    // new function 
}
```

派生类继承基类的<u>所有的</u>数据成员和成员函数，除了

1. 基类的构造函数和析构函数
2. 基类中用户定义的`new`和赋值运算符
3. 基类的友元关系

**例子**

- 基类`employee`

  ```cpp
  class employee
  {
  private:
      char *name;
      short age;
      float salary;
  
  public:
      employee()
      {
          name = 0;
          age = 0;
          salary = 0.0;
      }
      employee(char *name1, short age1, float salary1)
      {
          name = new char[strlen(name1) + 1];
          age = age1;
          salary = salary1;
      }
      void print() const
      {
          cout << "name:" << name << "age:" << age
               << "salary:" << salary << endl;
      }
      virtual ~employee()
      {
          delete[] name;
      }
  }; //~基类 employee
  ```

- 派生类`manager1`

  ```cpp
  class manager1 : public employee
  {
      // 继承了基类的所有数据成员和成员函数。
  };
  
  manager1 m;
  m.print(); //打印manager的name,age,salary
  ```

### 6.1.3. 多重继承（其意义尚有争议）

```cpp
class manager
{
    ...;
};
class temporary
{
    ...;
};

//类consultant(顾问)
class consultant : public manager, public temporary
{
    ...;
};

```

:warning: 当基类中含同名成员时，存在二义性的问题！

### 6.1.4. 继承和组合的联合

```cpp
class A
{
    int i;

public:
    A(int ii) : i(ii) {}
    ~A() {}
    void f() const {}
};
class B
{
    int i;

public:
    B(int ii) : i(ii) {}
    ~B() {}
    void f() const {}
};
class C : public B
{
    A a;

public:
    C(int ii) : B(ii), a(ii) {}
    ~C() {} // Calls ~A() and ~B()
    void f() const
    { // Redefinition
        a.f();
        B::f();
    }
};
int main()
{
    C c(47);
} ///:~
```

### 6.1.5. 组合 :vs: 继承

```cpp
class Base
{
public:
    int i;
    void b();
};
class Derive : public Base
{
public:
    void d();
};
class Member
{
public:
    Base ba;
    void d();
};
```

1. **概念**：`Member`是组合,`Derive`是继承

2. **用途**：代码重用

3. **操作**：

   ````cpp
   Base base;
   Derive derive;
   Member member;
   cout << base.i
        << derive.i
        << member.ba.i;
   base.b();
   derive.b();
   member.ba.b()
   ````

4. **继承**：派生类“是一个”基类

   **组合**：类中“有一个”其他类的成员对象

## 6.2. 继承的访问控制

### 6.2.1. 访问控制

1. 基类对派生类的访问控制 
2. 基类对派生类的对象的访问控制
3. 基类对派生类的派生类的访问控制

```cpp
class base
{
private:
    int i1;

protected:
    int j1;

private:
    int f1();
};
class drv : public/private/protected base
{
private:
    int i2;

protected:
    int j2;

public:
    int f2();
};
int main()
{
    drv d1;
}
```

:grey_question: 如何访问`d1`的数据成员和成员函数？

|                      | 基类成员对派生类的可见性                                     | 基类成员对派生类对象的可见性             |
| -------------------- | ------------------------------------------------------------ | ---------------------------------------- |
| 公有继承 `public`    | 公有成员和保护成员可见，私有成员不可见。                     | 公有成员可见，保护成员和私有成员不可见。 |
| 私有继承 `private`   | 公有成员和保护成员可见，私有成员不可见。<br />所有成员对<u>派生类的派生类</u>的成员不可见(即转变为派生类的私有成员) | 所有成员都不可见                         |
| 保护继承 `protected` | 公有成员和保护成员可见，私有成员不可见。<br />公有成员和保护成员对<u>派生类的派生类</u>的成员可见。 | 所有成员都不可见                         |

### 6.2.2. public继承

```cpp
class manager2 : public employee
{
public:
    int level;
    void m_print()
    {
        cout << “level” << level;
        // cout << age;  error 不能访问基类私有成员；
        print(); //OK,可以访问基类公有成员；
    }
};

manager2 m;
m.print(); //OK, 打印manager的ame,age,salary
```

### 6.2.3. private继承

```cpp
class manager3 : private employee
{
public:
    int level;
    void m_print()
    {
        cout << “level” << level;
        //cout << age;    错！不能访问基类私有成员；
        print();
    }
};

manager2 m;
m.print(); //manager3中的私有成员，不能访问
m.m_print(); // OK
```

#### 对private继承成员公有化

```cpp
class Pet
{
public:
    char eat() const { return 'a'; }
    int speak() const { return 2; }
    float sleep() const { return 3.0; }
    float sleep(int) const { return 4.0; }
};

class Goldfish : Pet
{ // 私有继承
public:
    Pet::eat;   // 公有化
    Pet::sleep; // 重载的成员均被公有化
};
int main()
{
    Goldfish bob;
    bob.eat();
    bob.sleep();
    bob.sleep(1);
    //! bob.speak();// Error: private member function
} ///:~

```

### 6.2.4. protected继承

```cpp
class Base
{
    int i;

protected:
    int read() const { return i; }
    void set(int ii) { i = ii; }

public:
    Base(int ii = 0) : i(ii) {}
    int value(int m) const { return m * i; }
};

class Derived : protected Base
{
    int j;

public:
    Derived(int jj = 0) : j(jj) {}
    void change(int x) { set(x); }
};

int main()
{
    Derived d;
    d.change(10);
    d.set(10); // 错误，保护成员不能访问
} ///:~
```

<u>protected继承不常用，只是为了语言的完备性。</u>

### 6.2.5. 小结

| 基类性质  | 继承性质  | 基类成员在派生类中性质 | 派生类对基类可访问性 | 派生类对象对基类的可访问性 | 派生类的派生类对基类的可访问性 |
| --------- | --------- | ---------------------- | -------------------- | -------------------------- | ------------------------------ |
| public    | public    | public                 | Yes                  | Yes                        | Yes                            |
| protected | public    | protected              | Yes                  | 不可访问                   | Yes                            |
| private   | public    | --                     | 不可访问             | 不可访问                   | 不可访问                       |
|           |           |                        |                      |                            |                                |
| public    | protected | protected              | Yes                  | 不可访问                   | Yes                            |
| protected | protected | protected              | Yes                  | 不可访问                   | Yes                            |
| private   | protected | --                     | 不可访问             | 不可访问                   | 不可访问                       |
|           |           |                        |                      |                            |                                |
| public    | private   | private                | Yes                  | 不可访问                   | 不可访问                       |
| protected | private   | private                | Yes                  | 不可访问                   | 不可访问                       |
| private   | private   | --                     | 不可访问             | 不可访问                   | 不可访问                       |

## 6.3. 继承与构造、析构函数

### 6.3.1. 继承与构造、析构函数

基类中不能被继承的部分：

1. <u>构造函数</u>
2. <u>析构函数</u>
3. 重载的`new`、赋值运算符
4. 友员关系

但是，在派生类的构造函数和重载赋值运算符中可以调用基类的构造函数和重载赋值运算符。

### 6.3.2. 如何初始化基类成员？ 

```cpp
class manager : public employee
{
    int level;

public:
    manager(char *name1, short age1, float, salary1, int level1)
        : employee(name1, age1, salary1)
    {
        level = level1;
    }
    void m_print()
    {
        cout << "salary:" << salary << "level" << level << endl;
        print();
    }
};

manager m("Maggie", 20, 6000, 1);
```

### 6.3.3. 构造、析构函数的调用次序

- 创建一个派生类的对象，调用次序为：
  1. 调用基类的构造函数,初始化基类成员；
  2. 调用数据成员对象的构造函数(若有多个，则按类中定义的先后次序调用)，初始化成员对象；
  3. 调用派生类自己的构造函数；初始化派生类成员。
- 析构一个派生类的对象，其调用次序正好相反：
  1. 调用派生类自己的析构函数；
  2. 调用数据成员对象的析构函数(若有多个，则按类中定义的先后次序逆序调用)；
  3. 调用基类的析构函数。

## 6.4. 继承与特殊成员函数

### 6.4.1. 继承与静态成员函数

- 静态成员函数可被继承到派生类中；
- 如果重新定义了一个静态成员，所有在基类中的其他重载函数会被隐藏；
- 如果改变了基类中一个函数的签名(signature),所有使用该函数名字的基类版本都将被隐藏。

与非静态成员函数的不同点：静态成员函数不可以是虚函数。

### 6.4.2. 继承与运算符重载函数（略）

## 6.5. 名字隐藏

### 6.5.1. 重定义（redefining)

在派生类中，重新定义基类的成员函数。

```cpp
#include <iostream>
using namespace std;
// 基类
class employee
{
public:
    short age;
    employee() { age = 0; }
    employee(short age1) { age = age1; }
    void print() const
    {
        cout << "base age:" << age << endl;
    }
};
// 派生类
class manager : public employee
{
public:
    int level;
    short age;
    manager(short age1, int level1)
        : employee(age1)
    {
        age = age1 + 1;
        level = level1;
    }
    void print()
    {
        cout << "derive age : " << age << endl;
        employee::print();
    }
};
// 主函数
int main()
{
    manager m(24, 1);
    m.print();
    employee &pref = m;
    pref.print(); //调用基类的print();
}

```

输出为

```
derive age : 25
base age:24
base age:24
```

### 6.5.2. 重写（overriding）

若被重新定义的基类函数是虚函数，则称派生类中同名函数为重写。(<u>注意：一组虚函数的参数和返回值必须相同。</u>）

```cpp
class shape
{
    …;
    virtual void draw(); // 基类中的虚函数
};

class point：shape
{
    …;
    void draw(); // overriding
};
class circle：point
{
    // …
    void draw(); // overriding
}
```

### 6.5.3. 名字隐藏（name hiding）

在重新定义基类的重载函数时(可以改变参数或返回值)，则基类中的所有其他版本的重载函数被自动隐藏了，对派生类的对象不可见。

```cpp
#include <iostream>
using namespace std;
class Base
{
public:
    int f() const
    {
        cout << "Base::f()\n";
        return 0;
    }
    int f(string) const
    {
        return 1;
    }
};
class Derived1 : public Base
{
public:
    // Redefinition:
    int f() const
    {
        cout << "Derived1::f()\n";
        return 1;
    } // int f(string)对Derived1不可用
};

class Derived2 : public Base
{
public:
    // Change return type:
    void f() const { cout << "Derived2::f()\n"; }
}; // int f()和int f(string)对Derived2都不可用
class Derived3 : public Base
{
public:
    // Change argument list:
    int f(int) const
    {
        cout << "Derived3::f()\n";
        return 3;
    } // int f()和int f(string)对Derived3都不可用
};

int main()
{
    int x;
    string s("hello");
    Derived1 d1;
    x = d1.f();
    //!  d1.f(s);    // error ! string version hidden
    Derived2 d2;
    //!  x = d2.f(); // error! return int version hidden
    //！ d2.f(s)     // string version hidden
    Derived3 d3;
    //!  x = d3.f(); // f() version hidden
    x = d3.f(1);
} ///:~
```

输出为

```
Derived1::f()   
Derived3::f()
```

## 6.6. 渐增式开发

### 6.6.1. 什么是“渐增式开发”？

假设你写好了一段代码C1，C1没有任何bug;现在你往代码C1中添加新的代码C2，扩充C1的功能,如果能保证不会因为代码的扩充给C1带来新的bug,也就是说，如果代码扩充后有bug，则bug只可能出现在C2中。我们称这种代码开发的方式为“渐增式开发”。

### 6.6.2. 继承、组合与“渐增式开发”

- 继承和组合都支持“渐增式开发”。
  - 派生类中的bug不可能影响到基类
  - 新类中的bug不可能影响到成员对象所属的类

## 6.7. 向上类型转换

### 6.7.1. 派生类和基类的关系

- “是一个”的关系
  - 派生类拥有所有的基类的数据成员和成员函数；
  - 派生类对象可以当成一个基类对象来看待；但反过来不可以。
  - 任何发送给基类的消息派生类都可以接收
- 继承首先是代码重用的一种机制；但另一个重要的特征在于：继承描述了一组类，这组类在一个相关联的“层次”上，其中，一个类可以看作是另一个类。

### 6.7.2. 向上类型转换

- 继承最重要的思想实际上是说：派生类同时也是一个基类。
  - 也就是说：你可以发送给基类的任何消息，同样可以发送给派生类！
- 编译器通过允许被称作“向上类型转换(`Upcast-ing`)”的机制来继承的这一特征。

```cpp
enum note
{
    middleC,
    Csharp,
    Cflat
}; // Etc.

class Instrument
{
public:
    void play(note) const {}
};

// Wind objects are Instruments
// because they have the same interface:
class Wind : public Instrument
{
};
// 由于派生类的对象同时也是基类的一个对象，因此，
// tune()函数对Instrument和从Instrument派生出
// 来的任何类都是有效的。
void tune(Instrument &i)
{
    // ...
    i.play(middleC);
}
int main()
{
    Wind flute;
    // Upcasting
    // 在函数调用时，实际上进行了一个转换，
    // 将Wind的引用转换成Instrument的引用。
    tune(flute);
} ///:~
```

这种将派生类的引用或指针转换成基类引用或指针的活动称为“向上类型转换” （Upcasting!）

- 向上类型转换不仅仅发生在虚实结合时，例如：

  ```cpp
  Wind w;
  Instrument * ip = &w ;   // upcasting 
  Instrument & ir = w ;     // upcasting
  ```

- 向上类型转换总是安全的：

  - 从派生类接口转换到基类接口可能出现的唯一的事情是失去成员，而不是获得他们

- 编译器允许向上类型转换；既不需要显式的转换，也不需要其他特别的标记。

### 6.7.3. upcasting与拷贝构造函数

```cpp
// Correctly creating the copy-constructor
#include <iostream>
using namespace std;

class Parent
{
    int i;

public:
    Parent(int ii) : i(ii)
    {
        cout << "Parent(int ii)\n";
    }
    Parent(const Parent &b) : i(b.i)
    {
        cout << "Parent(const Parent&)\n";
    }
    Parent() : i(0) { cout << "Parent()\n"; }
    friend ostream &
    operator<<(ostream &os, const Parent &b)
    {
        return os << "Parent: " << b.i << endl;
    }
};

class Member
{
    int i;

public:
    Member(int ii) : i(ii)
    {
        cout << "Member(int ii)\n";
    }
    Member(const Member &m) : i(m.i)
    {
        cout << "Member(const Member&)\n";
    }
    friend ostream &
    operator<<(ostream &os, const Member &m)
    {
        return os << "Member: " << m.i << endl;
    }
};

class Child : public Parent
{
    int i;
    Member m;

public:
    Child(int ii) : Parent(ii), i(ii), m(ii)
    {
        cout << "Child(int ii)\n";
    }
    friend ostream &
    operator<<(ostream &os, const Child &c)
    {
        return os << (Parent &)c << c.m
                  << "Child: " << c.i << endl;
    }
};
int main()
{
    Child c(2);
    cout << "calling copy-constructor: " << endl;
    Child c2 = c; // Calls copy-constructor
    cout << "values in c2:\n"
         << c2;
} ///:~
```

输出为

```
Parent(int ii)
Member(int ii)
Child(int ii)
calling copy-constructor:
Parent(const Parent&)
Member(const Member&)
values in c2:
Parent: 2
Member: 2
Child: 2
```

### 6.7.4. 非显式类型转换

- 基类和派生类之间对象的赋值
  - 派生类的类型和基类的类型并不相同
  - 派生类的对象可以当成基类对象来处理
    - 派生类中包含基类中所有的成员
    - 派生类中可能包含基类中没有的成员
    - 派生类可以赋值给基类
  - 基类对象不能当成派生类的对象来处理
    - 会造成多出来的那些派生类成员未定义
    - 基类不能赋值给派生类
    - 但可以通过重载赋值运算符来允许这样的赋值

- 通过指针来引用对象
  - 通过基类指针引用基类对象
    - Allowed
  - 通过派生类指针引用派生类对象
    - Allowed
  - 通过基类指针引用派生类对象
    - 可能会出现语法错误
    - 代码中只能引用基类的成员，否则会有语法错误
  - 通过派生类指针引用基类对象
    - 隐式转换会造成语法错误(Not Allowed)
    - 否则，派生类指针必须先转换成基类指针（显式转换）

### 6.7.5. 向下类型转换

- 当基类指针(或引用)转换到派生类指针(或引用)，称作“向下类型转换”。
- 向下类型转换不安全，因此，必须显式地进行(显式类型转换)。

### 6.7.6. 向上类型转换的问题

向上类型转换会丢失对象的类型信息。

```cpp
Wind  w;
Instrument *iptr = &w; // upcasting
iptr->play(middleC);
```

<u>由于`iptr`的静态类型`Instrument*`; 在编译时该函数调用将被绑定到`Instrument::play()`;而不是`Wind::play()`！</u>

如何解决：利用多态（polymorphism）！

## 6.8. 小结

- 继承和组合都允许由已存在的类型创建新类型；
- 如果想重用已有类的内部实现的话，最好用组合；如果想重用已有类的接口的话，就应使用继承。
- 继承描述的类之间的层次关系是多态的前提。

---

# 7. 多态性和虚函数

## 7.1. 多态性(Polymorphism)

### 7.1.1. 早绑定(Early Binding)



# 8. 模板(Template)

## 8.1. 引言：IntStack

- 创建一个栈，这个栈类只存放`int`类型的值
  1. 支持初始化、`push()`、`pop()`、清除等方法；
  2. 栈中所有元素的类型都是`int`

用来存放`int`的栈类`IntStack`

```cpp
class IntStack
{
    int *v; // 栈底
    int *p; // 栈顶
    int sz; // 栈的大小
public:
    IntStack(int s)
    {
        v = p = new int[sz = s];
    }
    ~IntStack() { delete[] v; }
    void push(int a){ *p++ = a};
    int pop() { return *--p; }
    int size() const { return p - v; }
};

```

## 8.2. 代码重用

现在希望再创建能存放其他类型的栈类，如：`char`, `float`, 甚至用户自定义的类型对象。 

从栈的结构和操作方式上来说，除了每个元素的类型不一样外，其它没有任何区别。因此应该可以重用`IntStack`的代码。

<u>问题是：如何重用？</u>

### 8.2.1. C的方法之一：代码拷贝

```cpp
class CharStack
{
    char *v; // 栈底
    char *p; // 栈顶
    int sz;  // 栈的大小
public:
    CharStack(int s) { v = p = new char[sz = s]; }
    ~CharStack() { delete[] v; }
    void push(char a){ *p++ = a};
    char pop() { return *--p; }
    int size() const { return p - v; }
};
```

缺点：
1. 表现繁琐;
2. 易发生错误;
3. 缺乏美感;

非常低效！

### 8.2.2. C的方法之二：typedef

```cpp
typedef int T;
class Stack
{
    T *v;   // 栈底
    T *p;   // 栈顶
    int sz; // 栈的大小
public:
    Stack(int s) { v = p = new T[sz = s]; }
    ~Stack() { delete[] v; }
    void push(T a){ *p++ = a};
    T pop() { return *--p; }
    int size() const { return p - v; }
};
```

缺点：
1. 每次使用`Stack`之前，都必须加上`typedef`语句，很麻烦。
2. 由于`T`是全局名，无法重新定义，一个类不能同时用到`char`栈和`int`栈；

### 8.2.3. smalltak方法(略)

使用继承,复杂,混乱!

### 8.2.4. C++的方法：模板

改进`typedef`，将它从预处理器移入到编译器中。新的代码替换装置称为“模板”(`template`)。

- 非常象宏，却更清晰，更易于使用；
- 模板实际上是一组类；
- 形式上很简洁。

## 8.3. 模板语法

### 8.3.1. 类模板定义

```cpp
template <class T>
class 类模板名
{
    // 类模板的定义
};
```

T是一个类模板的类型参数，可以有一个或多个，可以是任意类型。

**例子** `Stack<T>`

```cpp
template <class T>
class Stack
{
    T *v;   // 栈底
    T *p;   // 栈顶
    int sz; // 栈的大小
public:
    Stack(int s) { v = p = new T[sz = s]; }
    ~Stack() { delete[] v; }
    void push(T a){ *p++ = a};
    T pop() { return *--p; }
    int size() const { return p - v; }
};
```

### 8.3.2. 类模板实例化

给类模板的参数指定具体的类型，这一过程称为“类模板的实例化”。

`类模板名<具体类型表>`

```cpp
Stack<int>;  // 实例化成 int 型栈类；
Stack<char>; // 实例化成 char 型栈类；
```

:warning: 类模板实例化后的结果是类，而不是对象！

### 8.3.3. 类对象生成

类模板实例化得到的类可以进行实例化，生成最终的对象。

```cpp
Stack<int> si(20) ; // 创建一个大小为20的整形栈；
Stack<char> si(40); // 创建一个字符型栈；
Stack stack(100);   // error, 未指定模板参数，
```

![](/assets/img/CPlusPlus/image-20210710230546112.png)
_类模板实例化_

**完整的`Stack<T>`程序**

```cpp
template <class T>
class Stack
{
    T *v;   // 栈底
    T *p;   // 栈顶
    int sz; // 栈的大小
public:
    Stack(int s) { v = p = new T[sz = s]; }
    ~Stack() { delete[] v; }
    void push(T a){ *p++ = a};
    T pop() { return *--p; }
    int size() const { return p - v; }
};
void main()
{
    int i;
    Stack<char> sch(20);
    Stack<char> sch2(20);
    Stack<int> sint(10);
    for (i = 0; i < 10; i++)
    {
        sint.push(i + 1);
    }

    for (i = 0; i < 20; i++)
    {
        sch.push('*');
    }

    // ….

    for (i = 10; i > 0; i--)
    {
        if (sint.pop() != i)
        {
            error();
        }
    }

    for (i = 0; i < 20; i++)
    {
        if (sch.pop() != '*')
        {
            error();
        }
    }
}
```

### 8.3.4. 非内联函数定义

~一个值得注意的问题！~

#### 8.3.4.1. 语法

`template<class T>`
`返回值类型 类模板名<类模板参数>::成员函数名(函数参数1,函数参数2,函数参数3)`
非内联函数定义的`Stack<T>`

```cpp
template <class T>
class Stack
{
    T *v;   // 栈底
    T *p;   // 栈顶
    int sz; // 栈的大小
public:
       Stack(int s;
       ~Stack() ;        
       void push(T a);
       T pop() ;  
       int size() const;
};

template <class T>
Stack<T>::Stack(int s)
{
    v = p = new T[sz = s];
}
template <class T>
Stack<T>::~Stack() { delete[] v; }

template <class T>
void Stack<T>::push(T a){*p++ = a};

template <class T>
T Stack<T>::pop() { return *--p; }

template <class T>
int Stack<T>::size() const { return p - v; }
```

#### 8.3.4.2. 关于头文件

- 对类来说，在创建非内联函数定义时，我们通常把定义放在`.h`文件中，而把实现放在`.cpp`中。

  - 实际上是所谓的头文件原则：“在头文件中，不要放置分配存储空间的任何东西”，为了防止在连接期间的多重定义错误。

- 对模板来说，即使是在创建非内联函数定义时，模板的所有定义和实现<u>都必须放入一个头文件</u>中。

  - 因为模板很特殊，在`template<…>`之后的任何东西都意味着编译器在当时不为它分配存储空间（即不编译成目标代码），而是一直处于等待状态直到一个模板示例被告知。

  - 如果没有被实例化，模板不会被编译成目标代码：

    - `Stack.h`

      ```cpp
      template <class T>
      class Stack
      {
          T *v;   // 栈底
          T *p;   // 栈顶
          int sz; // 栈的大小
      public:
          Stack(int s);
          ~Stack();
          void push(T a);
          T pop();
          int size();
      };
      ```

    - `Stack.cpp`

      ```cpp
      template <class T>
      Stack<T>::Stack(int s) { v = p = new T[sz = s]; }
      
      template <class T>
      Stack<T>::~Stack() { delete[] v; }
      
      template <class T>
      void Stack<T>::push(T a){*p++ = a};
      
      template <class T>
      T Stack<T>::pop() { return *--p; }
      
      template <class T>
      int Stack<T>::size() const { return p - v; }
      ```

    - `test.cpp`

      ```cpp
      #include “Stack.h”
      int main()
      {
      	Stack<int> si(10);
      	si.push(1);     // 连接错误
      }
      ```

    - 发生连接错误：

      ```
      error LNK2001: unresolved external symbol "public: int __thiscall Stack<int>::push(int)" (?push@?$stack@H@@QAEHXZ)
      ```

      From MSDN:

      - Linker Tools Error LNK2001
      - unresolved external symbol "symbol"
      - Code will generate this error message if it references something (like a function, variable, or label) that the linker can’t find in all the libraries and object files it searches. In general, there are two reasons this error occurs: what the code asks for doesn’t exist (the symbol is spelled incorrectly or uses the wrong case, for example), or the code asks for the wrong thing (you are using mixed versions of the libraries?some from one version of the product, others from another version). 
      - Numerous kinds of coding and build errors can cause LNK2001. Several specific causes are listed below, and some have links to more detailed explanations.
        ......

### 8.3.5. 模板中的常量

模板参数并不局限于类定义的类型，可以使用编译器内置类型。

```cpp
template <class T, int size = 100>
class Array
{
    …;
};
```

**例子**

```cpp
#include "../require.h"
#include <iostream>
using namespace std;

template <class T, int size = 100>
class Array
{
    T array[size]; // 在实例化时，设置Array类的长度
public:
    T &operator[](int index)
    { // 重载[ ] 实现关联数组
        require(index >= 0 && index < size,
                "Index out of range");
        return array[index];
    }
    int length() const { return size; }
};
class Number
{
    float f;

public:
    Number(float ff = 0.0f) : f(ff) {}
    Number &operator=(const Number &n)
    { // 重载赋值符
        f = n.f;
        return *this;
    }
    operator float() const { return f; }
    friend ostream &operator<<(ostream &os, const Number &x)
    {
        return os << x.f;
    }
};
template <class T, int size = 20>
class Holder
{
    Array<T, size> *np;

public:
    Holder() : np(0) {}
    T &operator[](int i)
    {
        require(0 <= i && i < size);
        if (!np)
            np = new Array<T, size>;
        return np->operator[](i);
    }

    int length() const { return size; }
    ~Holder() { delete np; }
};
```

## 8.4. 模板的派生

### 8.4.1. 从类派生模板

模板实际上是一组类，如果某一类定义了这一组类的公共属性，则模板可以从该类派生。

```cpp
#include <iostream>
using namespace std;

class Base
{
    int i;

protected:
    float f;

public:
    void g() { cout << "g" << endl; }
};

template <class T>
class derived : public Base
{
    T t;
    // …
};

int main()
{
    derived<int> di;
    derived<char> dc;
    di.g(); //
    dc.g();
}
```

输出为

```
g
g
```

### 8.4.2. 从类模板派生模板

类模板的基类如果也是模板，派生类模板的参数表应包含基类模板的参数。

```cpp
#include <iostream>
using namespace std;
template <class T>
class Base
{
    int i;

protected:
    T f;

public:
    void g() { cout << "g" << endl; }
};

template <class T1, class T2>
class derived : public Base<T2>
{
    T1 t;
    // …
};

int main()
{
    derived<int, char> di;
    derived<char, float> dc;
    di.g(); //
    dc.g();
}
```

输出为

```
g
g
```

如果派生类没有类型参数，或者说它的类型参数与基类的类型参数相同时，派生类地模板参数只要包含基类的模板参数就可以了

```cpp
template <class T>
class C:public Base<T>   // 模板C的参数与基类相同
{ 
	T c;
	// …
};
```

## 8.5. 函数模板

~实际上是定义了一组函数~

### 8.5.1. 函数模板的定义

```cpp
template <模板参数表> 
返回值类型 函数名（函数参数表）
{
	//函数模板的定义
}
```

**例1** 求两个对象间的最大值

```cpp
template <class T>
T max(T a, T b)
{
    return a > b ? a : b;
}
// 这里T可以是 int, char ,float, 或者任何重载了>算符的对象。
```

**例2** 关于模板参数

```cpp
template <class T>
void f()
{
    // error, 函数参数列表中无函数模板参数T
    T a;
    // …
}
```

### 8.5.2. 函数模板的实例化

- [x] 函数模板的实例化不需要用户显式进行，而是在函数调用时由编译器来处理。

```cpp
int main()
{
    int a, b;
    char c, d;
    int m1 = max(a, b);  //调用max(int a, int b);
    char m2 = max(c, d); // 调用max(char c,char d);
}
```

- [ ] 应保证函数调用时的参数与模板参数正好匹配，因为编译器不会为函数模板的参数提供任何形式的转换！

```cpp
int a;
char c;
int m3=max(a,c) // 出错，不能生成 max(int, char)
```

### 8.5.3. 函数模板的重载

函数模板的重载跟普通函数重载一样，也应该保证两个重载的函数模板参数有所不同。

```cpp
#include <iostream>
using namespace std;
template <class T>
T sum(T *array, int size)
{
    T total;
    for (int i = 0; i < size; i++)
        total += array[i];
    return total;
}

template <class T>
T sum(T *array, T *array2, int size)
{
    T total;
    for (int i = 0; i < size; i++)
        total + = array1[i] + array2[i];
    return total;
}

void main()
{
    static int intarr1[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    static int intarr2[] = {2, 4, 6, 8, 10, 12, 14, 16};
    static double douarr[] = {1.1, 2.2, 3.3, 4.4, 5.5, 6.6,
                              87.7, 8.8, 9.9, 10.0};
    int itotal = sum(intarr1, 10); //
    double dtotal = sum(douarr, 10);
    int iatotal = sum(intarr1, intarr2, 8);
}
```

## 8.6. 小结

- 模板是一种代码重用的机制；
- 模板是在强类型语言上实现的一种弱类型机制。

## 8.7. 类型安全的通用模板类

### 8.7.1. 单链表类

$$
\text{单向链表结点}= \text{数据结点} + \text{链指针结点}
$$

1. 链结点:`slink`

   ```cpp
   struct slink
   {
       slink *next;
       slink() { next = 0; }
       slink(slink *p) { next = p; }
   };
   ```

2. 表结点:`name`

   ```cpp
   #include <string.h>
   class name : public slink
   {
       char *s;
   
   public:
       name(const char *ss)
       {
           s = new char[strlen(ss) + 1];
           strcpy(s, ss);
       }
       // …
   };
   ```

3. 单链表类:`slist_base`

   ```cpp
   class slist_base
   {
       // ...
   public:
       int insert(slink *); // upcasting,可以处理不同类型的结点
       int append(slink *); // upcasting
       slink *get();
   };
   ```

4. 使用单链表

   ```cpp
   void f(const char *s)
   {
       slist_base slb;
       slb.insert(new name(s));
       // …
       name *p = (name *)slb.get();
       // …
       delete p;
   }
   ```

### 8.7.2. 扩充：链表结点expr

```cpp
class expr : public slink
{
    // …
};

int main()
{
    slist_base slb;
    slb.insert(new expr);
    slb.insert(new name(“s1”));
    slb.insert(new expr);
    // …
    expr *e1 = (expr *)(slb.get());
    expr *e2 = (expr *)(slb.get()); // 使用e2可能出错
}
```

### 8.7.3. 有什么问题？

- 由于`slist_base`是按`slink`而不是`name`来定义对链表的操作，当对象放入链表之后，其静态类型信息也就消失了。当从链表中取出它时，需要进行强制类型转换，把链表中存放的对象转换成它原来的类型`name`。**但是：要记住每次取出对象的具体类型是很困难的。**
- 难题：如何保证链表中的类型安全性？
  - 若用虚函数，不必进行强制类型转换，没有什么问题。但用户只能通过虚函数的动态绑定来使用基类提供的接口
  - 若要对具体的派生类进行处理，则注定了这种异质链表（链表中可以存放不同类型的对象）不是类型安全的

### 8.7.4. 同质链表的类型安全性

- 同质链表：链表中只能存放相同类型的对象。
- 同样地，如何保证其类型安全性，即如何避免链表中插入对象与取出对象类型不一致的问题。
- 利用模板参数在编译时的类型检查！

1. 类模板`Islist`

   ```cpp
   template <class T>
   class Islist : private slist_base
   {
   public:
       void insert(T *a)
       {
           slist_base::insert(a);
       }
       T *get()
       {
           return (T *)slist_base::get() ;
       }
       // …
   };
   ```

2. 使用类模板

   ```cpp
   void f(const char *s)
   {
   	Islist<name> ilst;
   	ilst.insert(new name(s));
   	// …
   	name *p = ilst.get();
   	// …
   	delete p;
   }
   ```

3. 误用类模板

   ```cpp
   void f2(const char *s)
   {
       Islist<name> ilist;
       ilist.insert(new expr); // 编译时error，类型不匹配
       // …
   }
   ```

4. 小结

   - 
   - 在`Islist`实现中，实际的工作仍然由`slist_base`来完成，而类模板只是为了防止出现不安全的类型。
   - 直观地说，类模板`ilist`好像是一个过滤器，不安全的类型传给它时都会被检查出来

## 8.8. 容器与迭代器

### 8.8.1. 容器简述

什么是容器？

- 容器就是用来装对象的东西，容器本身也是一个对象。如：数组，链表，栈，队列，集合等。
- 他们之所以被称为“容器”就是因为他们的存在在很大程序上就是为了容纳一组别的东西。 

### 8.8.2. 通过模板构造容器

说明如何通过模板获得类型安全性检查 (参见 8.7)

### 8.8.3 迭代器

**迭代器**：

- 用于访问容器的东西的抽象概念，比如用于遍历链表结点的指针，用于访问数组元素的下标等等。
- 迭代器是一个对象，它在（其他对象的）容器上遍历，每次选择容器中的一个元素，但不需要提供对这个容器的实现的直接访问。
- 提供了一种访问元素的标准方法
- 通常与容器联合使用
- 在许多情况下，是一个“灵巧指针”；但比通常的指针运算更安全。

**例1** 

- 访问链表的迭代器类

  ```cpp
  class slist_base_iter
  {
      slink *ce;      //当前元素
      slist_base *cs; //当前链表
  public:
      slisk_base_iter(slist_base &s);
      slink *operator()();
  };
  ```

- 类型安全的迭代器类模板

  ```cpp
  template <class T>
  class Islist_iter : private slist_base_iter
  {
  public:
      Islist_iter(Islist<T> &s) : slist_base_iter(s) {}
      T *operator()()
      {
          return (T *)slist_base_iter::operator()();
      }
  };
  ```

### 8.8.4. 为什么使用迭代器

- 可以提供比下标更复杂的操作方法
- 隐藏了实现细节，比指针更易用。

## 8.9. C++标准模板库

STL（Standard Template Library）:标准模板库

- STL是一些“容器”的集合，这些“容器”有`list`,`vector`,`set`,`map`等，STL也是算法和其他一些组件的集合。这里的“容器”和算法的集合指的是世界上很多聪明人很多年的杰作。
- STL容器可以保存对象，内建对象和类对象。它们会安全地保存对象，并定义我们能够操作的这个对象的接口。
- STL算法是标准算法，我们可以把它们应用在那些容器中的对象上。这些算法都有很著名的执行特性。它们可以给对象排序，删除它们，给它们记数，比较，找出特殊的对象，把它们合并到另一个容器中，以及执行其他有用的操作。 
- STL iterator就象是容器中指向对象的指针。STL的算法使用iterator在容器上进行操作。Iterator设置算法的边界 ，容器的长度，和其他一些事情。举个例子，有些iterator仅让算法读元素，有一些让算法写元素，有一些则两者都行。Iterator也决定在容器中处理的方向。
  - `begin()`和`end()`
    你可以通过调用容器的成员函数`begin()`来得到一个指向一个容器起始位置的iterator。你可以调用一个容器的`end()`函数来得到过去的最后一个值（就是处理停在那的那个值）。 

---

