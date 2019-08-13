# python学习笔记
## 基本操作
```
input()//输入

print('hello, world')//输出 hello, world

 #为注释

print('''line1
... line2
... line3''')//注释
```

`None`为空值

Python的整数没有大小限制,浮点数也没有大小限制，但是超出一定范围就直接表示为`inf`（无限大）

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