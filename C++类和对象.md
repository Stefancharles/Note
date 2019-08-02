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
    Box box2;
    box1.length=1;
    box1.breadth=2;
    box1.heighth=3;

    return 0;
}


```