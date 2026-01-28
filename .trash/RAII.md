￼ 搜索
目录
简介
[零基础C++]
变量和输出
作用域和存储空间
头文件和源文件
extern的作用
引用类型
指针类型
const用法
类型推导和别名
结构体类型
命名空间
string类用法
vector类用法
迭代器用法
数组知识
多维数组
常见运算符
语句和作用域
函数用法
类基础用法
继承和多态
类面试常见问题
内存管理
智能指针
可调用对象function
stl容器详解
手写双端队列
手写线程安全智能指针
map用法以及手写map
unordermap用法
运算符重载
模板详解
万能引用和原样转发
单例模式演变
常用stream流
结课项目一(异步日志系统)
结课项目二(任务管理系统)
完结-封装继承多态
[C++]
[C++环境搭建]
[C++基础语法]
[C++类的认识]
[C++泛型用法]
[C++智能指针]
[C++内存管理]
[C11新特性]
[C++模板]
[Qt界面编程]
[Boost和网络]
[分布式技术]
[并发编程]
[C++全栈项目实战]
框架概述和登录界面设计
单例模板和http管理类
visualstudio配置boost
beast实现http服务器
处理post请求并解析json数据
windows编译grpc
visualstudio配置grpc
nodejs实现邮箱认证服务
redis服务搭建
多服务验证码派发功能
注册功能
注册界面完善
重置界面
登录和状态服务
客户端Tcp管理类设计
asio实现tcp服务器
登录服务验证和客户端数据管理
聊天主界面和点击按钮
实现搜索框和聊天列表
动态加载聊天列表
滚动聊天布局设计
实现气泡聊天对话框
侧边栏切换和搜索列表展示
事件过滤器实现点击位置判断
实现好友申请界面
实现联系人列表和好友申请列表
分布式聊天服务设计
好友查询和申请
好友认证和聊天通信
项目难点和面试技巧
文件传输
分布式锁设计思路
单服踢人逻辑
跨服踢人逻辑
心跳检测
QT头像裁剪
聊天信息存储方案
断点续传
多媒体信息传输
分布式事务死锁分析
Qt粘包分析和解决
[golang]
[Python]
[网络编程]
[解决方案]
详细技术视频请看我的主页

C++教程视频

什么是默认构造
默认构造就是不带参数的构造函数，如果我们不实现任何构造函数，系统会为我们生成一个默认的构造函数

比如下面

#include <thread>
class JoiningThread {
public:
    int GetIndex() const { return _i; }
private:
    std::thread _t;
    int _i;
};
所以我们可以直接使用默认构造函数构造一个对象，并且打印成员_i

//测试默认合成
JoiningThread jt;
std::cout << "member _i is " << jt.GetIndex() << std::endl;
输出

member _i is -1284874240
可以看到默认构造函数并不会帮我们初始化类成员变量。

什么是有参构造
有参构造就是传递参数的构造函数，可以根据参数构造对象

#include <thread>
class JoiningThread {
public:
    JoiningThread(int i) : _i{i} {}
    int GetIndex() const { return _i; }
private:
    std::thread _t;
    int _i;
};
我们可以通过如下方式构造

JoiningThread jt(1);
std::cout << "member _i is " << jt.GetIndex() << std::endl;
当我们执行程序，会输出

member _i is 1
但如果这样构造会产生问题

JoiningThread jt;
std::cout << "member _i is " << jt.GetIndex() << std::endl;
注意

如果我们实现了参数构造而不实现无参构造，系统将不会为我们实现默认构造，导致无法使用默认构造生成对象。

所以稳妥一点，我们基本都会实现无参构造

#include <thread>
class JoiningThread {
public:
    JoiningThread() :_i(0){}
    JoiningThread(int i) : _i{i} {}
    int GetIndex() const { return _i; }
private:
    std::thread _t;
    int _i;
};
拷贝构造函数是什么
回答要点：

定义：拷贝构造函数用于创建一个对象，该对象是通过复制另一个同类型对象来初始化的。

调用时机

：

使用现有对象初始化新对象。
按值传递对象作为函数参数。
按值返回对象。
默认拷贝构造函数：成员逐个拷贝。

