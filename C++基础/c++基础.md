1. 一个C++源文件从文本到可执行文件经历的过程：gcc hello.c -o hello

(一) 预处理阶段：gcc -E hello.c -o hello.i

对源代码文件中文件包含关系（头文件）、预编译语句（宏定义）进行分析和替换，生成预编译文件。

(二) 编译阶段：gcc –S hello.i –o hello.s

将经过预处理后的预编译文件转换成特定汇编代码（编译原理相关，词法分析、语法分析、语义分析等），生成汇编文件

(三) 汇编阶段：gcc –c hello.s –o hello.o

将编译阶段生成的汇编文件转化成机器码，生成可重定位目标文件

(四) 链接阶段：gcc hello.o –o hello

将多个目标文件及所需要的库打包连接成最终的可执行目标文件（或库文件以供其他程序使用）

2. .c .cc .cpp .h .hpp .inl 这些后缀名都有什么区别

C中：头文件后缀名.h, 源文件后缀名.c
C++中：头文件后缀名.h, .hpp, .hxx, 源文件后缀名.cpp, .cc, .cxx, .C .c++
.h和.hpp的区别是：*.h里面只有声明，没有实现，而*.hpp里声明实现都有，后者可以减少.cpp的数量，适合用来编写公用的开源库。
inl 文件是内联函数的源文件。内联函数通常在c++头文件中实现，但有的时候内联函数较多或者出于一些别的考虑（使头文件看起来更简洁等），往往会将这部分具体定义的代码添加到INL文件中，然后在该头文件的末尾将其用#include引入。由此也可以看到inl文件的一个用法的影子——模板函数、模板类的定义代码的存放。
3. gcc 和 g++的区别

简单来说，gcc与g++都是GNU(组织)的一个编译器，都可以编译c代码与c++代码。但是，后缀为.c的，gcc把它当做C程序，而g++当做是C++程序；后缀为.cpp的，两者都会认为是C++程序。编译阶段，g++会调用gcc，对于c++代码，两者是等价的，但是因为gcc命令不能自动和C＋＋程序使用的库联接，所以通常用g++来完成链接。
c) gcc编译cpp可以使用命令: gcc main.cpp -lstdc++
4. 静态链接、动态链接具体做了什么

静态链接是在形成可执行程序前，而动态链接的进行则是在程序执行时链接。
静态链接浪费空间，这是由于多进程情况下，每个进程都要保存静态链接函数的副本。更新困难，当链接的众多目标文件中有一个改变后，整个程序都要重新链接才能使用新的版本。但是静态链接运行效率高。
动态链接当系统多次使用同一个目标文件时，只需要加载一次即可，节省内存空间。程序升级变得容易，当升级某个共享模块时，只需要简单的将旧目标文件替换掉，程序下次运行时，新版目标文件会被自动装载到内存并链接起来，即完成升级。
静态链接是以目标文件为单位的，将各个目标文件连接起来形成可执行文件
动态链接的基本思想是把程序按照模块拆分成各个相对独立部分，在程序运行时才将它们链接在一起形成一个完整的程序，而不是像静态链接一样把所有程序模块都链接成一个单独的可执行文件
5. C和C++的区别

C是面向过程的语言，是一个结构化的语言，考虑如何通过一个过程对输入进行处理得到输出；C++是面向对象的语言，主要特征是“封装、继承和多态”。
C和C++动态管理内存的方法不一样，C是使用malloc/free，而C++除此之外还有new/delete关键字。
C++支持函数重载，C不支持函数重载
C++中有引用，C中不存在引用的概念
6. 面向对象技术的基本概念与特征

(一) 基本概念：类、对象、继承； 基本特征：封装、继承、多态。

(二) 封装：将低层次的元素组合起来形成新的、更高实体的技术，隐藏了实现细节，使得代码模块化。

(三) 继承：通过派生类继承父类的数据和方法，扩展已经存在的模块，实现代码重用。

