# C++ 面向对象编程之类的构造函数和析构函数

## 构造函数

C++ 中的**构造函数与类名同名**，且**构造函数是可以重载**的，另外构造函数是没有返回值的。

**初始化列表**会先于构造函数执行，只能用于构造函数。

### 修改 Person.h

对 Person 声明了三次构造函数，每次的参数类型或数量都是不同的，也实现了构造函数的重载。

```cpp
#include <string>
#include <iostream>
using namespace std;

class Person {
public:
  Person();
  Person(string name);
  Person(string name, int age);
  void info();
private:
  string p_name;
  int p_age;
};
```

### 修改 Person.cpp

对上面声明的构造函数进行定义，最后更新 Person 类里的 info 方法

```cpp
#include "Person.h"
#include <string>
#include <iostream>
using namespace std;

Person::Person(){ // 声明构造函数无参数
  p_name = "NULL";
  p_age = 0;
  cout<<"Person()"<<endl;
};
Person::Person(string name) // 声明只有一个参数的构造函数
{
  p_name = name;
  p_age = 0;
  cout<<"Person(string name)"<<endl;
};

// 使用初始化列表声明构造函数
Person::Person(string name, int age):p_name(name), p_age(age)
{
  cout<<"Person(string name, int age)"<<endl;
};

void Person::info() {
  cout <<"My name is "<<this->p_name<<" Age "<<this->p_age<<endl;
};
```

### 修改主函数

```cpp
#include "Person.h"

int main(int argc, const char * argv[]) {
  Person p1; // Person()
  p1.info(); // My name is NULL Age 0
   
  Person p2("Tom"); // Person(string name)
  p2.info(); // My name is Tom Age 0
  
  Person p3("Jack", 20); // Person(string name, int age)
  p3.info(); // My name is Jack Age 20
  
  return 0;
}
```

## 析构函数

**析构函数在对象销毁时自动被调用**，用于回收系统资源，没有参数、没有返回值且不能像构造函数一样进行重载。

如果没有自定义析构函数，系统也会自动创建。

### 析构函数声明

修改 Person.h 文件，使用 **～** 符号声明析构函数。

```cpp
class Person {
public:
  ...
  ~Person();
```

### 析构函数定义

修改 Person.h 文件，为了看到效果打印下析构函数名称。

```cpp
Person::~Person() {
  cout<<"~Person()"<<endl;
};
```