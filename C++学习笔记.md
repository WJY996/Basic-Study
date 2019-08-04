# C++学习笔记

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

    double pam(int);                         //声明函数
    double (*pf)(int);                       //声明指向函数的指针
    pf = pam;//pf指向pam()函数
    double x = pam(5);                       //通过函数名调用pam函数
    double y = (*pf)(4);                     //通过指针pf调用pam函数
    double z = pf(6);                        //C++允许这种情况
    void estimate(int i, double (*pf)(int))  //在其他函数中调用

### 内联函数

省略原型，将整个定义（即函数头及所有函数代码)放在原本应提供原型之处。

    inline double square(double x) {return x *x;}

### 引用变量

#### 创建引用变量
    int rats;
    int & rodents = rats;            //int * const pr = &rats;
    // &rodents == &rats

####将引用用作函数参数
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