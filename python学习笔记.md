# python学习笔记
## 基础
### 基本操作
```
 #为注释

input() #输入

print('hello, world') #输出 hello, world

print('''line1
... line2
... line3''') #多行输出

#定义一个求绝对值的函数
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
```
#### 格式化
在Python中，采用的格式化方式和C语言是一致的，用%实现
```
'Hello, %s' % 'world'
  'Hello, world'
'Hi, %s, you have $%d.' % ('Michael', 1000000)
  'Hi, Michael, you have $1000000.'
```
%运算符就是用来格式化字符串的。有几个`%?`占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个`%?`，括号可以省略。对普通字符`%`则用`%%`来转义表示一个`%`

> 常见的占位符有：
>
> 占位符	|替换内容
> -----|-----
> %d	|整数
> %f	|浮点数
> %s	|字符串
> %x	|十六进制整数

### 数据类型
`None`为空值

Python的整数没有大小限制,浮点数也没有大小限制，但是超出一定范围就直接表示为`inf`（无限大）
#### 字符串
单个字符的编码，Python提供了ord()函数获取字符的整数表示，chr()函数把编码转换为对应的字符：
```
ord('A')
 65
ord('中')
 20013
chr(66)
 'B'
chr(25991)
 '文'
```

要计算str包含多少个字符，可以用len()函数，如果换成bytes，len()函数就计算字节数：
```
len('ABC')
  3
len(b'ABC')
  3
len(b'\xe4\xb8\xad\xe6\x96\x87')
  6
len('中文'.encode('utf-8'))
  6 #1个中文字符经过UTF-8编码后通常会占用3个字节
```

#### 列表
Python内置的一种数据类型是列表：list。list是一种有序的集合，可以随时添加和删除其中的元素。

```
classmates = ['Michael', 'Bob', 'Tracy'] #声明列表
classmates #调用列表
  ['Michael', 'Bob', 'Tracy']
len(classmates) #获取列表元素个数
  3
classmates.append('Adam') #往list中追加元素到末尾
classmates.insert(1, 'Jack') #把元素插入到指定的位置
classmates.pop() #删除list末尾的元素
classmates.pop(1) #删除指定位置元素
classmates[1] = 'Sarah' #把某个元素替换成别的元素（直接赋值）
```
**注意：list内元素值按值传递**

for循环用法（结合list）
```
sum = 0
for x in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
    sum = sum + x
print(sum)
```

Python提供一个range()函数，可以生成一个整数序列，再通过list()函数可以转换为list
```
list(range(5))
  [0, 1, 2, 3, 4]
```
#### 元组
另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改，比如同样是列出同学的名字：

    classmates = ('Michael', 'Bob', 'Tracy') #声明元组
定义一个只有1个元素的tuple时必须加一个逗号,，来消除歧义：
```
t = (2,)
t
(1,) #Python在显示只有1个元素的tuple时，也会加一个逗号,，避免误解成数学计算意义上的括号。
```
**注意：tuple内元素值按引用传递**
#### 字典
Python内置了字典：dict的支持，使用键-值（key-value）存储，具有极快的查找速度
```
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
d['Michael']
  95
```
> 和list比较，dict有以下几个特点：
> 
> - 查找和插入的速度极快，不会随着key的增加而变慢；
> - 需要占用大量的内存，内存浪费多。
> 而list相反：
> 
> - 查找和插入的时间随着元素的增加而增加；
> - 占用空间小，浪费内存很少。

所以，dict是用空间来换取时间的一种方法。但是dict的key必须是不可变对象，因为dict根据key来计算value的存储位置

#### 集合
set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。
```
s = set([1, 1, 2, 3]) #创建一个set，需要提供一个list作为输入集合
s
  {1, 2, 3}
s.add(4) #添加元素到set中
s.remove(4) #删除元素
```
### 函数