(四) 多态：“一个接口，多种实现”，通过派生类重写父类的虚函数，实现了接口的重用

7. C++11的新特性

auto类型推导：让编译器通过初值推断变量的类型（auto定义的变量必须要有初始值），编译时对变量进行了类型推导，所以不会对程序的运行效率造成不良影响。
范围for循环：遍历给定序列的每个元素并对序列中的每个值执行某种操作。
lambda函数：用于定义并创建匿名的函数对象，以简化编程工作。
Override：override关键字保证了派生类中声明重写的函数与基类虚函数有相同的签名，可避免一些拼写错误
final 关键字：final限定某个类不能被继承或某个虚函数不能被重写。空指针常量nullptr消除NULL的二义性问题。因为c++中NULL就是0，0 既可以表示整型，也可以表示一个空指针(void *)。nullptr有类型，且可以被隐式转换为指针类型。
线程支持、智能指针、容器初始化、变长参数模板tuple等
8. 在C++ 程序中调用被C 编译器编译后的函数，为什么要加extern “C”

首先，extern是C/C++语言中表明函数和全局变量作用范围的关键字，该关键字告诉编译器，其声明的函数和变量可以在本模块或其它模块中使用。通常，在模块的头文件中对本模块提供给其它模块引用的函数和全局变量以关键字extern声明。
extern "C"是连接申明(linkage declaration),被extern "C"修饰的变量和函数是按照C语言方式编译和连接的。
作为一种面向对象的语言，C++支持函数重载，而过程式语言C则不支持。函数被C++编译后在符号库中的名字与C语言的不同。例如，假设某个函数的原型为：void foo( int x, int y);该函数被C编译器编译后在符号库中的名字为_foo，而C++编译器则会产生像_foo_int_int之类的名字。这样的名字包含了函数名、函数参数数量及类型信息，C++就是靠这种机制来实现函数重载的。
所以，可以用一句话概括extern “C”这个声明的真实目的:解决名字匹配问题，实现C++与C的混合编程。
9. vs调试和gdb调试

Windows上通过vs直接在代码上调试，在Linux上通过gdb在控制台上输入调试命令调试，可以通过多次命令："l" 来将所要调试的代码显示到控制台，然后看着代码调试。
vs中可以通过鼠标点击来下断点，而gdb是通过命令“b num”来下断点，其中b是命令，num是需要下断点的行数。
一步一步的调试, 在vs中有点击的图标，gdb中通过命令:"r" 让代码执行到你下的第一个断点，命令："c"让代码执行到下一个断点。
到断点后还需要一步一步的执行，gdb中命令："n" 让代码一步一步的执行，一个命令执行一步。
碰到需要调用的函数时，命令"n"不能进入函数，gdb中命令："s" 来进入函数内部执行
代码调试跑起来后，需要观察每次运行代码想要查看的值，gdb中使用命令：“p val”p是查看的命令，val是我们需要查看的变量，p &val，查看变量地址，p *ptr@len 通过指向数组的指针显示数组所有的元素，ptype val 显示变量的类型。
10. C++的内存管理

在C++中，内存被分成五个区：栈、堆、自由存储区、静态存储区、常量区

(一) 栈：存放函数的参数和局部变量，编译器自动分配和释放
(二) 堆：new关键字动态分配的内存，由程序员手动进行释放，否则程序结束后，由操作系统自动进行回收
(三) 自由存储区：由malloc分配的内存，和堆十分相似，由对应的free进行释放
(四) 全局/静态存储区：存放全局变量和静态变量
(五) 常量区：存放常量，不允许被修改
11. C++中内存泄漏的几种情况

内存泄漏是指己动态分配的堆内存由于某种原因程序未释放或无法释放，造成系统内存的浪费，导致程序运行速度减慢甚至系统崩溃等严重后果。有以下几个原因：