示例：

class MyClass {
public:
    MyClass(const MyClass& other) { // 拷贝构造函数
        // 复制代码
    }
};
是否会默认生成拷贝构造
在 C++ 中，如果你没有为一个类显式定义拷贝构造函数，编译器会自动生成一个默认的拷贝构造函数。这个默认拷贝构造函数会按成员的逐个拷贝（member-wise copy）方式来复制对象的每个成员变量。

默认拷贝构造函数的行为
逐个拷贝：默认拷贝构造函数会逐个拷贝所有的非静态成员变量。
指针成员：如果类中有指针成员，默认拷贝构造函数只会拷贝指针的值（地址），而不会拷贝指针所指向的对象。这可能会导致多个对象指向同一块内存，进而引发问题（如双重释放、内存泄漏等）。
const 和引用成员：如果类中有 const 成员或引用成员，编译器不会生成默认的拷贝构造函数，因为这些成员不能被复制。
类中包含不可拷贝对象时，无法合成默认拷贝构造函数
拷贝构造是否必须实现
当一个类A中有成员变量是另一个类类型B的时候，有时候拷贝构造会失效。

比如一个类JoiningThread中有成员变量std::thread，std::thread没有构造函数，所以A类的拷贝构造无法合成，需要显示编写。

比如我们这样调用

JoiningThread jt(1);
JoiningThread jt2(jt);
std::cout << "member _i is " << jt.GetIndex() << std::endl;
上面代码报错

error: use of deleted function 'std::thread::thread(const std::thread&)'
所以我们要显示实现拷贝构造，指定一个拷贝规则

JoiningThread(const JoiningThread & other): _i(other._i){}
什么是浅拷贝和深拷贝
类在拷贝构造或者拷贝赋值的时候，将被拷贝的类中的成员值拷贝到目的类，如果被拷贝的类中包含指针成员，只是简单的拷贝指针的值。

同样析构也要显示编写，等待线程完成。

除此之外我们可以自己实现拷贝构造，进而实现浅拷贝和深拷贝的不同效果

￼

￼

构造顺序和析构顺序
类A中包含成员变量是类B的类型，如果是先调用A的构造还是B的构造呢？

如果析构的时候是A先析构还是B先析构呢？

class InnerB {
public:
    InnerB() {
        std::cout << "InnerB()" << std::endl;
    }

    ~InnerB(){
        std::cout << "~InnerB()" << std::endl;
    }
};

class WrapperC {
public:
    WrapperC(){
        std::cout << "WrapperC()" << std::endl;
    }
    ~WrapperC(){
        std::cout << "~WrapperC()" << std::endl;
    }
    InnerB _inner;
};
执行结果，先调用B的构造，在调用C的构造。

析构时，先析构C再析构B

InnerB()
WrapperC()
~WrapperC()
~InnerB()
类默认构造是否必须实现
如果类中有继承关系或者其他类型的成员，默认构造函数是很有必要实现的。

系统提供的合成的默认构造函数不会对成员做初始化操作。

比如我们之后要学到的继承

class DerivedA: public BaseA {
public:
    DerivedA(std::string name,std::string num) :
    BaseA(name), _num(num) {
        std::cout << "DerivedA()" << std::endl;
    }
private:
   std::string _num;
};
调用

 DerivedA a("zack","1001");
this 指针的特性和用途
指向当前对象：

this 指针是一个隐式参数，指向调用成员函数的对象。通过 this，你可以访问当前对象的属性和方法。
区分成员变量和参数：

在构造函数或成员函数中，参数名和成员变量可能同名。使用
 ```
 this
 ```



 可以明确指代成员变量。例如：

 ```cpp
 class MyClass {
 private:
     int value;
 public:
     MyClass(int value) {
         this->value = value; // 使用 this 指针区分成员变量和参数
     }
 };
 ```
返回当前对象：

this
 可以用于返回当前对象的引用，以支持链式调用。例如：

 ```cpp
 class MyClass {
 private:
     int value;
 public:
     MyClass& setValue(int value) {
         this->value = value;
         return *this; // 返回当前对象的引用
     }
 };

 MyClass obj;
 obj.setValue(10).setValue(20); // 链式调用
 ```
