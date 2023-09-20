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

函数遮蔽：子类和父类函数相同优先调用子类，只是重名（重定义）
> 而重写函数名，类型，参数都要相同（重新定义和父类虚函数一模一样的函数的逻辑，函数主体）
解决办法：调用父类 或者域解析符
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
本类指针只能调用本类的方法，**虚函数可以调用子类方法**，父类加virtual，子类就不用加
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

指针函数相当于回调函数


## 0914
向上转型（安全）：用子类对象去构造一个父类对象（将一个子类对象给一个父类对象）
该对象会调用父类方法
向下转型非法
//基类指针只会调用基类析构，不会调用子类析构
//父类指针调用子类析构：多态
Q:多态中，父类指针指向子类对象，释放父类指针只会调用父类析构，导致子类的指针成员无法释放，内存泄漏。
解决办法：为了能调用子类的析构函数，把父类的析构函数声明为虚函数
```c++
A *ptrA = new B(1, 2);  
ptrA->func();
delete ptrA;
```


重载 & 重定义（函数遮蔽） & 重写


纯虚函数:虚函数声明后面 = 0；
特点：
* 具有纯虚函数的类不能实例化对象，该类也称为抽象类（接口类）
* 继承于接口类的类，如果重写纯虚函数，仍为抽象类
* 重写*全部*纯虚函数的子类，可以实例化
作用：定义接口标准

计算立体图形的体积

不清楚类型可以用const char* type
Sort *CreateMySort(int type);变成Sort *CreateMySort(const char* type);

* 封装库：
![](${currentFileDir}/20230914104146.png)

封装动态库：在封装文件所在文件夹上打开终端
g++ -shared -fPIC StdMyString.cpp -o libStdMyString.so
sudo cp libStdMyString.so /usr/lib
sudo cp libStdMyString.h /usr/include

程序运行指令（多加-lStdMyString）：g++ sort.cpp -o sort -lStdMyString


Makefile伪命令


switch只对整型有效

本地化文件配置：不用修改代码

* 类的内存分布:
![](${currentFileDir}/20230914112925.png)

成员变量依旧遵循内存对齐和（struct相同），成员函数不占有类的内存
static被所有类的对象共享，所以不占用内存
const虽然是在表里查找，但还是占用内存，const相当于常量

class继承关系和struct的嵌套规则一样。

子类的对象中，先放父类成员，再放子类成员。
虚函数的实现方式：在对象内存中塞入虚函数指针，塞在对象的开头。
所有派生类只有一个

多态实现的原因：虚函数中含有虚函数指针，虚函数指针指向虚函数表。
虚函数表只有一个。
多个虚函数一个指针就够了。


虚函数占8个字节

虚继承的继承方式：虚继承指针 虚继承表
每多一次虚继承，多一个虚继承指针

带虚函数，带虚继承的实现方式

虚继承的内存顺序：虚函数指针 虚函数成员 虚继承指针 虚继承的成员

![](${currentFileDir}/20230914135834.png)

![](${currentFileDir}/20230914140208.png)
跟构造一样，父类在最后面

```c++
//解决以下代码出现 warning: format ‘%x’ expects argument of type ‘unsigned int’, but argument 2 has type ‘C*’ [-Wformat=]的原因
#include <iostream>
using namespace std;

class A
{
public:
    virtual void func(){}
    static void func2(){}

    // static int m_c;
    int m_a;
    // char m_b;
};

class B:virtual public A
{
public:
    virtual void fun3(){}
    int m_b;
};

class C:public B
{
public:
    int m_c;
};

int main()
{
    // cout<<sizeof(B)<<endl;
    // printf("m_a:%x m_b%x m_%x\n", &B::m_a, &B::m_b, &B::m_d);

    C c;
    cout<<sizeof(c)<<endl;
    printf("%x %x %x %x\n",&c, &c.m_a, &c.m_b, &c.m_c);

    return 0;
}
```

4种数据类型的转换方式
1.static_cast：静态转换：C语言的强制类型转换的替代品
* 基本数据类型的转换，基本数据类型的指针不能转
* 继承关系间的向上转型，无关系的类不能转换
* 可以用于void*（万能指针）和普通指针之间的转换
2.dynamic_cast
* 含有虚函数的父子之间转换
* 向下转型时如果非法，返回空指针nullptr
3.const_cast
const与非const之间的转换
```c++
int s = 10;
const int *a = &s;
int *b = const_cast<int*>(a);
*b = 20;
cout<<s<<endl;
```
4.reinterpret_cast重定义转换


泛型编程： 
模板：
1.当模板函数遇上普通函数的时候，优先调用普通函数。
add<int>(1, 2); 模板的显示调用
add(1, 2)；隐式调用
2.当模板函数能有更好的匹配时，优先选择模板。
3.普通函数有自动类型转换机制，模板没有，必须参数严格匹配。

显示的具体化：
template<> void Swap<Person>(Person& a, Person& b)
显示具体化优先于模板使用

类模板：
类模板的成员函数在类外定义时要当成函数模板去写，要显示声明函数模板类型：
TemplateClass<T>::TemplateClass(T a)

