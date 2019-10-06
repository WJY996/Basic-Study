# C++操作

## 运算符
### 除法 /
**整数除法**用`/`的话得到的是一个**整数**（得到小数的话自动去掉小数位只保留整数位），所以这里要得到实际除出来的数的话，**先将两个数转化为`double`类型**，再进行`/`除法。至于要规定输出保留多少位小数，则用`cout<<setprecision(2)<<fixed<<……;`其中`2`表示保留多少位小数（2表示两位）。同时要注意`seprecision`函数的使用要搭配`<iomanip>`头文件。

> 关于<iomanip>头文件：
> ```C++
> setw(int);//设置显示宽度。 
> left//right//设置左右对齐。 
> setprecision(int);//设置浮点数的精确度。
> ```

### 作用域解析运算符（::）
指出函数所属的类

    void Stock::update(double price)
    // update()函数是Stock类的成员

## 关键字
### 说明符
1. `auto`： 在C++11中不再是说明符。在C++11前，auto指出变量为自动变量，但在C++11后，`auto`用于自动类型推导。
2. `register`：用于声明中指示寄存器存储，但在C++11中，它只是显示地指出变量是自动的。
3. `static`：静态声明。用在作用域为整个文件的声明中时，表示内部链接行；用于局部声明中，表示无链接性。
4. `extern`：引用声明，即声明引用其他地方定义的变量。
5. `thread_local`：C++11新增。该变量的生命周期与其所属线程的生命周期一致。`thread_local`变量之于线程，犹如常规静态变量之于整个程序。
6. `mutable`：表面变量是可更改的。即使结构（类）或变量为`const`类型，其成员也是可以被修改的。

### 限定符
1. const：常量声明。const类型变量的连接性为内部的，即所有的文件可以声明相同的const变量。但全局变量默认是外部的。
2. volatile：表面即使程序代码没有对内存单元做修改，但其值也可能发生了改变。该关键字表面不要对该变量进行优化，程序取值时会去内存中取值。
3. mutable：指出即使结构（或类）变量为const，其某个成员也可以被修改。

#### volatile关键字
`volatile`关键字是一种类型修饰符，用它声明的类型变量表示可以被某些编译器未知的因素更改，比如：操作系统，硬件或者其他线程等。遇到这个关键字声明的变量，编译器对访问该变量的代码就不再进行优化，从而可以提供对特殊地址的稳定访问。**当要求使用`volatile`声明的变量的值的时候，系统总是重新从它所在的内存读取数据，即使它前面的指令刚刚从该处读取过数据。而且读取的数据立刻被保存。**

```C++
volatile int i=10;
int a = i;
...
// 其他代码，并未明确告诉编译器，对 i 进行过操作
int b = i;
```

`volatile`指出`i`是随时可能发生变化的，每次使用它的时候必须从`i`的地址中读取，因而编译器生成的汇编代码会重新从`i`的地址读取数据放在 b 中。而优化做法是，由于编译器发现两次从`i`读数据的代码之间的代码没有对`i`进行过操作，它会自动把上次读的数据放在`b`中。而不是重新从`i`里面读。这样以来，如果`i`是一个寄存器变量或者表示一个端口数据就容易出错，所以说`volatile`可以**保证对特殊地址的稳定访问**。

按照C++标准，对于`glvalue`的`volatile`变量进行操作，与其他输入输出一样，顺序和内容都是不能改变的。这个结果就像是把对`volatile`的操作看做程序外部的输入输出一样。（`glvalue`是值类别的一种，简单说就是内存上分配有空间的对象）

同时，这是C++标准中`volatile`唯一的功能，但是在一些编译器（如，MSVC）中，`volatile`还有线程同步的功能，但这就是编译器自己的拓展了，并不能跨平台应用。


## 数组

### C++将数组名参数视为数组第一个元素的地址
    //以下两个声明等价
    typename arr[];
    typename * arr;

## 函数

### 使用函数的三个步骤
提供函数原型、函数定义，并调用函数。

### 递归
    void countdown(int n)
    {
        cout << "Counting down ... " << n << endl;
        if (n > 0)
            countdown(n-1);
        cout << n << ":Kaboom!\n";
    }

### 函数指针
与直接调用函数相比，允许在不同的时间传递不同函数的地址，意味着可以在不同的时间使用不同的函数，允许每个程序员提供自己的算法。
    
    double *p(int);                          //声明返回指针的函数
    double pam(int);                         //声明函数
    double (*pf)(int);                       //声明指向函数的指针
    pf = pam;                                //pf指向pam()函数
    double x = pam(5);                       //通过函数名调用pam函数
    double y = (*pf)(4);                     //通过指针pf调用pam函数
    double z = pf(6);                        //C++允许这种情况
    void estimate(int i, double (*pf)(int))  //在其他函数中调用

### 内联函数