- 定义函数时，需要确定函数名和参数个数；
- 如果有必要，可以先对参数的数据类型做检查；
- 函数体内部可以用return随时返回函数结果；
- 函数执行完毕也没有return语句时，自动return None。
- 函数可以同时返回多个值，但其实就是一个tuple。

#### 空函数
```
def nop():
    pass
```
pass语句可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个pass，让代码能运行起来。
```
if age >= 18:
    pass
```

#### 默认参数
```
def enroll(name, gender, age=6, city='Beijing'):
#为age,city设置默认值
enroll('Bob', 'M', 7) #与默认参数不同时
enroll('Adam', 'M', city='Tianjin') #不按顺序提供部分默认参数时
```
#### 可变参数（传入的参数个数是可变的）
```
def calc(numbers): #参数个数不确定，将其作为一个list或tuple传入
calc([1, 2, 3]) #调用的时候，需要先组装出一个list或tuple

def calc(*numbers): #改为可变参数
calc(1, 2, 3) #简化调用方式
#在函数内部，参数numbers接收到的是一个tuple
```
#### 关键字参数
```
def person(name, age, **kw): #函数person除了必选参数name和age外，还接受关键字参数kw。
    print('name:', name, 'age:', age, 'other:', kw)

person('Adam', 45, gender='M', job='Engineer')
  name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'} #可以传入任意个数的关键字参数
```
#### 命名关键字参数

如果要限制关键字参数的名字，就可以用命名关键字参数。
```
def person(name, age, *, city, job): #只接收city和job作为关键字参数
    print(name, age, city, job)
```
和关键字参数`**kw`不同，命名关键字参数需要一个特殊分隔符`*`，`*`后面的参数被视为命名关键字参数。
```
person('Jack', 24, city='Beijing', job='Engineer') #命名关键字参数必须传入参数名
  Jack 24 Beijing Engineer
```
如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了：
```
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
#使用*args和**kw是Python的习惯写法
```
#### 递归
使用递归函数需要注意防止栈溢出。

在计算机中，函数调用是通过栈（stack）这种数据结构实现的，每当进入一个函数调用，栈就会加一层栈帧，每当函数返回，栈就会减一层栈帧。由于栈的大小不是无限的，所以，递归调用的次数过多，会导致栈溢出。

解决递归调用栈溢出的方法是通过**尾递归**优化。**尾递归**是指，在函数返回的时候，调用自身本身，并且，return语句不能包含表达式。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。
```
def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product) #仅返回函数本身
```
#### 切片
Python提供了切片（Slice）操作符，能大大简化需要经常取指定索引范围的操作。
```
L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
L[0:3]  #从索引0开始取，直到索引3为止，但不包括索引3
  ['Michael', 'Sarah', 'Tracy']
#如果第一个索引是0，还可以省略： L[:3]
```
#### 迭代
如果给定一个list或tuple，我们可以通过for循环来遍历这个list或tuple，这种遍历我们称为迭代（Iteration）。当我们使用for循环时，只要作用于一个可迭代对象，for循环就可以正常运行。
```
from collections import Iterable # 通过collections模块的Iterable类型判断对象是否可迭代
isinstance('abc', Iterable) # str是否可迭代
  True

for x, y in [(1, 1), (2, 4), (3, 9)]: # 同时引用两个变量
     print(x, y)
  1 1
  2 4
  3 9
```
#### 列表生成式
```
[x * x for x in range(1, 11)]
  [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
# 写列表生成式时，把要生成的元素x * x放到前面，后面跟for循环，就可以把list创建出来。

[x * x for x in range(1, 11) if x % 2 == 0]
  [4, 16, 36, 64, 100]
# for循环后面还可以加上if判断，如筛选出仅偶数的平方

[m + n for m in 'ABC' for n in 'XYZ']
  ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
# 还可以使用两层循环生成全排列
```
#### 生成器
[https://www.cnblogs.com/wj-1314/p/8490822.html](https://www.cnblogs.com/wj-1314/p/8490822.html)