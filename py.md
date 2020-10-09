变量前面加一个`*`和`**`的区别
一个`*`会将参数转变为数组, `**`会将参数转变为字典,如果格式不对则无法转换成功







open(filename, mode, encoding)
encoding='utf-8' 指定文件编码为utf-8,如果不设置此参数编码为系统默认编码



py class带括号与不带括号的区别

简单地说带括号是实例化,不带括号是别名
不带括号就相当于指向了此类
带括号相当于实例化





```python
if __name__ == '__main__'

# __name__ 是当前模块名，当模块被直接运行时模块名为 __main__  这句话的意思就是，当模块被直接运行时，以下代码块将被运行，当模块是被导入时，代码块不被运行
```



```python
# 单行注释
```

```python
'''
多行注释
3个单引号或者3个双引号都可以
'''
```









# str

```python
str.replace('', '') # 把参数1的内容替换为参数2, replace方法为str类的
```



# os 模块

```python
os.getcwd() # 获取当前的工作目录
```

```python
os.chdir() # 设置工作目录
```

```python
os.environ # 这个不能加括号,加括号会提示错误
```



# 变量

变量就是⼀个存储数据的的时候当前数据所在的内存地址的名字



大驼峰: 每个单词首字母大写, `MyName`

小驼峰: 除首字母外单词首字母大写, `myName`



```python
a = 'aaaaaaaaaaaaaaaaaa' 
# 定义一个变量
# 变量严格区分大小写
# 变量a 和 变量A 是两个变量
```

```python
print(a) # 打印一个变量
```



显示一个变量的内存地址

```python
print(id(a)) # 10进制
```



## 变量标识符

变量标识符命名规则是Python中定义各种名字的时候的统⼀规范

- 只能由数字, 字母, 下划线组成
- 不能以数字开头
- 不能使用关键字
- **严格区分大小写**





## 关键字

| **关键字** | **描述**                                       |
| ---------- | ---------------------------------------------- |
| and        | 逻辑运算符。                                   |
| as         | 创建别名。                                     |
| assert     | 用于调试。                                     |
| break      | 跳出循环。                                     |
| class      | 定义类。                                       |
| continue   | 继续循环的下一个迭代。                         |
| def        | 定义函数。                                     |
| del        | 删除对象。                                     |
| elif       | 在条件语句中使用，等同于 else if。             |
| else       | 用于条件语句。                                 |
| except     | 处理异常，发生异常时如何执行。                 |
| False      | 布尔值，比较运算的结果。                       |
| finally    | 处理异常，无论是否存在异常，都将执行一段代码。 |
| for        | 创建 for 循环。                                |
| from       | 导入模块的特定部分。                           |
| global     | 声明全局变量。                                 |
| if         | 写一个条件语句。                               |
| import     | 导入模块。                                     |
| in         | 检查列表、元组等集合中是否存在某个值。         |
| is         | 测试两个变量是否相等。                         |
| lambda     | 创建匿名函数。                                 |
| None       | 表示 null 值。                                 |
| nonlocal   | 声明非局部变量。                               |
| not        | 逻辑运算符。                                   |
| or         | 逻辑运算符。                                   |
| pass       | null 语句，一条什么都不做的语句。              |
| raise      | 产生异常。                                     |
| return     | 退出函数并返回值。                             |
| True       | 布尔值，比较运算的结果。                       |
| try        | 编写 try...except 语句。                       |
| while      | 创建 while 循环。                              |
| with       | 用于简化异常处理。                             |
| yield      | 结束函数，返回生成器。                         |

 

## 类型

- str, 字符串

  ```python
  a = 'a'
  # 单引号和双引号没有区别
  ```

  

- list, 列表

  ```python
  a = [1, 2, 3]
  ```

  序列中的每个值都有对应的位置值，称之为索引，第一个索引是 0，第二个索引是 1，依此类推

  

- tuple, 元组

  ```python
  a = (1, 2, 3)
  ```

  元组与列表类似，不同之处在于元组的元素不能修改
  
  元组和列表, 下表都是从 `0` 开始
  
  
  
- set, 集合

  ```python
  a = {1, 2, 3}
  ```

  ```python
  a = set()
  ```

  集合（set）是一个无序的不重复元素序列。

  可以使用大括号 **{ }** 或者 **set()** 函数创建集合，注意：创建一个空集合必须用 **set()** 而不是 **{ }**，因为 **{ }** 是用来创建一个空字典。

  

