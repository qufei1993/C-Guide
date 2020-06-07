# C++ 面向对象编程之对象成员

在 C++ 面向对象中，除了基本类型用来做为对象成员之外，还可以使用类来声明一个对象成员。

譬如两个类：作者类（Author）、书籍类（Book），则可以定义 Author 类为 Book 类的对象成员。

## 实现 Author 类

创建一个 Author 类，声明作者的信息，本节只是测试说明，只实例化一个 name 即作者的名称。

```cpp
#include <string>
#include <iostream>
using namespace std;

class Author {
public:
  Author(string _name);
  ~Author();
  string getName();
private:
  string name;
};

Author::Author(string _name) {
  name = _name;
  cout <<"Constructor: Author()"<<endl;
};
Author::~Author() {
  cout <<"Destructor: ~Author()"<<endl;
};
string Author::getName() {
  return name;
}
```

## 实现 Book 类

在这个 Book 类中，定义了书籍的名称、编号，还通过上面定义的 Author 类声明了一个对象成员 author。

```cpp
class Book {
public:
  Book(string bookName, int bookId, string authorName);
  ~Book();
  void info();
private:
  string bookName;
  int bookId;
  Author author;
};

Book::Book(string bookName, int bookId, string authorName)
  :bookName(bookName), bookId(bookId), author(authorName)
{
  cout <<"Constructor: Book()"<<endl;
};
Book::~Book() {
  cout <<"Destructor: ~Book()"<<endl;
};
void Book::info() {
  cout <<"书籍名称："<<bookName<<", 书籍编号："<<bookId<<", 作者名称："<<author.getName()<<endl;
};
```

## 实例化我们的一个对象

实例化一个指针类型的对象 book 调用 info 打印输出信息。

```cpp
int main(int argc, const char * argv[]) {
  Book *book = new Book("深入浅出Nodejs", 25768396, "朴灵");
  book->info();
  
  delete book; // 先销毁 Book 还是 Author？
  book = NULL;
  return 0;
}
```

输出信息中，首先构造函数先实例化 Author 之后再是 Book，销毁时正好相反先销毁 Book 之后是 Author。

就好比装箱、拆箱一样，装的时候先装里面，最后才是外面封装；拆箱这个过程则相反，先从外面拆箱，之后才能从里面取。

```
Constructor: Author()
Constructor: Book()
书籍名称：深入浅出Nodejs, 书籍编号：25768396, 作者名称：朴灵
Destructor: ~Book()
Destructor: ~Author()
```