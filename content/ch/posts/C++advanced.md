---
title: C++继承/多态/虚函数
date: 2019-12-20
lastmod: 2019-12-27 
url:
    /tips/cpp-advanced
tags:
    - Tips  
    - C++
categories:
    - 2019-12
description: ""
---


继承
--

### 访问控制和继承

| 访问 | public | protected | private |
| --- | --- | --- | --- |
| 同一个类 | yes | yes | yes |
| 派生类 | yes | yes | no |
| 外部的类 | yes | no | no |

一个派生类继承了所有的基类方法，但下列情况除外：

*   基类的构造函数、析构函数和拷贝构造函数。
*   基类的重载运算符。
*   基类的友元函数。

#### 一个问题

    #include <iostream>
    class A{
    private:
    int a;
    char b;
    };
    
    class B: public A{
    public:
    char getc(){return this->c;}
    private:
    char c='j';
    };
    
    int main() {
        B test;
        cout<<test.getc()<<endl;
        cout<<sizeof(int)<<endl;
      // output is 4
        cout<<sizeof(char)<<endl;
     // output is 1
        cout<<sizeof(test)<<endl;
     // output is 8
    }

> 如果把A的`private`属性`a`,`b`变成`public`属性，那么`test`的size就变成了12

### 继承类型

*   **公有继承（public）：**当一个类派生自**公有**基类时，基类的**公有**成员也是派生类的**公有**成员，基类的**保护**成员也是派生类的**保护**成员，基类的**私有**成员不能直接被派生类访问，但是可以通过调用基类的**公有**和**保护**成员来访问。
*   **保护继承（protected）：** 当一个类派生自**保护**基类时，基类的**公有**和**保护**成员将成为派生类的**保护**成员。
*   **私有继承（private）：**当一个类派生自**私有**基类时，基类的**公有**和**保护**成员将成为派生类的**私有**成员。

在多继承时，如果省略继承方式，默认为private

多态
--

C++多态性是通过虚函数来实现的

多态与非多态的实质区别就是函数地址是早绑定还是晚绑定。如果函数的调用，在编译器编译期间就可以确定函数的调用地址，并生产代码，是静态的，就是说地址是早绑定的。而如果函数调用的地址不能在编译器期间确定，需要在运行时才确定，这就属于晚绑定。

### 多态的四种形式

多态总体上分为：编译时的多态（静态多态）和运行时的多态（动态多态）。又被细分为：**参数多态**，**包含多态**，**过载多态**，**强制多态**。前两种为通用多态，后两种为特定多态。

1.  参数多态：采用参数化模板，通过给出不同的类型参数，使得一个结构有多种类型。如 C++语言中的函数模板和类模板属于参数多态。参数多态又叫静态多态，它的执行速度快，异常少，调用在编译时已经确定。参数多态是应用比较广泛的一种多态，被称为最纯的多态。
2.  包含多态：在许多语言中都存在，最常见的例子就是子类型化，即一个类型是另外一个类型的子类型。一般需要进行运行时的类型检查，属于动态多态。包含多态的基础是虚函数。虚函数是引入了派生概念后用来表现基类和派生类的成员函数之间的一种关系。
3.  过载多态：同一个名字在不同的上下文中所代表的含义不同。典型的例子是运算符重载和函数重载，属于静态多态。
4.  强制多态：编译程序通过语义操作，把操作对象的类型强行加以变换，以符合函数或操作符的要求。程序设计语言中基本类型的大多数操作符，在发生不同类型的数据进行混合运算时，编译程序一般都会进行强制多态。程序员也可以显示地进行强制多态的操作。如 int+double，编译系统一般会把 int 转换为 double，然后执行 double+double 运算，这个int->double 的转换，就实现了强制多态，即可是隐式的，也可显式转换。强制多态属于静态多态。

### 相关概念

**多态性**

指相同对象收到不同消息或不同对象收到相同消息时产生不同的实现动作。C++支持两种多态性：编译时多态性，运行时多态性。  
**  a、编译时多态性：通过重载函数实现**  
**  b、运行时多态性：通过虚函数实现**