- dict, 字典

  ```python
  a = {'a':1, 'b':2, 'c':3}
  # 键值对的键一定要用引号括起来, 否则python会认为键是一个变量
  ```

  字典是另一种可变容器模型，且可存储任意类型对象

  字典的每个键值对用冒号 **`:`** 分割，每个对之间用逗号(**`,`**)分割，整个字典包括在花括号 **{}** 中

  键必须是唯一的，但值则不必

  值可以取任何数据类型，但键必须是不可变的，如字符串，数字

  值的类型不可以是**列表**, **字典**

  ```python
  a = {'a': 'a', 1:1}
  ```

  ```python
  list = [1, 2, 3]
  
  dict = {list:1}
  '''
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  TypeError: unhashable type: 'list'
  '''
  ```

  ```python
  dict = {1:1}
  
  a = {dict:1}
  '''
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  TypeError: unhashable type: 'dict'
  '''
  ```

  

  

- 数值

  int, 整数, integer

  float, 浮点

- 布尔型

  True, 真

  False, 假



bytes和str类型的区别

Python 3最重要的新特性之一是对字符串和二进制数据流做了明确的区分。文本总是Unicode，由str类型表示，二进制数据则由bytes类型表示。Python 3不会以任意隐式的方式混用str和bytes，你不能拼接字符串和字节流，也无法在字节流里搜索字符串（反之亦然），也不能将字符串传入参数为字节流的函数（反之亦然）。
在将字符串存入磁盘和从磁盘读取字符串的过程中，Python自动地帮你完成了编码和解码的工作，你不需要关心它的过程。
使用bytes类型，实质上是告诉Python，不需要它帮你自动地完成编码和解码的工作，而是用户自己手动进行，并指定编码格式。
Python已经严格区分了bytes和str两种数据类型，你不能在需要bytes类型参数的时候使用str参数，反之亦然。这点在读写磁盘文件时容易碰到
从str到bytes，是编码,   比特流=str(串,encoding='utf-8')
从bytes到str，是解码，串=bytes(比特流,encoding='utf-8')



## 数据类型转换

| **函 数**              | **作 用**                                          |
| ---------------------- | -------------------------------------------------- |
| int(x)                 | 将 x 转换成整数类型                                |
| float(x)               | 将 x 转换成浮点数类型                              |
| complex(real，[,imag]) | 创建一个复数                                       |
| str(x)                 | 将 x 转换为字符串                                  |
| repr(x)                | 将 x 转换为表达式字符串                            |
| eval(str)              | 计算在字符串中的有效 Python 表达式，并返回一个对象 |
| chr(x)                 | 将整数 x 转换为一个字符                            |
| ord(x)                 | 将一个字符 x 转换为它对应的整数值                  |
| hex(x)                 | 将一个整数 x 转换为一个十六进制字符串              |
| oct(x)                 | 将一个整数 x 转换为一个八进制的字符串              |

 



# 转义符

| **转义字符** | **说明**                                                     |
| ------------ | ------------------------------------------------------------ |
| \n           | 换行符，将光标位置移到下一行开头。                           |
| \r           | 回车符，将光标位置移到本行开头。                             |
| \t           | 水平制表符，也即 Tab 键，一般相当于四个空格。                |
| \a           | 蜂鸣器响铃。注意不是喇叭发声，现在的计算机很多都不带蜂鸣器了，所以响铃不一定有效。 |
| \b           | 退格（Backspace），将光标位置移到前一列。                    |
| \\           | 反斜线                                                       |
| \'           | 单引号                                                       |
| \"           | 双引号                                                       |
| \            | 在字符串行尾的续行符，即一行未完，转到下一行继续写。         |





# 内置函数

