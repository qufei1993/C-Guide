# C++ 面向对象编程之类的声明、定义、实例化入门篇

## 定一个简单的类

C++ 中提供了三个类的访问修饰符 private、protected、public。

```cpp
class Person {
public:
  string name;
  int age;
  
  void info(string _name, int _age) {
    cout <<"My name is "<<_name<<" Age "<<age<<endl;
  };
}
```

## 类的实例化

C++ 中我们实例化一个类的对象，**既可以在栈区实例化对象**，**也可以在堆区实例化对象**。

### 从栈中实例一个类

栈中申请的实例，程序运行结束会帮我们自动释放掉。

实例化出来的对象成员使用 **.** 符号访问。

```cpp
int main(int argc, const char * argv[]) {
  Person p1;
  p1.name = "Jack";
  p1.age = 20;
  p1.info(p1.name, p1.age);
  
  Person p2[2];
  p2[0].name = "Tom";
  p2[0].age = 18;
  p2[0].info(p2[0].name, p2[0].age);
  return 0;
}
```

### 从堆中实例一个类

定义一个指针变量通过 new 运算符从堆中申请一块内存，最后一定要通过 delete 关键字将申请的内存释放掉。

实例化出来的对象成员使用 **->** 符号访问。

```cpp
int main(int argc, const char * argv[]) {
  Person *p1 = new Person();
  
  if (p1 == NULL) {
    cout<<""<<endl;
    return 0;
  }
  p1->name = "Jack";
  p1->age = 20;
  p1->info(p1->name, p1->age); // My name is Jack Age 20
  
  delete p1;
  p1 = NULL;
  return 0;
}
```

## 类外定义

在 TypeScript、Java 等编程中，我们都是在同一个类的内部进行声明和定义，但是在 **C++ 中成员函数的函数体可以写在类的外部**，既可以是**同文件类外定义**、也可以是**不同文件类外定义**。

### 同文件类外定义

同文件类外定义是类的声明与类的定义进行分离。

类的定义要使用 :: 符号，前面加上类名，例如：**Person::**

```cpp
class Person {
public:
  string name;
  int age;
  
  void info(string _name, int _age);
};

void Person::info(string _name, int _age) {
  cout <<"My name is "<<_name<<" Age "<<age<<endl;
};
```

### 不同文件类外定义

可以将类的声明与类的定义分成不同的文件编写。

**Person.h**

新建头文件 Person.h 进行类的声明，具体实现不再这里。

```cpp
#include <string>
using namespace std;

class Person {
public:
  string name;
  int age;
  
  void info(string _name, int _age);
};
```

**Person.cpp**

新建 Person.cpp 文件，进行类外定义，例如：上面定义的 void info() 方法就在这里实现。

```cpp
#include "Person.h"
#include <string>
#include <iostream>
using namespace std;

void Person::info(string _name, int _age) {
  cout <<"My name is "<<_name<<" Age "<<_age<<endl;
};
```

**main.cpp**

main.cpp 为我们的入口文件，进行调用。

注意编译时要引入**头文件**。

```cpp
#include "Person.h"

int main(int argc, const char * argv[]) {
  Person p1;
  p1.name = "Jack";
  p1.age = 20;
  p1.info(p1.name, p1.age); // My name is Jack Age 20
  
  return 0;
}
```

## 对象的生命周期

```
申请内存 ---------> 初始化列表 ---------> 构造函数
  ｜                                       ｜
  ｜                                       ｜
  ｜                                       ｜
释放内存 <-------- 析构函数 <---------参与运算 + 
```