# C++ 面向对象编程之对象数组

对象数组的实例化分两种形式：一是实例化的对象数组在栈区，会自动进行内存回收；另外一种是实例化的对象数组在堆区，需要手动进行内存回收。

## 实现一个 Person 类

使用同文件类外定义的方式实现一个 Person 类用于下面测试。

```cpp
#include <string>
#include <iostream>
using namespace std;

class Person {
public:
  Person();
  ~Person();
  string getName();
  void setName(string _name);
private:
  string p_name;
};

Person::Person() {
  cout <<"Constructor: Person()"<<endl;
};
Person::~Person() {
  cout <<"Destructor: ~Person()"<<endl;
};
void Person::setName(string _name) {
  p_name = _name;
};
string Person::getName() {
  return p_name;
}
```

## 栈区实例化的对象数组

实例化一个对象 p 会保存于栈中

```cpp
int main(int argc, const char * argv[]) {
  Person p[3];
  p[0].setName("Tom");
  p[1].setName("Jack");
  p[2].setName("Rose");
  
  for (int i=0; i < 3; i++) {
    cout <<p[i].getName()<<endl;
  }
  
  return 0;
}
```

运行之后可以看到首先构造函数初始化三次，待程序执行完毕，析构函数会被自动调用。

```
Constructor: Person()
Constructor: Person()
Constructor: Person()
Tom
Jack
Rose
Destructor: ~Person()
Destructor: ~Person()
Destructor: ~Person()
```

## 堆区实例化的对象数组

创建一个有 3 个元素的指针类型对象数组 p。

每次赋值之后指针要向后移动，下例中就是去做 p++ 操作。

在 for 循环输出结果时，同样指针也要向前移动，做 p-- 操作。

```cpp
int main(int argc, const char * argv[]) {
  Person *p = new Person[3];
  //p->setName("Tom"); 等价于 p[0].setName("Tom");
  p[0].setName("Tom");
  
  p++;
  p->setName("Jack");

  p++;
  p->setName("Rose");
  
  for (int i=0; i < 3; i++) {
    cout <<p->getName()<<endl;
    //cout <<p[0].getName()<<endl; 等价于 cout <<p->getName()<<endl;
    p--;
  }
  
  return 0;
}
```

输出结果，可以看到析构函数并没有自动被回收，下面我们在进行修改。

```
Constructor: Person()
Constructor: Person()
Constructor: Person()
Rose
Jack
Tom
```

### 为堆中的对象数组实例做内存释放

修改 main 函数在最后对实例化的对象数组做内存释放。

需要注意的是，因为 for 中最后一次 p-- 之后相当于 p[-1] 这样在去做 delete 内存释放是会报错的，所以先执行了一步 p++。

delete []p 之后，对象数组中的元素是被删除了，但是 P 指针还在，要再设置 p = NULL;

```cpp
int main(int argc, const char * argv[]) {
  ...
  
  for (int i=0; i < 3; i++) {
    cout <<p->getName()<<endl;
    //cout <<p[0].getName()<<endl;
    p--;
  }
  
  // 改变之处开始
  p++;
  delete []p;
  p = NULL;
  // 改变之处结束

  return 0;
}
```

运行上面我们修改之后的代码，可以看到最后执行了析构函数。

```
Constructor: Person()
Constructor: Person()
Constructor: Person()
Rose
Jack
Tom
Destructor: ~Person()
Destructor: ~Person()
Destructor: ~Person()
```