| abs()         | divmod()    | input()      | open()              | staticmethod()  |
| ------------- | ----------- | ------------ | ------------------- | --------------- |
| all()         | enumerate() | int()        | ord()               | str()           |
| any()         | eval()      | isinstance() | pow()               | sum()           |
| basestring()  | execfile()  | issubclass() | [print()](#print()) | super()         |
| bin()         | file()      | iter()       | property()          | tuple()         |
| bool()        | filter()    | len()        | range()             | type()          |
| bytearray()   | float()     | list()       | raw_input()         | unichr()        |
| callable()    | format()    | locals()     | reduce()            | unicode()       |
| chr()         | frozenset() | long()       | reload()            | vars()          |
| classmethod() | getattr()   | map()        | repr()              | xrange()        |
| cmp()         | globals()   | max()        | reverse()           | zip()           |
| compile()     | hasattr()   | memoryview() | round()             | import()        |
| complex()     | hash()      | min()        | set()               |                 |
| delattr()     | help()      | next()       | setattr()           |                 |
| dict()        | hex()       | object()     | slice()             |                 |
| dir()         | id()        | oct()        | sorted()            | exec 内置表达式 |

 

## print()

```python
print('%s' % 'hello world') # hello world
```

- 转换说明符（Conversion Specifier）只是一个占位符，它会被后面表达式（变量、常量、数字、字符串、加减乘除等各种形式）的值代替
- 在 print() 函数中，由引号包围的是格式化字符串，它相当于一个字符串模板，可以放置一些转换说明符（占位符）。本例的格式化字符串中包含一个`%s`说明符，它最终会被后面的值所替代
- 中间的`%`是一个分隔符，它前面是格式化字符串，后面是要输出的表达式



```python
a = 'aaa'
b = 'bbb'
c = 'ccc'

print('%s\t%s\t%s' % (a, b, c))
# aaa     bbb     ccc
```

- 格式化字符串中也可以包含多个转换说明符，这个时候也得提供多个表达式，用以替换对应的转换说明符；多个表达式必须使用小括号`( )`包围起来



|格式符| 描述|
| ---- | ---- |
|[%s](#%s)| 字符串 (采用str()的显示)|
|%r| 字符串 (采用repr()的显示)|
|%c| 单个字符及其ASCII码|
|%u| 整数(无符号)|
|%b| 二进制整数|
|%o| 八进制数(无符号)|
|[%d](#%d)| 十进制整数|
|%i| 十进制整数|
|%x| 十六进制数(无符号)|
|%X| 十六进制数大写(无符号)|
|%e| 指数 (基底写为e)，用科学计数法格式化浮点数|
|%E| 指数 (基底写为E)，作用同%e|
|[%f](#%f)| 浮点数，可指定小数点后的精度|
|[%g](#%g)| %f和%e的简写，指数(e)或浮点数 (根据显示长度)|
|%G| %F和%E的简写，指数(E)或浮点数 (根据显示长度)|
|%p| 用十六进制数格式化变量的地址|
|%%| 转义,字符"%"|



### %s

- %5s——右对齐，占位符5位

  ```python
  print('%1s' % 'abc')
  # |abc
  
  print('%3s' % 'abc')
  # |abc
  
  print('%5s' % 'abc')
  # |  abc(多出两个空格)
  ```

  默认情况下，print() 输出的数据总是右对齐的。也就是说，当数据不够宽时，数据总是靠右边输出，而在左边补充空格以达到指定的宽度

  

- %-5s——左对齐，占位符5位

  ```python
  print('%-1s' % 'abc')
  # |abc
  
  print('%-3s' % 'abc')
  # |abc
  
  print('%-5s' % 'abc')
  # |abc  |(多出两个空格)
  ```

  

- %.2s——截取2位字符串

  ```python
  print('%.1s' % 'abc')
  # a
  
  print('%.2s' % 'abc')
  # ab
  
  print('%.3s' % 'abc')
  # abc
  
  print('%.15s' % 'abc')
  # abc
  ```

  

- %5.2s——5位占位符，截取两位字符串

  ```python
  print('%5.2s' % 'abc')
  # |   ab(3个空格)
  ```




### %f

```python
print('%f' % 1.11)
# 1.110000
# 默认保留6位小数
```



- 取1位小数

  ```python
  print('%.1f' % 1.11)
  # 1.1
  ```



### %g

```bash
print('%g' % 1111.1111)
# 1111.11
# 默认6位有效数字
```



### %d

```python
print('%05d' % 123)
# 00123

print('%015d' % 123)
# 000000000000123
```





### .format








# iso

```python
import os, urllib.request

url = [
    ['https://mirrors.ustc.edu.cn/centos/8.2.2004/isos/x86_64/CentOS-8.2.2004-x86_64-dvd1.iso', 'centos', 'ustc'],
    ['https://mirrors.ustc.edu.cn/centos/8.2.2004/isos/x86_64/CentOS-8.2.2004-x86_64-minimal.iso', 'centos-mini', 'ustc'],
    ['https://mirrors.ustc.edu.cn/fedora/releases/32/Server/x86_64/iso/Fedora-Server-netinst-x86_64-32-1.6.iso', 'fedora-server', 'ustc'],
    ['https://mirrors.ustc.edu.cn/ubuntu-releases/20.04/ubuntu-20.04-desktop-amd64.iso', 'ubuntu', 'ustc'],
    ['http://www.tinycorelinux.net/11.x/x86/release/Core-11.1.iso', 'tinycore', 'ustc'],
    ['https://download.freebsd.org/ftp/snapshots/amd64/amd64/ISO-IMAGES/12.1/FreeBSD-12.1-STABLE-amd64-20200709-r363017-disc1.iso', 'freebsd', ''],
    ['https://cdn.openbsd.org/pub/OpenBSD/6.7/amd64/install67.iso', 'openbsd', ''],
    ['https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.0/images/NetBSD-9.0-amd64.iso', 'netbsd', '']
]

if not os.path.exists('./iso'):
    os.mkdir('./iso', 0o755)
    '''py3 数字前面不允许0开头
    ob  2进制
    0x  16进制
    0o  8进制'''

for i in url:
    urllib.request.urlretrieve(i[0], './' + os.path.basename(i[0]))
```

