# C++ 中的 This 指针

C++ 中 this 指针无需用户定义，编译器将会自动生成，不同对象的 this 指针会指向不同的内存空间。

## 指针区分同名的数据成员

下面示例，运行之后会报错，因为编译器无法区分是要将参数赋予给成员变量，还是将成员变量赋予给参数。

```cpp
class Person {
public:
  Person(string name);
private:
  string name;
};

Person::Person(string name) {
  name = name; // 报错
  cout <<"Constructor: Person()"<<endl;
};
```

解决这个问题的办法，一方面是规范的命名规则，数据成员变量不要和函数的成员参数同名，另外一方面可以通过 this 指针来区分同名的数据成员。

```cpp
class Person {
public:
  Person(string name);
  ~Person();
  string getName();
private:
  string name;
};

Person::Person(string name) {
  this->name = name; // 改变之处
  cout <<"Constructor: Person()"<<endl;
};
```

## This 实现的链式调用

info() 方法返回的是一个 Person 类型的值（临时对象），里面的 this 表示的是该对象本身，所以我们后面可以通过 **->** 符号进行指针的形式来访问。 

```cpp
#include <string>
#include <iostream>
using namespace std;

class Person {
public:
  Person(string name);
  ~Person();
  Person* setName(string name);
  Person* info();
private:
  string name;
};

Person::Person(string name) {
  this->name = name;
  cout <<"Constructor: Person()"<<endl;
};
Person::~Person() {
  cout <<"Destructor: ~Person()"<<endl;
};
Person* Person::setName(string name) {
  //cout <<sizeof(this)<<endl;
  this->name = name;
  return this;
};
Person* Person::info() {
  cout << this->name <<endl;
  return this;
}
 
int main(int argc, const char * argv[]) {
  Person p1("Tom");
  p1.info()->setName("Jack")->info();
  // Tom
  // Jack
  return 0;
}
```