TemplateClass<int> a(10);显示指定类模板的类型

模板函数和模板类的定义都只能在头文件里。

重载了就不需要回调

typename:告诉编译器 T::Node是一个类型名，而不是一个静态成员变量

**移动语义**，右值构造
//StdMyString(const StdMyString&& str);

模板动态数组、模板栈、模板队列、模板数：模板通用树，模板平衡排序二叉树


## 0917
模板类的友元函数：友元函数的友元声明是不会共享类模板的声明的；
友元函数的声明模板变量的类型不能和类模板重名。

模板类的全特化：将一个类模板的所有模板变量都进行特殊化。
template<>
class Test<int, int>
类模板遇上全特化，优先调用全特化

模板类的偏特化：将一个类模板的部分模板变量进行特殊化
当类模板遇上偏特化，优先调用偏特化
template<typename T2>//模板函数的重载，不是偏特化，偏特化要再加一个int
即template<typename T2，int>

函数模板不需要偏特化，用重载函数可以实现对应的需求功能

STL：标准模板库
六大组件：容器、算法、迭代器、仿函数、适配器、空间适配器
不同容器的迭代器：
![](${currentFileDir}/20230918164339.png)

（1）vector:单端动态数组，连续空间

3种初始化：
```c++
//数组初始化
std::vector<int> a(arr,arr+5);
//迭代器初始化
std::vector<int> b(a.begin(), a.begin()+5);
//拷贝初始化
vector<int> c(b);
for(auto&value : b)
{
    cout<<" "<<endl;
}
```

base range for 

vector:[]：vector重载的中括号不会进行越界检测
       at：会抛出越界异常

vector的缺点

* emplace_back & psuh_back
a.emplace_back(StdMyString("world"));//移动语义，减少对象的拷贝，谨慎使用不然内存出错。
内存优化上：采用了就地构造（直接在容器内构造对象，不用拷贝一个复制品再使用）+强制类型转换。
效率上：省去拷贝构造

```c++
1.没有重载中括号就不能使用，a.size()的size是每次都要计算的
    for(int i = 0;i < a.size();i++)
    {
        std::cout<<a[i]<<" ";
    }
    std::cout<<std::endl;
2.迭代器访问，只要容器支持迭代器就可以使用（所有迭代器都支持）
    auto ite = a.begin();
    for(auto ite = a.begin();ite != a.end();ite++);//迭代器
    {
        std::cout<<*ite<<" ";
    }
    std::cout<<*a.begin()<<std::endl;
    std::cout<<*a.end()<<std::endl;
    std::cout<<std::endl;
3.访问速度最快，但无法计算个数，不知道下标和前后的值，最多返回当前值的引用
    for(auto&value : a)
    {
        std::cout<<value<<" ";
    }
```

（2）deque：很少用，双端动态数组，非连续数组，指针数组
用构造的代价换来复杂的空间计算
用vector不能满足头部操作的优化换来更大代价

（3）list：双向链表
list 由链表组成，不能随机读取，不能一次跨越两个
强大在随机存取，插入删除强
list迭代器不能+1
list没有重载下标[]

* std::list是顺序容器，但不是随机访问容器（仅有std::vector，C数组和c++11中的std::array是），可以从两端顺序访问，所以其迭代器只支持++和–这种双向的链式操作（c++11中的slist则只支持++）。如果想一次移动多个位置，也可以使用里的advance函数:
* ```c++
    auto it2 = list.begin();
    advance(it2, 2);
    cout<<*it2;
  ```

list优势：即它可以在序列已知的任何位置快速插入或删除元素 **（时间复杂度为O(1)）**。并且在 list 容器中**移动元素**，也比其它容器的效率高。
list缺点：它不能像 array 和 vector 那样，通过位置**直接访问元素**。举个例子，如果要访问 list 容器中的第 6 个元素，它不支持容器对象名[6]这种语法格式，正确的做法是从容器中第一个元素或最后一个元素开始**遍历**容器，直到找到该位置。

forward_list是C++11引入的新容器之一。它的底层是单向链表，引入它的主要目的是为了达到手写链表的性能。同时节省了部分内存空间。（只有一根指针）。
![](${currentFileDir}/20230918195838.png)


capacity容器扩容

vector<char>自带扩容重载

c++在Linux和wins上编码方式不同
wins上是GBK Linux上是UTF-8

在线聊天室


## 开源C++库的综合列表：
Redis
boost库：HTTP，Websocket
异端HTTP客户端库
QT库
release-OpenCV

(4)set：排序容器，值唯一
底层结构：平衡二叉树之红黑树

lower_bound(value)：输出数列中第一个大于等于Value的值的迭代器
upper_bound(value)：输出数列中第一个大于Value的值

(5)pair对组:

迭代器auto:
vector<pair<string, bool>>::iterator it = v.begin();
auto it2 = v.begin();//代替上面繁冗代码

(6)map容器：键值对列表 key,value
map[] = ;存在则更新数据，可以直接修改值
map.insert();存在键值则无视，不能修改值
insert_or_assign:插入或更新

