# C++类的静态成员
我们可以使用 static 关键字来把类成员定义为静态的。当我们声明类的成员为静态时，这意味着无论创建多少个类的对象，静态成员都只有一个副本。

静态成员在类的所有对象中是共享的。如果不存在其他的初始化语句，在创建第一个对象时，所有的静态数据都会被初始化为零。我们**不能把静态成员的初始化放置在类的定义中**，但是可以在类的外部通过使用范围解析运算符 :: 来重新声明静态变量从而对它进行初始化。

例子：

```c++
#include <iostream>
 
using namespace std;
 
class Box
{
   public:
      static int objectCount;
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
         // 每次创建对象时增加 1
         objectCount++;
      }
      double Volume()
      {
         return length * breadth * height;
      }
   private:
      double length;     // 长度
      double breadth;    // 宽度
      double height;     // 高度
};
 
// 初始化类 Box 的静态成员
int Box::objectCount = 0;
 
int main(void)
{
   Box Box1(3.3, 1.2, 1.5);    // 声明 box1
   Box Box2(8.5, 6.0, 2.0);    // 声明 box2
 
   // 输出对象的总数
   cout << "Total objects: " << Box::objectCount << endl;
 
   return 0;
}
```

静态成员属于类作用域，但不属于类对象，它的生命周期和普通的静态变量一样，程序运行时进行分配内存和初始化，程序结束时则被释放。所以不能在类的构造函数中进行初始化。

# C++静态成员函数
如果把函数成员声明为静态的，就可以把函数与类的任何特定对象独立开来。静态成员函数即使在类对象不存在的情况下也能被调用，静态函数只要使用类名加范围解析运算符 :: 就可以访问。

静态成员函数只能访问静态成员数据、其他静态成员函数和类外部的其他函数。

静态成员函数有一个类范围，他们不能访问类的 this 指针。您可以使用静态成员函数来判断类的某些对象是否已被创建。

### 静态成员函数与普通成员函数的区别：
* 静态成员函数没有 this 指针，只能访问静态成员（包括静态成员变量和静态成员函数）。
* 普通成员函数有 this 指针，可以访问类中的任意成员。

例子：
```c
#include <iostream>
 
using namespace std;
 
class Box
{
   public:
      static int objectCount;
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
         // 每次创建对象时增加 1
         objectCount++;
      }
      double Volume()
      {
         return length * breadth * height;
      }
      static int getCount()
      {
         return objectCount;
      }
   private:
      double length;     // 长度
      double breadth;    // 宽度
      double height;     // 高度
};
 
// 初始化类 Box 的静态成员
int Box::objectCount = 0;
 
int main(void)
{
  
   // 在创建对象之前输出对象的总数
   cout << "Inital Stage Count: " << Box::getCount() << endl;
 
   Box Box1(3.3, 1.2, 1.5);    // 声明 box1
   Box Box2(8.5, 6.0, 2.0);    // 声明 box2
 
   // 在创建对象之后输出对象的总数
   cout << "Final Stage Count: " << Box::getCount() << endl;
 
   return 0;
}
```
# static成员的用处
* static成员的名字是在类的作用域中，因此可以避免与其它类成员或全局对象名字冲突。
* 可以实施封装，static成员可以是私有的，而全局对象不可以。
* 阅读程序容易看出static成员与某个类相关联，这种可见性可以清晰地反映程序员的意图。

# static成员函数特点
* 因为static成员函数没有this指针，所以静态成员函数不可以访问非静态成员。
* 非静态成员函数可以访问静态成员。
* 静态数据成员与类的大小无关，因为静态成员只是作用在类的范围而已。