1）类的构造函数和析构函数中new和delete没有配套
2）在释放对象数组时没有使用delete[]，使用了delete
3）没有将基类的析构函数定义为虚函数，当基类指针指向子类对象时，如果基类的析构函数不是virtual，那么子类的析构函数将不会被调用，子类的资源没有正确释放，因此造成内存泄露
4）没有正确的清楚嵌套的对象指针
12. new、delete、malloc、free之间的关系

new/delete,malloc/free都是动态分配内存的方式；
new/delete是运算符，编译器保证调用构造和析构函数对对象进行初始化/析构，但是库函数malloc/free是库函数，不会执行构造/析构；
new会自动计算需分配的空间，malloc不行；
new是类型安全的，而malloc不是；
new返回指定类型指针，malloc返回void*指针，需要强制类型转换；
new可以被重载，malloc不能
new底层调用malloc函数分配内存，然后调用构造函数
13. delete和delete[]的区别

a) delete只会调用一次析构函数，而delete[]会调用每个成员的析构函数
b) 用new分配的内存用delete释放，用new[]分配的内存用delete[]释放
14. 类中 private，protect，public 三种访问限制类型的区别

(一) private 是私有类型，只有本类中的成员函数访问;
(二) protect 是保护型的，本类和继承类可以访问;
(三) public 是公有类型，任何类都可以访问.
15. struct与union的区别

结构体：将不同类型的数据组合成一个整体，是自定义类型

共同体：不同类型的几个变量共同占用一段内存

1）结构体中的每个成员都有自己独立的地址，它们是同时存在的；共同体中的所有成员占用同一段内存，它们不能同时存在；
2）sizeof(struct)是内存对齐后所有成员长度的总和，sizeof(union)是内存对齐后最长数据成员的长度
16. 什么是内存对齐？字节对齐的规则是什么？

尽管内存是以字节为单位的，但是大部分处理器并不是以字节来存取数据，一般会以四字节、八字节或更长的单位来取内存。使用内存对齐可以保证每次取内存都是访问块内存地址首部以提高存取效率。

字节对齐规则：

1) 结构体中每个变量首地址的偏移量必须能够被其有效对齐值 min(变量自身对齐值, 编译器指定对齐值) 整除。
2) 结构体的自身对齐值为结构体中最宽变量的大小，结构体的大小必须被其有效对齐值 min(结构体的自身对齐值, 编译器指定对齐值) 整除。
17. #define和const的区别

1）#define定义的常量没有类型，所给出的是一个立即数；const定义的常量有类型名字，存放在静态区域
2）处理阶段不同，#define定义的宏变量在预处理时进行替换，可能有多个拷贝，const所定义的变量在编译时确定其值，只有一个拷贝。
3）#define定义的常量是不可以用指针去指向，const定义的常量可以用指针去指向该常量的地址
4）#define可以定义简单的函数，const不可以定义函数
18. 指针和引用的区别

引用是变量的一个别名，内部实现是只读指针
引用只能在初始化时被赋值，其他时候值不能被改变，指针的值可以在任何时候被改变
引用不能为 NULL，指针可以为 NULL
引用变量内存单元保存的是被引用变量的地址
“sizeof 引用” = 指向变量的大小 ， “sizeof 指针”= 指针本身的大小
引用可以取地址操作，返回的是被引用变量本身所在的内存单元地址
引用使用在源代码级相当于普通的变量一样使用，做函数参数时，内部传递的实际是变量地址
19. 头文件中的ifndef/define/endif有什么作用

这是C++预编译头文件保护符，保证即使文件被多次包含，头文件也只定义一次。

20. ＃include<file.h> 与 ＃include "file.h"的区别

前者是从标准库路径寻找和引用file.h
后者是从当前工作路径搜寻并引用file.h
21. 智能指针

智能指针：C++内存管理是一个令人很头疼的事情，尽管每次写完new都会写一个delete，但是如果程序还没有执行到delete的时候就跳转了或者函数返回了，那么就会导致内存泄漏，使用智能指针可以很大程度上的避免这个问题，因为智能指针就是一个类，当类的实例超出了作用域的时候，就会自动调用其析构函数，析构函数会自动释放资源。

