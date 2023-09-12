## 9.8
1.cpp和c的不同是编程思想不同

2.::域解析符

3.命名空间：值、函数

4.bool类型：true、false 非0就是1
            大小:1字节
            接口返回类型为bool

5.const
在C语言中通过指针间接影响const值;c++中const值无法修改。
```c
const int a = 1;
int *p = (int *)&a;`
```

表的优先级在内存之上
符号表在预处理阶段完成，define，const

6.constexpr常量表达式,修饰的变量有const属性，在编译器确定值。表达式左右两边都是常量。

7.inline内联函数
不要出现循环（也包括递归）、函数代码不要太长、逻辑太复杂，函数不能有取地址行为、函数内不能有静态变量

8.引用：变量的别名 &
int&：int类型的引用
引用必须初始化

9.函数重载
add(int, int)即_int_int_add
条件：参数类型不同，参数个数不同，参数顺序不同，无法用返回值类型区分重载

1）默认参数：
当参数没有被传入时，以默认值进行传入。
默认参数只能放在参数列表末尾。
默认参数会导致重载函数的歧义，最后别同时出现。

2）占位符
int add(int a, int b, int = 0)；

10.三目运算符
(a > b ? a : b)
c中作右值，c++中作左值是变量

11.类
protect、public、private

成员方法
函数类外定义,要声明那个类::
构造函数（函数名与类名相同，
         没有返回值，
         可以重载，
         系统给类分配默认无参空实现的构造函数，自定义了，就不会分配默认构造函数）
析构函数(对象被释放时自动调用)（函数名与类名相同，
                             没有返回值）
new & delect：堆空间操作，申请与释放一个对象。
              运算符关键字，无需头文件
              > student *stu1 = new student(100);
* 对象 = new 对象[];

this:指向本类对象

初始化列表：给不能正常赋值的对象赋初值
```c++
//h
const int grade;
//cpp
student::student(const char *name, int age):grade(16),age(age)
//可以区分成员变量和传入参数
```

12.static
静态成员变量：
在类里用static定义变量，则变量属于整个类，不独属于某一个类的对象

静态成员函数：
该函数可直接调用，不需要对象调用，不属于某一特定对象。也没有this指针，也不能访问非静态成员变量
```c++
//h
void static func();
//cpp
student::func();
```

* static关键字的作用：
修饰：
  C:局部变量（只初始化一次，全局保存）、全局变量（对其他文件隐藏）
  C++：成员变量（由该类成员共享，只初始化一次）、成员函数（该函数可直接调用，不需要对象调用，可以直接用类调用，只能访问静态成员变量）

struct & class
C：
1.struct没有访问权限的区别
2.struct不能放函数
3.struct不算新的数据类型定义，class是定义的新类型
C++：
默认的访问权限不同

用struct隐藏private的参数
```c++
//h
struct studentPrivate;
//class Student里定义
studentPrivate *p;
//cpp
struct studentPrivate
{
    int age;
    char *name;
    const int grade;

    studentPrivate():grade(16){};
    ~studentPrivate(){};
};
//构造函数调用p
student::student(const char *name, int age):p(new studentPrivate)
```


数据结构：单向数据库，双向链表

拷贝构造函数：为什么参数一定是引用
引用不会改变原内容

构造函数和拷贝构造函数调用同一个
所以每个对象都要申请空间new

C++也提供了默认复制构造函数

浅拷贝：调用的构造函数对于指针成员只进行地址的值复制，会造成二次释放问题。故要进行深拷贝：
拷贝时将指针的内存拷贝，而不是指针的值拷贝。


检查内存泄漏：new与delete配对，拷贝构造是深拷贝


## 9.10
1.C++之rvo和移动语义
移动语义：抢内存，目的是减少对象的拷贝
```c++
//类
demo();
demo GetCopy()
{
    demo temp = *this;
    return temp;
}
~demo();
//主函数
demo a;
demo c = a.GetCopy();//return抢走原内存（temp）内存，原对象内存不能再使用

