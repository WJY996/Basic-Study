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