`inline`是C++语言中的一个关键字，可以用于程序中定义内联函数。**内联函数**是C++中的一种特殊函数，它可以像普通函数一样被调用，但是在调用时并不通过函数调用的机制而是通过将函数体直接插入调用处来实现的，这样可以大大减少由函数调用带来的开销，从而提高程序的运行效率。一般来说`inline`用于定义类的成员函数。
```C++
inline double square(double x)
{return x *x;}
//省略原型，将整个定义（即函数头及所有函数代码)放在原本应提供原型之处。
```
 > 优点：
 > 
> - inline 定义的类的内联函数，函数的代码被放入符号表中，在使用时直接进行替换，（像宏一样展开），没有了调用的开销，效率也很高。
> - 编译器在调用一个内联函数时，会首先检查它的参数的类型，保证调用正确。然后进行一系列的相关检查，就像对待任何一个真正的函数一样。这样就消除了它的隐患和局限性。（宏替换不会检查参数类型，安全隐患较大）
> - inline函数可以作为一个类的成员函数，与类的普通成员函数作用相同，可以访问一个类的私有成员和保护成员。内联函数可以用于替代一般的宏定义，最重要的应用在于类的存取函数的定义上面。
> 
 >   缺点：
 >  
> - 内联函数不能包括复杂的控制语句，如循环语句和switch语句；   
> - 内联函数具有一定的局限性，内联函数的函数体一般来说不能太大。`inline`说明对编译器来说只是一种建议，编译器可以选择忽略这个建议。如果内联函数的函数体过大，一般的编译器会放弃内联方式，而采用普通的方式调用函数。

### 引用变量

#### 创建引用变量
    int rats;
    int & rodents = rats;            //int * const pr = &rats;
    // &rodents == &rats

#### 将引用用作函数参数
    //按值传递
    void sneezy(int x)
    //按引用传递
    void crumpy(int &x)
    //传递指针
    void swap(int * x)

#### 应尽可能将引用形参声明为const
- 避免无意中修改数据
- 能够处理const和非const实参
- 使函数能够正确生成并使用临时变量

#### 返回引用变量
返回引用的函数实际上是被引用的变量的别名

    //传统返回机制（按值传递）
    cout << sqrt(25);
    //等效于
    temp=sqrt(25)=5;
    cout << temp;

#### 将引用用于结构
    struct A
    {
        string name;
        int point;
    }

    A & function(a & one, a & two)
    {
        one.point += two.point;
        return one;
    }
使用引用变量避免将整个结构复制到一个临时位置

### 函数重载
```C++
Void dribble(char * bits);            //重载
Void dribble (const char *cbits);     //重载
Void dabble(char * bits);             //非重载
Void drivel(const char * bits);       //非重载

Const char p1[20]="How’s the weather?";
Char p2[20]="How"s business?"；
Dribble(p1);               // dribble(const char *); 
Dribble(p2);               // dribble(char *)；
Dabble(p1);                // no match 
Dabble(p2);                // dabble(char *);
Drivel(p1);                // drivel(const char *)；
Drivel(p2);                // drivel(const char *);
```
`dribble()`函数有两个原型，一个用于`const`指针，另一个用于常规指针，编译器将根据实参是否为`const`来决定使用哪个原型。`dribble()`函数只与带非`const`参数的调用匹配，而`drivel()`函数可以与带`const`或非`const`参数的调用匹配。`drivel()`和`dabble()`之所以在行为上有这种差别，主要是由于将非`const`值赋给`const`变量是合法的，但反之则是非法的。

### 函数模板
```C++
template <typename T>    // T = AnyType
void Swap(T &a, T &b)    // 用引用实现值交换
{
    T tmp = a;
    a = b;
    b = tmp;
}

// 调用
int a = 10;
int b = 20;
Swap(a, b);	         // 自动推到调用
Swap<int>(a, b);         // 显示指定调用
```

#### 显式具体化
```C++
struct job
{
    char name[20];
    int salary;
};


template <> void Swap<job>(job &a, job &b)
{
     int salary;
     salary = a.salary;
     a.salary = b.salary;
     b.salary = salary;
}
```

#### 模板类
C++ 中类模板的写法如下：
```C++
template <类型参数表>
class 类模板名{
    成员函数和成员变量
};
```
类型参数表的写法如下：
    class类塑参数1, class类型参数2, ...

类模板中的成员函数放到类模板定义外面写时的语法如下：
```C++
template <类型参数表>
返回值类型  类模板名<类型参数名列表>::成员函数名(参数表)
{
    ...
}
```
用类模板定义对象的写法如下：
`类模板名<真实类型参数表> 对象名(构造函数实际参数表);`

如果类模板有无参构造函数，那么也可以使用如下写法：
`类模板名 <真实类型参数表> 对象名;`

类模板看上去很像一个类。下面以`Pair`类模板为例来说明类模板的写法和用法。

> 实践中常常会碰到，某项数据记录由两部分组成，一部分是关键字，另一部分是值。关键字用来对记录进行排序和检索，根据关键字能查到值。例如，学生记录由两部分组成，一部分是学号，另一部分是绩点。要能根据学号对学生进行排序，以便方便地检索绩点，则学号就是关键字，绩点就是值。