三种智能指针：unique_ptr，shared_ptr，weak_ptr。

(一) shared_ptr维护了一个指向control block的指针对象，来记录引用个数。
(二) weak_ptr用于避免shared_ptr相互指向产生的环形结构，造成的内存泄漏。weak_ptr count是弱引用个数；弱引用个数不影响shared count和对象本身，shared count为0时则直接销毁，不能通过weak_ptr直接访问对象的方法，要先通过lock()转换为shared_ptr。
(三) unique具有唯一性，对指向的对象值存在唯一的unique_ptr。unique_ptr不可复制，赋值，但是move()可以转换对象的所有权，局部变量的返回值除外。与shared_ptr相比，若自定义删除器，需要在声明处指定删除器类型，而shared不需要，shared自定义删除器只需要指定删除器对象即可，在赋值时，可以随意赋值，删除器对象也会被赋值给新的对象。unique的实现中，删除器对象是作为unique_ptr的一部分，而shared_ptr，删除器对象保存在control_block中。
判断weak_ptr的对象是否失效有三种方法：

1) expired()：检查被引用的对象是否已删除。
2) lock()会返回shared指针，判断该指针是否为空。
3) use_count()也可以得到shared引用的个数，但速度较慢。
22. C++四种类型转换符各自的作用

(一) static_cast:

1）在基本数据类型之间转换，如把 int 转换为 char，这种带来安全性问题由程序员来保证；
2）在有类型指针与 void * 之间转换；(不能使用 static_cast 在有类型指针内转换)
3）用于类层次结构中基类和派生类之间指针或引用的转换。上行转换（派生类---->基类）是安全的；下行转换（基类---->派生类）由于没有动态类型检查，所以是不安全的。
(二) dynamic_cast: 用于将一个父类的指针/引用转化为子类的指针/引用（下行转换）。基类必须要有虚函数，因为 dynamic_cast 是运行时类型检查，需要运行时类型信息，而这个信息是存储在类的虚函数表中。

(三) const_cast: 常量指针（或引用）与非常量指针（或引用）之间的转换。

(四) reinterpret_cast: 用在任意指针（或引用）类型之间的转换。能够将整型转换为指针，也可以把指针转换为整型或数组。

23. 虚函数是什么以及其作用

虚函数是允许被其子类重新定义的成员函数，可以实现用父类型的指针指向其子类的实例，然后通过父类的指针调用实际子类的成员函数。
有了虚函数，基类指针指向基类对象时就使用基类的成员（包括成员函数和成员变量），指向派生类对象时就使用派生类的成员，从而实现多态。
注意，构造函数不能为虚函数，但是析构函数可以为虚函数，并且虚析构函数可以防止父类指针销毁子类对象时不正常导致的内存泄漏。
普通函数（非成员函数）、构造函数、友元函数、静态成员函数、内联成员函数，不能声明为虚函数。
24. 虚函数表和纯虚函数

虚函数是通过一张虚函数表来实现的。简称为V-Table。如果一个类中包含虚函数（virtual修饰的函数），那么这个类就会包含一张虚函数表，虚函数表存储的每一项是一个虚函数的地址。
纯虚函数是在基类中声明的虚函数，它在基类中没有定义，但要求任何派生类都要定义自己的实现方法，带有纯虚函数的类为抽象类。
析构函数可以是纯虚的，但纯虚析构函数必须有定义体，因为析构函数的调用是在子类中隐含的。
25. 构造函数为什么不能为虚函数

1) 当派生类在创建对象的时候会调用基类的构造函数，但是如果基类的构造函数是虚函数的话，派生类的构造函数又会把基类的构造函数覆盖，所以无法进一步执行而出错。
2) 同时，虚函数通过虚函数表来实现，而指向虚函数表的指针也需要在对象实例化后创建，那么就违背了先实例化后调用的准则。