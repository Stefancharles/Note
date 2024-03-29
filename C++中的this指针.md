# 概述
在 C++ 中，每一个对象都能通过 this 指针来访问自己的地址。this 指针是所有成员函数的隐含参数。因此，在成员函数内部，它可以用来指向调用对象。

友元函数没有 this 指针，因为友元不是类的成员。只有成员函数才有 this 指针。

# 经典比喻
当你进入一个房子后，你可以看见桌子、椅子、地板等，但是房子你是看不到全貌了。
对于一个类的实例来说，
你可以看到它的成员函数、成员变量，
但是实例本身呢？
this是一个指针，它时时刻刻指向你这个实例本身。

# this指针的用处
一个对象的this指针并不是对象本身的一部分，不会影响sizeof(对象)的结果。this作用域是在类内部，当在类的非静态成员函数中访问类的非静态成员的时候，编译器会自动将对象本身的地址作为一个隐含参数传递给函数。也就是说，即使你没有写上this指针，编译器在编译的时候也是加上this的，它作为非静态成员函数的隐含形参，对各成员的访问均通过this进行。 　　

* 例如，调用date.SetMonth(9) <===> SetMonth(&date, 9)，this帮助完成了这一转换 . 

# this指针的使用

### 1.在类的非静态成员函数中返回类对象本身的时候，直接使用 return *this

### 2.当参数与成员变量名相同时，如this->n = n （不能写成n = n）。 

# this指针的特点
## 1.this只能在成员函数里使用
全局函数、静态函数都不能使用this。实际上，成员函数默认第一个参数为T * const this。

如： 
```c++
class A 
{ 
public: 
int func(int p) 
{ } 
}; 
```
其中，func的原型在编译器看来应该是： 
* int func(A const this,int p)

## 2.this在成员函数的开始前构造，在成员函数的结束后清除。 
这个生命周期同任何一个函数的参数是一样的，没有任何区别。 


