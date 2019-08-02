# C++类和对象
类是C++的核心特性，也被称为*用户定义的类型*。
类用于指定对象类型，包括数据的表示方法和处理数据的方法。类中的数据和方法称为类的成员。

# C++类定义
定义一个类，本质上是定义一个数据类型的蓝图。定义了类的对象包括了什么，和可以在这上面进行哪些操作。
定义以关键字*class*开头。然后是类的名称。类的主体部分用一对花括号括起来。在主体部分后必须接分号或者声明列表。

# 举例
```c++
#include "iostream"
using namespace std;

class Box
{
    public:
    double length;
    double breadth;
    double heigth;

};

int main()
{
    Box box1;
    box1.length=1;
    box1.breadth=2;
    box1.heighth=3;
    return 0;
}
```

# 成员函数
使用类的成员函数访问类的成员
* 一种方法是直接在类中进行定义
在类中定义的成员函数把函数声明为*内联的*，虽然这没有使用*inline*标识符。
```c++
#include  "iostream"
using namespace std;
class Box
{
    public:
    double length;
    double breadth;
    double height;
    double getVolume(void)
    {
        return length*breadth*height;
    }
};
```
* 一种方法是在类的外面单独使用 **范围解析运算符::** 
```c++
#include  "iostream"
using namespace std;
class Box
{
    public:
    double length;
    double breadth;
    double height;
    double getVolume(void)；
};

double Box::getVolume(void)
{
     return length*breadth*height;
}
```

# C++类访问修饰符
数据封装是面向对象编程的一个特性，它阻止函数直接访问类的内部成员。
* 关键字：
```c++
public,private,protected
```
## private
如果没有给定修饰符，则默认修饰符是private.
私有成员变量和函数在外部是不可访问的。只有类和友元函数可以访问私有成员。

## protected
保护成员变量和函数和私有成员的比较类似。不同的地方是保护成员在派生类（子类）是可以访问的。
```c++
#include <iostream>
using namespace std;
 
class Box
{
   protected:
      double width;
};
 
class SmallBox:Box // SmallBox 是派生类
{
   public:
      void setSmallWidth( double wid );
      double getSmallWidth( void );
};
 
// 子类的成员函数
double SmallBox::getSmallWidth(void)
{
    return width ;
}
 
void SmallBox::setSmallWidth( double wid )
{
    width = wid;
}
 
// 程序的主函数
int main( )
{
   SmallBox box;
 
   // 使用成员函数设置宽度
   box.setSmallWidth(5.0);
   cout << "Width of box : "<< box.getSmallWidth() << endl;
 
   return 0;
}
```
父类Box派生一个子类SmallBox，width可以被派生出的子类的任意成员函数访问。
# 继承的特点
有三种继承方式，他们相应的改变了基类成员的访问属性。

|继承方式|基类public成员|基类protected成员|基类private成员|

|:---:|:---:|:---:|:---:|
|public继承|public|protected|private|
|protected继承|protected|protected|private|
|privae继承|private|private|private|

总之：
* private成员只能被本类和友元访问，派生类不能访问它；
* protected成员可以被派生类访问。