//最后只进行两次构造析构
```

2.左值（有内存的对象）右值（只有值）
```c++
demo a;
demo b = a;//=相当于拷贝，a内存给b
```
右值引用：把一个左值变右值
```c++
demo(demo&& d);//右值引用，两个&

demo::demo(demo &&d)
{
    this->m = d.m;
    d.m = NULL;
    printf("move construction\n");
}

demo a;
demo d = std::move(a);//move变右值
```

3.std::cout & std::cin 标准输入输出流

4.匿名对象：作用域就一行,如demo(10)
```c++
//定义一个对象，想再输入值
    demo a = demo(10);
//结果只构造一次有参构造和析构
```

5.explicit：解决构造函数的隐式转换


6.const三种用法：形参，返回值，函数
（1）const对（对象，会触发移动语义）返回值没有约束：
```c++
const integer GetInteger()
{
    return *this;
}
```
（2）但是const的约束力只在返回值是指针的时候有效
```c++
const int* GetInteger()
{
    return &num;
}
```
（3）常函数：const在函数最后加，修饰是是函数成员（私有成员不允许修改）,故在函数内不能对类的成员修改
```c++
void SetNum(int a)const
{
    num = a;//num是私有成员，会报错
}
```

7.运算符的重载：operator + 运算符 (符号：+ - .....)
```c++
//const virtualNumm& a;避免实参到形参的拷贝
virtualNum operator+(const virtualNum& a)//c需要写两个参数，c++使用this只用写入一个
{
    virtualNum c(0, 0);
    c.real = a.real + this->real;
    c.vir = a.vir + this->vir;
    return c;
}
```

重载可以直接使用运算符

8.友元函数 friend 允许访问私有成员
会破坏封装性，少用

友元类

友元关系是单向的。


> void不能连续等于
> 对象返回要多调用一次引用构造
> 调用不需要额外

![](${currentFileDir}/20230910151335.png)

单独的时候使用赋值重载
StdMyString str5 = "hhhhh"隐式转换


类型运算符


## 0912
1.new 对象数组
delete指针时 只会释放第一个对象
delete数组： delete []d;

对于基本类型的数组： new delete
自定义对象类的数组： new delete []p;

就地构造:
在调用构造函数开始时先调用就地构造.
委托构造：
在构造函数的初始化列表中调用别的构造函数。
顺序：就地 > 委托 > 自己

继承：**代码复用**
公有，私有（父类所有成员到子类全变成私有成员，保护protected（该类和继承该类的类都可以访问，父类所有成员到子类变保护成员）
继承构造using

函数遮蔽：子类和父类函数相同优先调用子类，即重写
解决办法：调用父类
```c++
using Person::SetId;
void SetId(const float& id);
```

子类对象的创建和销毁顺序：父类构造->子类构造->子类析构->父类析构

多继承:子类对象在构造时调用的构造函数的顺序只与继承 有关，与调用顺序无关

菱形继承的参数的二义性-> **虚继承**：
在多重继承中，保证所有的派生类的父类副本都只有一份

**代码复用**：组合
把一个类的对象当成另一个对象的成员变量

计算图形面积
圆适合用继承：只需一个点
长方形更适合组合：



多态：一个接口，多种状态
接口复用：就是函数复用
1.c++在父子类之间允许类型转换，有继承关系
2.父类指针指向子类对象
> 指针是8个字节，智能指针能指向任何字节的对象
```c++
//用父类指针指向子类对象，调用子类的接口
Shape* s = &rect;
s->GetArea();
s = &c;
s->GetArea();
```
3.重写虚函数:
本类指针只能调用本类的方法，虚函数可以调用子类方法，父类加virtual，子类就不用加
```c++
virtual Shape::double GetArea();
```
> 可以用override检测子类重写虚函数名是否正确：double GetArea() override;

重载和重写区别：
1.重载发生在同一个类内或者全局，重写发生在父子之间
2.重载要求函数名相同，参数不同；重写是子类方法名和父类相同，则直接函数遮蔽
3.重载发生在编译阶段，重写体现在运行期间
4.重载被称为静多态，重写是动多态


服务器用多态