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
    pf = pam;//pf指向pam()函数
    double x = pam(5);                       //通过函数名调用pam函数
    double y = (*pf)(4);                     //通过指针pf调用pam函数
    double z = pf(6);                        //C++允许这种情况
    void estimate(int i, double (*pf)(int))  //在其他函数中调用

### 内联函数

`inline`是C++语言中的一个关键字，可以用于程序中定义内联函数。**内联函数**是C++中的一种特殊函数，它可以像普通函数一样被调用，但是在调用时并不通过函数调用的机制而是通过将函数体直接插入调用处来实现的，这样可以大大减少由函数调用带来的开销，从而提高程序的运行效率。一般来说inline用于定义类的成员函数。
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
Swap(a, b);	             // 自动推到调用
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

#### 类型问题
模板在使用时，可能会出现不知道应该声明是什么类型的状况。
```C++
template<typename T1,typename t2>
void  fun(T1 a,T2 b)
{
    ?type? aplusb=a+b;
}
```
在上叙情况中，我们事先并**不知道`aplusb`的类型，无法对其进行声明。**C++11中新增加的关键字`decltype`解决了这个问题
```C++
int x;
decltype(x) y;
```
这使得`y`的类型与`x`相同，`decltype`可以是表达式，函数调用等等
例如
```C++
int fun1(a){return a;}
decltype (fun1) x;//令x类型与fun1的返回类型相同

int x;
double y;
decltype(x+y) xpy;//令xpy类型与x+y相同
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
这个函数模板的返回类型即为`decltype(a+b)`

这在实际的泛型编程中非常有用，如果一开始未知要返回什么类型，先设置返回类型为`auto`再在后面`->type(expression)`，这就可以解决很大一部分的问题。


