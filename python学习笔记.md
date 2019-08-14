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
### 格式化
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

### list
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
### tuple
另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改，比如同样是列出同学的名字：

    classmates = ('Michael', 'Bob', 'Tracy') #声明元组
定义一个只有1个元素的tuple时必须加一个逗号,，来消除歧义：
```
t = (2,)
t
(1,) #Python在显示只有1个元素的tuple时，也会加一个逗号,，避免误解成数学计算意义上的括号。
```
**注意：tuple内元素值按引用传递**
### dict
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
> 
> 所以，dict是用空间来换取时间的一种方法。但是dict的key必须是不可变对象，因为dict根据key来计算value的存储位置

### set
set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。
```
s = set([1, 1, 2, 3]) #创建一个set，需要提供一个list作为输入集合
s
  {1, 2, 3}
s.add(4) #添加元素到set中
s.remove(4) #删除元素
```
### 空函数
```
def nop():
    pass
```
pass语句可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个pass，让代码能运行起来。
```
if age >= 18:
    pass
```