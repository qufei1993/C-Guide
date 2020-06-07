# C++ 面向对象编程之拷贝构造函数、深拷贝

拷贝构造函数的参数是确定的，不能重载，当没有自定义拷贝构造函数时系统会自动生成一个，拷贝构造函数没有返回值。

## 声明格式

类名: (const 类名& 变量名)

## 浅拷贝

浅拷贝就是将值直接拷贝过去，指向的还是同一块内存地址。

```cpp
#include <string>
#include <iostream>
using namespace std;

class Person {
public:
  Person(string _name);
  Person(const Person& arr);
  ~Person();
  string getName();
  void setName(string _name);
  void info();
private:
  string name;
};

Person::Person(string _name) :name(_name)
{
  cout <<"Constructor: Person()"<<endl;
};
Person::Person(const Person& args) {
  name = args.name;
  cout <<"Constructor: Person(const Person& args)"<<endl;
};
Person::~Person() {
  cout <<"Destructor: ~Person()"<<endl;
};
void Person::setName(string _name) {
  name = _name;
};
void Person::info() {
  cout <<name<<endl;
};

int main(int argc, const char * argv[]) {
  Person p1("Tom");
  // Person p2 = p1; 等价于 Person p2(p1);
  Person p2(p1);
  
  p1.info();
  p2.info();
  return 0;
}
```

运行结果如下所示，执行 Person p2(p1) 时会触发拷贝构造函数。

```
Constructor: Person()
Constructor: Person(const Person& args)
Tom
Tom
Destructor: ~Person()
Destructor: ~Person()
```

大多数情况，使用浅拷贝也可以很好的工作，但是存在一些动态的对象成员，可能就会有些问题了。

## 前拷贝某些场景可能存在的问题

基于上面的例子进行修改

**修改类的声明**

类 Person 声明增加指针类型 hobby，表示一个人的爱好。

```cpp
class Person {
public:
  ...
private:
  ...
  int *hobby;
};
```

**修改类的定义**

构造函数里创建 3 个元素的存储空间，拷贝构造函数做赋值操作。

最后定义 printPointAddr 方法打印指针变量 hobby 的地址。

```cpp
Person::Person(string _name) :name(_name)
{
  hobby = new int[3];
  cout <<"Constructor: Person()"<<endl;
};
Person::Person(const Person& args) {
  hobby = args.hobby;
  name = args.name;
  cout <<"Constructor: Person(const Person& args)"<<endl;
};
Person::~Person() {
  delete []hobby;
  hobby = NULL;
  cout <<"Destructor: ~Person()"<<endl;
};
void Person::printPointAddr() {
  cout <<"point *hobby addr: "<<hobby<< endl;
}
```

**修改 main 函数**

```cpp
int main(int argc, const char * argv[]) {
  Person p1("Tom");
  Person p2(p1);
  p1.printPointAddr();
  p2.printPointAddr();
  return 0;
}
```

**运行测试**

可以看到两个对象 p1、p2 指向的内存空间地址是相同的，正因如此，析构函数销毁时对同一个空间做了两次操作，导致下面报错。

```
Constructor: Person()
Constructor: Person(const Person& args)
point *hobby addr: 0x100608a20
point *hobby addr: 0x100608a20
Destructor: ~Person()
CPP-Test(30857,0x1000d7dc0) malloc: *** error for object 0x100608a20: pointer being freed was not allocated
CPP-Test(30857,0x1000d7dc0) malloc: *** set a breakpoint in malloc_error_break to debug
```

## 深拷贝

深拷贝不再是简单的赋值就可以了，需要重新分配内存空间，进行一个一个拷贝，也就解决上了上面最后遇到的问题。

基于上面的代码，仅修改拷贝构造函数即可：
* 先申请一块内存
* 将传入进来的对象挨个拷贝到申请的内存中

```cpp
Person::Person(const Person& args) {
  hobby = new int[3]; // 先申请一块内存
  
  for (int i=0; i<3; i++) {
    hobby[i] = args.hobby[i]; // 将传入进来的对象挨个拷贝
  }
  
  name = args.name;
  cout <<"Constructor: Person(const Person& args)"<<endl;
};
```

再次运行查看，指针变量 hobby 的内存地址已经不同了。

```
Constructor: Person()
Constructor: Person(const Person& args)
point *hobby addr: 0x10076c670
point *hobby addr: 0x100761e10
Destructor: ~Person()
Destructor: ~Person()
Program ended with exit code: 0
```