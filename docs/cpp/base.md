# C++ 基础入门篇

C 语言是 C++ 的子集，C++ 除了支持 C 语言的面向过程编程之外，还支持面向对象编程是高级计算机语言。

## 与 C 语言的不同

### 1. 布尔值声明

C++ 中新增加 bool 布尔值类型，做一些判断时方便多了，下面分别给出 C 与 C++ 的示例。

```c
// C 示例：
int flag = 1;
if (flag == 1) console.log('true');
```

```c++
// C++ 示例：
bool flag = 0;
if (flag)  console.log('true');
```

### 2. 输入输出方式

**cout 输出：**

endl 结束换行符，可选，如果 endl 不写，最后的 “>>” 符号也可以不用写。

“<<” 匹配输出

```C++
int a = 0;
int b = 1;
cout <<a<< endl;
cout <<a;
cout <<"a+b="<<a+b<<endl;
```

**cin 输入：**

“>>” 匹配输入

```c++
int num = 0;
cout<<"请输入一个整数，按回车键进行八进制、十进制转换、十六进制："<<endl;
cin>>num;
cout<<"八进制："<<oct<<num<<endl; // 八进制：12
cout<<"十进制："<<dec<<num<<endl; // 十进制：10
cout<<"十六进制："<<hex<<num<<endl; // 十六进制：a
```

## 命名空间

命名空间可以用来区分相同名字的变量、函数，通过 namespace 来定义。

例如，有一个同事叫 Jack，但是隔壁另外一个同事也叫 Jack，此时如果你在楼下喊一声 Jack 你的快递到了请到楼下取，就会造成两个人同事下去，他们也不确定你叫的是哪个，所以此时就需要通过命名空间（namespace）来标识。

```c++
#include <iostream>
using namespace std;

namespace CompanyA {
  string name = "Jack";
  void info() {
    cout <<"This is Jack from Company A."<<endl;
  }
}

namespace CompanyB {
  string name = "Jack";
  void info() {
    cout <<"This is Jack from Company B."<<endl;
  }
}

int main(int argc, const char * argv[]) {
  CompanyA::info(); // This is Jack from Company A.
  CompanyB::info(); // This is Jack from Company B.
}
```

## C++ 中的引用

在计算机编程中引用就是指变量的一个别名，引用必须初始化且初始化完成之后会和它的初始化对象绑定在一起，也不能再绑定到另外一个对象。

### 基本数据类型引用

下例中变量 b 是变量 a 的一个别名引用，那么我们修改 b 值时也相当于在修改变量 a 的值。 

```c++
int main(int argc, const char * argv[]) {
  int a = 1;
  int &b = a; // 引用必须初始化
  b = 2;
  cout<<a<<endl; // 2

  // 错误的写法
  // int a = 1;
  // int &b;
}
```

### 结构体类型引用

```c++
typedef struct {
  string name;
} Person;

int main(int argc, const char * argv[]) {
  Person p1;
  Person &p2 = p1;
  p2.name = "MayJun";
  
  cout<<p1.name<<endl; // MayJun
}
```

### 指针类型的引用

```cpp
int main(int argc, const char * argv[]) {
  int a = 1;
  int *b = &a;
  int *&c = b;
  *c = 3;
  cout<<a<<endl; // 3
  *b = 2;
  cout<<a<<endl; // 2
}
```

### 引用做为函数参数

```cpp
void swap(int &a, int &b) {
  int c = 0;
  c = a;
  a = b;
  b = c;
}

int main(int argc, const char * argv[]) {
  int a = 1;
  int b = 2;
  swap(a, b);
  cout<<a<<"-----"<<b<<endl; // 2-----1
}
```

### 指针与引用区别？

指针需要额外的存储空间来存储变量的地址，不需要初始化，过程中是可以更改的。

引用是原变量的别名共用内存空间，必须初始化且不能被再次修改，相对指针更加安全。

## Const 声明

编程中如果可以确定某个值是不变的，就应该明确使用 const

### 基本类型 const 声明

const 在数据类型（int）左边或者是右边都是等价的。

```cpp
const int a = 1; // 等价于 int const a = 1;

// 下面是错误的
a = 2; // Cannot assign to variable 'a' with const-qualified type 'const int'
```

### 指针类型 const 声明

**cosnt 声明在 * 左侧**

const 声明在 **\* 左侧**表示指针本身是变量，指针所指向的数据是常量

```cpp
int main(int argc, const char * argv[]) {
  int a = 1;
  int const *b = &a;
  int c = 3;
  b = &c; // 正确

  cout<<*b<<endl;
}
```

**cosnt 声明在 * 右侧**

const 声明在 **\* 右侧** 表示指针本身是常量。

```cpp
int main(int argc, const char * argv[]) {
  int a = 1;
  int * const b = &a;
  int c = 3;
  b = &c; // 错误，Cannot assign to variable 'b' with const-qualified type 'int *const'
  //b = 2;
  cout<<*b<<endl;
}
```

**const 在 * 左右都有声明**

const 在 * 左右都有声明表示指针和指针所指向的数据都不能修改。

```cpp
int main(int argc, const char * argv[]) {
  int a = 1;
  const int *const b = &a;
}
```

### 引用类型 const 声明

```cpp
int main(int argc, const char * argv[]) {
  int a = 1;
  const int &b = a;
  a = 2; // 正确
  // b = 2; // 错误，Cannot assign to variable 'b' with const-qualified type 'const int &'

  cout<<b<<endl; // 2
}
```

### 一道 const 声明的题

当 const 已经修饰一个变量时，在声明一个指针类型的变量去指向这个 const 声明的变量是危险的，因为指针会改变变量的值，因此下述写法是错误的。

```cpp
int const a = 1;
int *b = &a; // 错误
```

通过以下方式声明，保证 *b 的值不能变。

```cpp
int const a = 1;
int const *b = &a;
```

## 函数重载

C++ 允许多个函数名相同，只要参数数量或类型不一样即可，通常也称为函数重载。

例如，我们要实现一个函数 add 分别实现两个整数相加或两个 double 类型的数相加，如果是在 C 语言中我们需要为函数取两个不同的名字，在 C++ 中我们则可以通过函数重载来实现，看下面示例：

```cpp
int add (int a, int b) {
  return  a + b;
};

double add (double a, double b) {
  return  a + b;
};

int main(int argc, const char * argv[]) {
  cout<<add(1, 2)<<endl; // 3
  cout<<add(1.1, 2.1)<<endl; // 3.2
}
```