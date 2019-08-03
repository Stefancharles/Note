# c++构造函数
类的构造函数是类的一种特殊成员函数，它会在每次创建类的新对象时自动执行。
* 构造函数的函数名称与类名相同
* 不会有返回类型，也不会返回void
* 构造函数可以给一些类的成员变量设置初始值。
* 如果我们没有定义构造函数，系统会为我们自动定义一个无参的默认构造函数的，它不对成员属性做任何操作，如果我们自己定义了构造函数，系统就不会为我们创建默认构造函数了。
* 构造函数没有返回类型，但是可以有参数。也就是说构造函数可以重载。


```c
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line();  // 这是构造函数
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(void)
{
    cout << "Object is being created" << endl;
}
 
void Line::setLength( double len )
{
    length = len;
}
 
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line;
 
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```
# 带参的构造函数
默认的构造函数没有任何参数，但如果需要，构造函数也可以带有参数。这样在创建对象时就会给对象赋初始值。
```c
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line(double len);  // 这是构造函数
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line( double len)
{
    cout << "Object is being created, length = " << len << endl;
    length = len;
}
 
void Line::setLength( double len )
{
    length = len;
}
 
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line(10.0);
 
   // 获取默认设置的长度
   cout << "Length of line : " << line.getLength() <<endl;
   // 再次设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```
# 构造函数的重载
在一个类中可以有多个构造函数，他们构成了函数的重载。

# 使用初始化列表来初始化字段
```c
Line::Line( double len): length(len)
{
    cout << "Object is being created,length = " << len << endl;
}
```
上面的语法等同于
```c
Line::Line( double len)
{
    length = len;
    cout << "Object is being created, length = " << len << endl;
}
```
假设有一个类 C，具有多个字段 X、Y、Z 等需要进行初始化，同理地可以使用上面的语法，只需要在不同的字段使用逗号进行分隔.
```c
C::C( double a, double b, double c): X(a), Y(b), Z(c)
{
  ....
}
```
# C++析构函数
它的作用与构造函数相反，一般是执行对象的清理工作，当对象的生命周期结束的时候，会自动的调用。析构函数的作用并不是删除对象，在对象撤销它所占用的内存之前，做一些清理的工作。清理之后，这部分内存就可以被系统回收再利用了。在设计这个类的时候，系统也会默认的提供一个析构函数。在对象的生命周期结束的时候，程序就会自动执行析构函数来完成这些工作。同构造函数，用户自己定义，系统自动调用。
* 析构函数没有返回类型，没有参数
* 没有参数，所以不能重载，也就是说一个类只有一个析构函数
* 析构函数除了释放工作，还可以做一些用户希望它做的一些工作，比如输出一些信息。