[https://blog.csdn.net/Hackbuteer1/article/details/7475622](https://blog.csdn.net/Hackbuteer1/article/details/7475622)

虚函数
---

### 定义

C++中的虚函数的作用主要是实现了多态的机制。基类定义虚函数，子类可以重写该函数；在派生类中对基类定义的虚函数进行重写时，需要再派生类中声明该方法为虚方法。

在派生类中重新定义基类中定义的虚函数时，会告诉编译器不要静态链接（又称早绑定：基类定义的函数没有使用virtual关键字，调用的函数被编译器设置为基类中的版本）到该函数。我们想要的是在程序中任意点可以根据所调用的对象类型来选择调用的函数，这种操作被称为**动态链接**，或**后期绑定**。

虚函数只能借助于指针或者引用来达到多态的效果。

当子类重新定义了父类的虚函数后，当父类的指针指向子类对象的地址时，\[即B b; A a = &b;\] 父类指针根据赋给它的不同子类指针，动态的调用子类的该函数，而不是父类的函数（不使用virtual方法，如果使用了**virtual**关键字，程序将根据**引用或指针**指向的 **对象类型**来选择方法，否则使用**引用类型或指针类型**来选择方法。），且这样的函数调用发生在运行阶段，而不是发生在编译阶段，称为**动态联编**。

### 实例

    
    class A
    {
    public:
    	void fee()
    	{
    		cout<<"Parent"<<endl;
    	}
    &amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;virtual void foo()
    &amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;{
    &amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;&amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;cout<<"A::foo() is called"<<endl;
    &amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;}
    };
    class B:public A
    {
    public:
    	void fee()
    	{
    		cout<<"Child"<<endl;
    	}
    &amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;void foo()
    &amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;{
    &amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;&amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;cout<<"B::foo() is called"<<endl;
    &amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;}
    };
    int main(void)
    {
    &amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;A *a = new B();
    &amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;a->fee();	// 输出为：Parent
    &amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;a->foo();&amp;amp;amp;nbsp;&amp;amp;amp;nbsp; // 输出为：B::foo() is called
    &amp;amp;amp;nbsp;&amp;amp;amp;nbsp; &amp;amp;amp;nbsp;return 0;
    }
    

### 虚函数底层实现机制

**实现原理：虚函数表+虚表指针**

编译器处理虚函数的方法是：

为每个类对象添加一个隐藏成员，隐藏成员中保存了一个指向函数地址数组的指针，称为虚表指针（vptr），这种数组成为虚函数表（virtual function table, vtbl），即，**每个类使用一个虚函数表，每个类对象用一个虚表指针**。

基类对象包含一个虚表指针，指向基类中所有虚函数的地址表。派生类对象也将包含一个虚表指针，指向派生类虚函数表。

*   如果派生类重写了基类的虚方法，该派生类虚函数表将保存重写的虚函数的地址，而不是基类的虚函数地址。
*   如果基类中的虚方法没有在派生类中重写，那么派生类将继承基类中的虚方法，而且派生类中虚函数表将保存基类中未被重写的虚函数的地址。注意，如果派生类中定义了新的虚方法，则该虚函数的地址也将被添加到派生类虚函数表中。

![](api/images/999Yw3pmkXIA/虚表指针.jpg)

基类虚表指针

    class A {
    public:
        virtual void vfunc1();
        virtual void vfunc2();
        void func1();
        void func2();
    private:
        int m_data1, m_data2;
    };
    
    class B : public A {
    public:
        virtual void vfunc1();
        void func1();
    private:
        int m_data3;
    };
    
    class C: public B {
    public:
        virtual void vfunc2();
        void func2();
    private:
        int m_data1, m_data4;
    };
    

类A是基类，类B继承类A，类C又继承类B：

![](api/images/QKs9akFzvofb/20160528104806455.jpg)

继承

纯虚函数
----

### 定义

纯虚函数是在基类中声明的虚函数，它在基类中没有定义，但要求任何派生类都要定义自己的实现方法。在基类中实现纯虚函数的方法是在函数原型后加“=0”：`virtual void funtion1()=0`

声明了纯虚函数的类是一个抽象类。所以，用户不能创建类的实例，只能创建它的派生类的实例。派生类仅仅只是继承函数的接口，必须实现函数才能使用。

### 抽象类

抽象类是一种特殊的类，它是为了抽象和设计的目的为建立的，它处于继承层次结构的较上层。

1.  抽象类的定义：称带有纯虚函数的类为抽象类。
2.  抽象类的作用：抽象类的主要作用是将有关的操作作为结果接口组织在一个继承层次结构中，由它来为派生类提供一个公共的根，派生类将具体实现在其基类中作为接口的操作。所以抽象类实际上刻画了一组子类的操作接口的通用语义，这些语义也传给子类，子类可以具体实现这些语义，也可以再将这些语义传给自己的子类。
3.  使用抽象类时注意：

*   抽象类只能作为基类来使用，其纯虚函数的实现由派生类给出。如果派生类中没有重新定义纯虚函数，而只是继承基类的纯虚函数，则这个派生类仍然还是一个抽象类。如果派生类中给出了基类纯虚函数的实现，则该派生类就不再是抽象类了，它是一个可以建立对象的具体的类。
*   抽象类是不能定义对象的。

Conclusion
----------

在有动态分配堆上内存的时候，析构函数必须是虚函数，但没有必要是纯虚的。  
友元不是成员函数，只有成员函数才可以是虚拟的，因此友元不能是虚拟函数。但可以通过让友元函数调用虚拟成员函数来解决友元的虚拟问题。

析构函数应当是虚函数，将调用相应对象类型的析构函数，因此，如果指针指向的是子类对象，将调用子类的析构函数，然后自动调用基类的析构函数。

Reference
---------

[https://blog.csdn.net/Hackbuteer1/article/details/7558868](https://blog.csdn.net/Hackbuteer1/article/details/7558868)

[https://blog.csdn.net/iFuMI/article/details/51088091](https://blog.csdn.net/iFuMI/article/details/51088091)