在 const 成员函数中的使用：

在 const 成员函数中，this 的类型为 const MyClass*，这意味着你不能通过 this 修改成员变量。这有助于确保对象的状态不被改变。
在静态成员函数中的不可用性：

静态成员函数没有 this 指针，因为它们不属于任何特定对象，而是属于类本身。因此，静态成员函数不能访问非静态成员变量和成员函数。
示例代码

以下是一个简单的示例，展示了 this 指针的用法：

#include <iostream>

class MyClass {
private:
    int value;

public:
    // 构造函数
    MyClass(int value) {
        this->value = value; // 使用 this 指针区分成员变量和参数
    }

    // 成员函数
    MyClass& setValue(int value) {
        this->value = value; // 使用 this 指针
        return *this; // 返回当前对象的引用
    }

    // 输出值
    void printValue() const {
        std::cout << "Value: " << this->value << std::endl; // 使用 this 指针
    }
};

int main() {
    MyClass obj(10);
    obj.printValue(); // 输出: Value: 10

    obj.setValue(20).printValue(); // 链式调用，输出: Value: 20

    return 0;
}
delete和default
C++11用法：

delete可以删除指定的构造函数。

default可以指定某个构造函数为系统默认合成。

class DefaultClass {
public:
    DefaultClass() = default;
    ~DefaultClass() = default;
    DefaultClass(const DefaultClass &) = delete;
    DefaultClass &operator=(const DefaultClass &) = delete;
    friend std::ostream& operator << (std::ostream &out, const DefaultClass &defaultClass);
private:
    int _num ;
};
主函数中调用

DefaultClass b;
std::cout << b << std::endl;
输出num是一个随机数

DefaultClass num is 331
什么是移动构造函数？与拷贝构造函数有何不同？
回答要点：

定义：移动构造函数用于通过“移动”资源来初始化对象，而不是复制资源。

语法：使用右值引用作为参数 (MyClass(MyClass&& other)).

优势

：

性能更高，避免不必要的深拷贝。
适用于临时对象。
区别

：

拷贝构造函数复制资源，移动构造函数转移资源所有权。
示例：

class MyClass {
public:
    MyClass(MyClass&& other) noexcept { // 移动构造函数
        // 移动资源
    }
};
默认构造函数和用户定义的构造函数有什么区别？
回答要点：

默认构造函数

：

无参数的构造函数。
如果没有用户定义的构造函数，编译器会自动生成一个默认构造函数。
用户定义的构造函数

：

开发者自定义的构造函数，可以有参数。
一旦定义了任何构造函数，编译器不会再生成默认构造函数
示例：

class MyClass {
public:
    MyClass() { // 默认构造函数
        // 初始化代码
    }

    MyClass(int x) { // 有参数的构造函数
        // 初始化代码
    }
};
什么是初始化列表？为什么在构造函数中使用它？
回答要点：

定义：初始化列表是在构造函数的参数列表之后，函数体之前，用于初始化成员变量的语法。

优点

：

提高性能，特别是对于常量成员或引用成员。
必须用于初始化常量成员、引用成员以及基类。
避免对象先默认构造再赋值，减少不必要的操作。
示例：

class MyClass {
    string x;
    string y;
public:
    MyClass(string  a, string b):x(a),y(b)  { // 初始化列表
        // 其他初始化代码
    }
};
什么是虚析构函数？为什么需要它？
回答要点：

定义：在基类中将析构函数声明为virtual，以确保通过基类指针删除派生类对象时，能正确调用派生类的析构函数。

用途

：

防止内存泄漏。
确保派生类的资源被正确释放。
不使用虚析构函数的风险

：

仅调用基类析构函数，导致派生类资源未释放。
示例：

如果BaseA的析构不写成虚析构，则主函数开辟子类对象赋值给基类指针，以后delete基类指针的时候会发现没有析构子类

class BaseA{
public:
    BaseA(std::string name):_name(name){
        std::cout << "BaseA()" << std::endl;
    }

    ~BaseA(){
        std::cout << "~BaseA()" << std::endl;
    }