利用operator()，可以根据大小排序
```c++
class Person
{
    public:
    string m_name;
    int m_age;
    Person(string name, int age)
    {
        m_name = name;
        m_age = age;
    }
}
class myComparePerson
{
    public:
    bool operator()(const Person &p1, const Person &p2) const//const可以修饰类成员函数
    {
        return p1.m_age>p2.m_age;
    }
};
//main
set<Person, myComparePerson> s;
```



## 0919
### 1.map 不能随便插入顺序，是要可以小于号比较的数值类型。字符串要**重载小于号**

### 2.tuple：元组容器，返回值有多个类型
make_tuple
```c++
tuple<int, double, std::string> t(1, 1.2, "hh");
auto t = make_tuple(1, 1.2, "hh");//优化，代价可读性下降
//tie 捆绑
std::tie(myint, mydouble, std::ignore) = t;
//可以用get取值：
get<0>(t)
```

### 3.any：万能匹配
any_cast转换类型不匹配，直接段错误
解决：
使用RTTI：运行时类型信息 void*
1）typeid：返回类型信息
```c++
// 判断pg指向的是否是ClassName类的对象
typeid(ClassName) == typeid(*pg)
```
2）type_info
补充：any可以表示任意类型，所以用不了多态，因为没有统一的接口
any优点：存放类型本身（无需存放指针而担心内存指向内容的生存），可以存放任何类型。
any缺点：用不了多态，用不了统一接口

野指针访问：内存已释放
异常处理:异常三个部分：抛出异常throw，检测异常try，捕获异常
try 尝试运行
{

}
catch 捕获异常
{

}

栈的解旋：在异常抛出后，被捕获之前，释放掉栈上的所有对象
堆呢

体系：纯虚函数->继承->多态

多态:引用（本质：指针对象）和父类指针指向子类对象

![](${currentFileDir}/20230919140512.png)

适配器：栈和队列，没有迭代器
栈要先取出栈元素，再抛出

约瑟夫环：队列，链表

临时对象就是匿名对象

算法库
函数对象（仿函数）：重载了（）的类

lambda表达式：匿名函数对象  适用于短小精悍代码
[]：捕获列表：捕获上文的变量和参数
变量名：捕获变量的值(右值，不能修改，修改要使用引用)
&变量名：捕获变量的引用
[&]：所有变量以引用形式进行捕获
[=]：所有变量以值的形式进行捕获

不能重复捕获

自动推导返回值类型,->表明返回类型
auto function = [&](int a, int b)->int{}

算法库
```c++
class Test1
{
public:
    void operator()(const int &a)
    {
        std::cout<<a<<std::endl;
    }

};

void Print(const int &x)
{
    std::cout<<x<<std::endl;
}
//main
std::vector<int> vec = {1, 2, 3, 4, 5, 6};
//三种表示方法
std::for_each(vec.begin(), vec.end(), Print);//一行遍历
std::for_each(vec.begin(), vec.end(), [](const int& x)       
        {std::cout<<x<<std::endl;});//一行遍历
std::for_each(vec.begin(), vec.end(), Test1());//一行遍历
```

回调三种方法：普通函数指针、函数对象、lambda表达式

vector:
reserve：预留空间，不构建对象，插入insert放入数据
resize：创建空间，构建对象

stable_partition：稳定版本，保存数列的原来的相对位置
partition：不稳定版本

? remove_if max

迭代器的使用：
迭代器失效的时机：（把迭代器当指针）
1.尾插：不一定导致迭代器失效。对于vector：重新分配空间，所有迭代器失效；不重新分配空间，除end外其他迭代器正常。那list呢，只要不动头，头部就不失效
![](${currentFileDir}/20230919193220.png)

2.中间插：
![](${currentFileDir}/20230919193337.png)

multiset和multimap：有序，允许重复的值

map红黑树
unordered_map：无序 ，底层由哈希表实现

function函数包装器

成员与对象，只有绑定了成员才能使用
静态成员函数没有this指针，无需指针可以调用。其他需指针

bind绑定函数 ：（不带std的是在Tcp接口中使用）将一个函数和参数进行绑定，生成一个新的函数对象（改变函数的结构），该对象可以直接调用，但会自动填充参数。可以用于绑定成员函数

std::placeholders::_1：占位符，函数调用时，参数会被自动填充到相应位置

C++ 内存管理
谁申请谁释放：类的内部而已

智能指针：管理堆上空间
* shared_ptr：共享指针。
（1）不要用原始指针去初始化智能指针，当原始指针去初始化智能指针，智能指针默认独占原始指针，当拷贝智能指针时，会造成错误。
（2）循环引用
* unique_ptr：独占指针，独占一块内存，会自动释放。独占体现在拷贝构造被删除，赋值运算符重载函数被删除
* weak_ptr：解决循环引用问题。指向shared_ptr的内存不会让引用计数器加1

内存自动释放原因：
含有引用计数器：计算智能指针指向内存的个数。
当没有指针指向内存，内存自动释放

一旦使用智能指针，就不要用裸指针

libevent 

h文件封装：sudo cp CSTL.h /usr/include/