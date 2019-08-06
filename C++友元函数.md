# C++友元函数
类的友元函数定义在类外部，但是有权访问类的私有成员和保护成员。
友元分为友元函数和友元类。在友元类中，整个类和成员都是友元。

# 关键字friend
```c++
class Box
{
   double width;
public:
   double length;
   friend void printWidth( Box box );
   void setWidth( double wid );
};
```
# 特性
## 1.优点
提高程序运行效率
## 2. 缺点
破坏了类的封装性和数据的透明性

# 友元函数声明和定义
在类的声明的任何区域声明，定义在类的外部。

# 友元类的声明和定义
友元类的声明在该类的声明中，而实现在该类外。

```c++
class B
{
public:
	B(int _b):b(_b){};
 
	friend class C;//声明友元类C
 
private:
	int b;
};

  //实现友元类C
class C   
{
public:
	int getB_b(B _classB){
		return _classB.b;//访问友元类B的私有成员
	};
};
 
 
int _tmain(int argc, _TCHAR* argv[])
{
	B _classB(3);
	C _classC;//定义友元类实例
	std::cout<<_classC.getB_b(_classB);
	return 0;
}
```