# C++ 内存管理

C++ 中使用关键字 new 申请内存、delete 释放内存，内存申请不是完全会成功的，我们还需要判断是否成功，做异常处理，同样释放内存时也要设空指针。

```cpp
int main(int argc, const char * argv[]) {
  int *p = new int; // 内存申请
  if (p == NULL) {
    cout<<"内存申请失败"<<endl;
    return 0;
  };
  *p = 1;
  cout<<*p<<endl;
  delete p; // 内存释放
  p = NULL; // 将 p 的指针置为空
  return 0;
}
```

块级内存释放时一定要加上符号 []，否则只能释放指向的第一个内存。 

```cpp
int main(int argc, const char * argv[]) {
  int *p = new int[1000];
  if (p == NULL) {
    cout<<"内存申请失败"<<endl;
    return 0;
  };
  p[0] = 1;
  p[1] = 2;
  cout<<p[0]<<"----"<<p[1]<<endl;
  delete []p;
  p = NULL;
  return 0;
}
```
