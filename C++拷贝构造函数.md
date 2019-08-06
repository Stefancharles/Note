# 什么是拷贝构造函数
从名称上来看，拷贝构造函数是属于构造函数的一种。关于
[构造函数](https://github.com/Stefancharles/Note/blob/master/C%2B%2B%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0.md)
的介绍在这里。
首先来看一个例子，对于普通类型的对象之间的复制：
```c++
int a = 66;
int b;
b=a;
```
可以这么看，int a = 66相当于实例化了一个对象是a，值为66，又实例化了一个对象b，并把a的值复制给b。这种复制是简单的。但是类的对象比普通对象更为复杂，有很多其他的结构在其中，存在各种成员变量。
```c++
#include <iostream>
using namespace std;

class CExample {
private:
    　int a;
public:
      //构造函数
    　CExample(int b)
    　{ a = b;}

      //一般函数
    　void Show ()
    　{
        cout<<a<<endl;
      }
};

int main()
{
    　CExample A(100);
    　CExample B = A; //注意这里的对象初始化要调用拷贝构造函数，而非赋值
    　 B.Show ();
    　return 0;
}
```
从以上代码可以看出，系统为对象 B 分配内存并完成了与对象 A 的复制过程。就类对象而言，相同类型的类对象是通过**拷贝构造函数**来完成整个复制过程的。
```c++
#include <iostream>
using namespace std;

class CExample {
private:
    int a;
public:
    //构造函数
    CExample(int b)
    { a = b;}
    
    //拷贝构造函数
    CExample(const CExample& C)
    {
        a = C.a;
    }

    //一般函数
    void Show ()
    {
        cout<<a<<endl;
    }
};

int main()
{
    CExample A(100);
    CExample B = A; // CExample B(A); 也是一样的
     B.Show ();
    return 0;
} 
```
CExample(const CExample& C)　就是我们自定义的拷贝构造函数。可见，拷贝构造函数是一种特殊的构造函数，函数的名称必须和类名称一致，它必须的一个参数是本类型的一个引用变量。

# 什么时候需要拷贝构造函数
## 1.对象以值传递的方式传入函数参数
```c++
class CExample 
{
private:
 int a;

public:
 //构造函数
 CExample(int b)
 { 
  a = b;
  cout<<"creat: "<<a<<endl;
 }

 //拷贝构造
 CExample(const CExample& C)
 {
  a = C.a;
  cout<<"copy"<<endl;
 }
 
 //析构函数
 ~CExample()
 {
  cout<< "delete: "<<a<<endl;
 }

void Show ()
 { 
     cout<<a<<endl;
 }

};

//全局函数，传入的是对象
void g_Fun(CExample C)
{
 cout<<"test"<<endl;
}

int main()
{
 CExample test(1);
 //传入对象
 g_Fun(test);

 return 0;
}
```
调用g_Fun()时，会产生以下几个重要步骤：
* (1).test对象传入形参时，会先会产生一个临时变量，就叫 C 吧。
* (2).然后调用拷贝构造函数把test的值给C。 整个这两个步骤有点像：CExample C(test);
* (3).等g_Fun()执行完后, 析构掉 C 对象。

从下图可以看到，拷贝的时候，生成一个C的对象
![1234.png](https://i.loli.net/2019/08/04/awXDScWJbsn561q.png)

对于析构来说，先析构临时的C对象，再析构test对象。

## 2.对象以值传递的方式从函数返回
```c++
class CExample 
{
private:
 int a;

public:
 //构造函数
 CExample(int b)
 { 
  a = b;
 }

 //拷贝构造
 CExample(const CExample& C)
 {
  a = C.a;
  cout<<"copy"<<endl;
 }

void Show ()
 {
  cout<<a<<endl;
 }
};

//全局函数
CExample g_Fun()
{
 CExample temp(0);
 return temp;
}

int main()
{
 g_Fun();
 return 0;
}
```
当g_Fun()函数执行到return时，会产生以下几个重要步骤：
* (1). 先会产生一个临时对象名为C。
* (2). 然后调用拷贝构造函数把temp的值给C。整个这两个步骤有点像：CExample C(temp);
* (3). 在函数执行到最后先析构temp局部变量。
* (4). 等g_Fun()执行完后再析构掉对象C。

## 3.某对象需要另外一个对象进行初始化
```c++
CExample A(100);
CExample B = A; // CExample B(A); 
```
# 浅拷贝和深拷贝
## 1.默认拷贝构造函数
很多时候我们没有自己写拷贝构造函数，传递对象给某个函数作为参数或者从某个函数返回对象都可以正常进行。这是因为编译器会自动生成一个“默认拷贝构造函数”。但是这个拷贝构造函数很简单，仅仅把老对象的数据成员的值对新对象的数据成员一一赋值。

## 2.浅拷贝
浅拷贝，指的是在对象复制时，只对对象中的数据成员进行简单的赋值，默认拷贝构造函数执行的也是浅拷贝的一种。大多情况下“浅拷贝”已经能很好地工作了，但是一旦对象存在了动态成员，那么浅拷贝就会出问题。

## 3.深拷贝
当我们的成员变量是动态分配的堆内存的时候，这个时候C++默认的为我们写的拷贝构造函数把成员变量的值进行了复制，那么副本也会指向原始对象所分配的堆内存，当我们在析构函数中释放堆内存的时候，就会发生两次析构现象（原始对象释放堆内存 + 副本对象释放堆内存）因此造成程序崩溃。深拷贝则不让指针指向同一地址，重新开辟一段内存给新的对象的指针数据成员。

## 4.技巧：防止默认拷贝函数