下面的`Pair`类模板就可用来处理这样的数据记录：
```C++
template <class T1,class T2>
class Pair
{
public:
    T1 key;    // 关键字
    T2 value;  // 值
    Pair(T1 k,T2 v):key(k),value(v) { };
    bool operator < (const Pair<T1,T2> & p) const;
};

template<class T1,class T2>
bool Pair<T1,T2>::operator < (const Pair<T1,T2> & p) const
               // Pair的成员函数 operator <
{              // "小"的意思就是关键字小
    return key < p.key;
}

int main()
{
    Pair<string,int> student("Tom",19); // 实例化出一个类 Pair<string,int>
    cout << student.key << " " << student.value;
    return 0;
}
```
程序的输出结果是：

`Tom 19`

#### 类型问题
模板在使用时，可能会出现不知道应该声明是什么类型的状况。
```C++
template<typename T1,typename t2>
void  fun(T1 a,T2 b)
{
    ?type? aplusb=a+b;
}
```
在上叙情况中，我们事先并**不知道`aplusb`的类型，无法对其进行声明。** C++11中新增加的关键字`decltype`解决了这个问题。
```C++
int x;
decltype(x) y;
```
这使得`y`的类型与`x`相同，`decltype`可以是表达式，函数调用等等
```C++
int fun1(a){return a;}
decltype (fun1) x;     //令x类型与fun1的返回类型相同

int x;
double y;
decltype(x+y) xpy;     //令xpy类型与x+y相同
```
##### C++后置返回类型
```C++
template<typename T1,typename t2>
 ?type? fun(T1 a,T2 b)
{
    return a+b;
}
```
由于在提供返回类型之前，还未声明变量`a`,`b`所以无法对返回类型设置为`decltype(a,b)`，在C++11中提供了一个解决方案，**后置返回类型**。
```C++
template<typename T1,typename t2>
auto fun(T1 a,T2 b) -> decltype(a+b)
{
    return a+b;
}
```
这个函数模板的返回类型即为`decltype(a+b)`，在实际的泛型编程中这非常有用，如果一开始未知要返回什么类型，先设置返回类型为`auto`再接着`->type(expression)`，就可以解决很大一部分的问题。

### 断言
断言函数`assert()`宏的原型定义在`<assert.h>`中，其作用是如果它的条件返回错误，则终止程序执行。

```C++
#include <assert.h>
void assert( int expression );
```
`assert`的作用是先计算表达式`expression`，如果其值为假（即为0），那么它先向`stderr`打印一条出错信息，然后通过调用`abort`来终止程序运行。

`assert`是用来避免显而易见的错误的，而不是处理异常的。错误和异常是不一样的，错误是不应该出现的，异常是不可避免的。c语言异常可以通过条件判断来处理，其它语言有各自的异常处理机制。

一个非常简单的使用`assert`的规律就是，在方法或者函数的最开始使用，如果在方法的中间使用则需要慎重考虑是否是应该的。方法的最开始还没开始一个功能过程，在一个功能过程执行中出现的问题几乎都是异常。

**用法举例：在函数开始处检验传入参数的合法性**
```C++
int resetBufferSize(int nNewSize) 
{ 
//功能:改变缓冲区大小, 
//参数:nNewSize 缓冲区新长度 
//返回值:缓冲区当前长度 
//说明:保持原信息内容不变 nNewSize<=0表示清除缓冲区 
assert(nNewSize >= 0); 
assert(nNewSize <= MAX_BUFFER_SIZE); 
}
```

### 友元
友元有三种：

- 友元函数
- 友元类
- 友元成员函数

通过让函数成为类的友元，可以赋予该函数与类的成员函数相同的访问权限，实现对类对象私有部分的访问。

创建友元函数的第一步是将其原型放在类声明中，并在原型声明前加上关键字`friend`

    friend Time operator*(double m, const Time & t);

第二步是编写函数定义，因其不是类成员函数，**不要使用`Time::`限定符**，同时亦**不要用关键字`friend`**
```
Time operator*(double m, const Time & t)
{
    Time result;
    long totalminutes = t.hours * mult *60 + t.minutes *mult;
    result.hours = total.minutes / 60;
    result.minutes = totalminutea % 60;
    return result;
}
```
只有类声明能决定哪一个函数是友元，因此类声明仍然控制了哪些函数可以访问私有数据。类方法和友元只是表达类接口的两种不同机制。

### 复制构造函数
复制构造函数用于将一个对象复制到新创建的对象中。它是用于初始化过程中（包括按值传递对象，函数返回对象），而不是常规的赋值过程中。

    Class_name(const Class_name &);//类的复制构造函数原型
默认的复制构造函数逐个复制非静态成员（成员复制也称为**浅复制**），复制的是成员的值。

有些情况下一些类成员是使用`new`初始化的、指向数据的指针，而不是数据本身，因此必须要定义复制构造函数，进行**深度复制**。