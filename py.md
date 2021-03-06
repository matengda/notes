# PEP

Python 官方提供有一系列 PEP（Python Enhancement Proposals） 文档

其中第 8 篇文档专门针对 Python 的代码格式 给出了建议，也就是俗称的 PEP 8

文档地址：<https://www.python.org/dev/peps/pep-0008/>

谷歌有对应的中文文档：<http://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/>



# class

py class带括号与不带括号的区别

简单地说带括号是实例化,不带括号是别名
不带括号就相当于指向了此类
带括号相当于实例化



# str.class

**在python3中字符总是 `Unicode`，由 `str `类型表示，**

Python中的字符串用单引号 **`'`** 或双引号 **`"`** 括起来，同时使用反斜杠**`\`**转义特殊字符

索引值以 0 为开始值，-1 为从末尾的开始位置



## %-formatting

**%**格式化字符串是python最早的, 也是能兼容所有版本的一种字符串格式化方法, **%**会把字符串中的格式化符按顺序后面参数替换



| 格式符    | 描述                                         |
| --------- | -------------------------------------------- |
| [%s](#%s) | 字符串 (采用str()的显示)                     |
| %r        | 字符串 (采用repr()的显示)                    |
| %c        | 单个字符及其ASCII码                          |
| %u        | 整数(无符号)                                 |
| %b        | 二进制整数                                   |
| %o        | 八进制数(无符号)                             |
| [%d](#%d) | 十进制整数                                   |
| %i        | 十进制整数                                   |
| %x        | 十六进制数(无符号)                           |
| %X        | 十六进制数大写(无符号)                       |
| %e        | 指数 (基底写为e)，用科学计数法格式化浮点数   |
| %E        | 指数 (基底写为E)，作用同%e                   |
| [%f](#%f) | 浮点数，可指定小数点后的精度                 |
| [%g](#%g) | %f和%e的简写，指数(e)或浮点数 (根据显示长度) |
| %G        | %F和%E的简写，指数(E)或浮点数 (根据显示长度) |
| %p        | 用十六进制数格式化变量的地址                 |
| %%        | 转义,字符"%"                                 |



### %s

```python
print('%s' % 'hello world')
# hello world
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

  

%5s    右对齐，占位符5位

  ```python
  print('%1s' % 'abc')
  # |abc
  
  print('%3s' % 'abc')
  # |abc
  
  print('%5s' % 'abc')
  # |  abc(多出两个空格)
  ```

默认情况下，print() 输出的数据总是右对齐的。也就是说，当数据不够宽时，数据总是靠右边输出，而在左边补充空格以达到指定的宽度

  

%-5s    左对齐，占位符5位

  ```python
  print('%-1s' % 'abc')
  # |abc
  
  print('%-3s' % 'abc')
  # |abc
  
  print('%-5s' % 'abc')
  # |abc  |(多出两个空格)
  ```

  

%.2s    截取2位字符串

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

  

%5.2s    5位占位符，截取两位字符串

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

```python
print('%.1f' % 1.11)
# 1.1
# 取1位小数
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





## .format()

Python2.6 开始，新增了一种格式化字符串的函数 **str.format()**，它增强了字符串格式化的功能。

基本语法是通过 **{}** 和 **:** 来代替以前的 **%** 。

format 函数可以接受不限个参数，位置可以不按顺序。

**^**, **<**, **>** 分别是居中、左对齐、右对齐，后面带宽度， **:** 号后面带填充的字符，只能是一个字符，不指定则默认是用空格填充。

**+** 表示在正数前显示 **+**，负数前显示 **-**； （空格）表示在正数前加空格

b、d、o、x 分别是二进制、十进制、八进制、十六进制



```python
'{} {}'.format('a', 'b')
# 'a b'
# 不带参数，即{}
# 不设置指定位置，按默认顺序
```

```python
'{}{}'.format('a', 'b') # format('a','b')
# 'ab'
# 参数里面没空格, 输出也就没有空格
```

```python
'{1} {0}'.format('a', 'b')
# 'b a'
# 带数字参数, 可调换顺序, 及'{1} {0}'
```

```python
'{a} {b} {c}'.format(a='aaa', b='bbb', c='ccc')
# 'aaa bbb ccc'
# 带关键字, 即{a}, {b}, {c}
```

```python
a = [1, 2]

'i:{0[0]}, j:{0[1]}'.format(a)
# 'i:1, j:2'
# 通过索引访问
```

```python
a = {'a': 'aaa', 'b': 'bbb'}

'i:{0[a]}, j:{0[b]}'.format(a)
# 'i:aaa, j:bbb'
# 通过key访问
```



### 格式转换

| 符号    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| **b**   | 2进制                                                        |
| **c**   | 字符, 将整数转换成对应的Unicode字符串                        |
| **d**   | 10进制                                                       |
| **o**   | 8进制                                                        |
| **x/X** | 16进制, 小写, 大写                                           |
| **e**   | 幂符号, 用科学计数法打印数字, 用'e'表示幂                    |
| **g**   | 一般格式, 当数值特别大的时候, 用幂形式打印                   |
| **n**   | 数字, 当值为整数时和'd'相同, 值为浮点数时和'g'相同, 不同的是它会根据区域设置插入数字分隔符 |
| **%**   | 百分数, 将数值乘以100然后以fixed-point('f')格式打印, 值后面会有一个百分号 |



```python
'{:b}'.format(255)
# '11111111'
# 如果不指定位置默认是第一个参数
# '{0:b}'.format(255) 等效
```

```python
'{1:b}'.format(15, 0xff)
# '11111111'
# 选择输出第2个参数, 也就是0xff
```

```python
'{:#X}'.format(255)
# '0XFF'
```

```python
'{:c}'.format(65)
# A
```

```python
'{:0<5d}'.format(1)
# '10000'
```

```python
'{:0>5d}'.format(1)
# '00001'
```

```python
a = 1
b = 2
'{:%}'.format(b/a)
# '200.000000%'
```

```python
a = 1
b = 2
'{:.2%}'.format(b/a)
# '200.00%'
# 保留小数点后两位
```







### 对齐方式

| 符号  |              描述              |
| :---: | :----------------------------: |
| **<** |          左对齐(默认)          |
| **>** |             右对齐             |
| **^** |            居中对齐            |
| **=** | 在小数点后进行补齐(只用于数字) |



```python
'{:15s}{}'.format('aaa', 'bbb')
# 'aaa            bbb'
```

```python
'{:5s}{}'.format('aaa', 'bbb')
# 'aaa  bbb'
# aaa后面2两个空格
# 默认左对齐
# '{:<5s}{}'.format('aaa', 'bbb') 等效
```

```python
'{:>5s}{}'.format('aaa', 'bbb')
# '  aaabbb'
# 右对齐, aaa前面2个空格
```

```python
'{:^15s}{}'.format('aaa', 'bbb')
#  '      aaa      bbb'
# 居中对齐
# aaa左边6个空格, 右边6个空格, 15-3=12 12/2=6
```

```python
'{:^16s}{}'.format('aaa', 'bbb')
# '      aaa       bbb'
# aaa左边6个空格, 右边7个空格
```



### 正负符号显示

```python
'{:+f}    {:+f}'.format(3.14, -3.14)
# '+3.140000    -3.140000'
```

```python
'{:-f}    {:-f}'.format(3.14, -3.14)
# '3.140000    -3.140000'
```

```python
'{: f}    {: f}'.format(3.14, -3.14)
# ' 3.140000    -3.140000'
# 若是+数，则在前面保留1个空格
```





### 千位分隔符

```python
'{:,}'.format(123456789)
# '123,456,789'
```

```python
'{:_}'.format(123456789)
# '123_456_789'
```









## f-string

f-string，亦称为格式化字符串常量（formatted string literals），是Python3.6新引入的一种字符串格式化方法，该方法源于PEP 498 – Literal String Interpolation，主要目的是使格式化字符串的操作更加简便。f-string在形式上是以 f 或 F 修饰符引领的字符串（f'xxx' 或 F'xxx'），以大括号 {} 标明被替换的字段；f-string在本质上并不是字符串常量，而是一个在运行时运算求值的表达式

> While other string literals always have a constant value, formatted strings are really expressions evaluated at run time.
>
> 与具有恒定值的其它字符串常量不同，格式化字符串实际上是运行时运算求值的表达式
>
> [Python Documentation](https://docs.python.org/3/reference/lexical_analysis.html)

f-string在功能方面不逊于传统的[%-formatting语句](https://docs.python.org/3/library/stdtypes.html)语句和[`str.format()`函数](https://docs.python.org/3/library/stdtypes.html)，同时性能又优于二者，且使用起来也更加简洁明了，因此对于Python3.6及以后的版本，推荐使用f-string进行字符串格式化

用了这种方式明显更简单了，不用再去判断使用 %s，还是 %d

```python
a = 'aaa'
f'bbb{a}'
# bbbaaa
```

- f-string用大括号 {} 表示被替换字段，其中直接填入替换内容

  

```python
f'a{1*2}'
# 'a2'
```

```python
a = 'aaa'
f'b{a.upper()}'
# 'bAAA'
```

- f-string的大括号 `{}` 可以填入`表达式`或`调用函数`，Python会求出其结果并填入返回的字符串内



```python
f'a{'b'}'
# SyntaxError: f-string: expecting '}'
```

```python
f"a{'b'}"
# 'ab'
```

```python
f'a{"b"}'
# 'ab'
```

- f-string大括号内所用的引号不能和大括号外的引号定界符冲突，可根据情况灵活切换 `'` 和 `"`



```python
f'a{{b}}'
```

- 如果输出需要显示大括号则使用2个大括号



```python
f'''a"b"'''
# 'a"b"'
```

```python
f'''"b"'a''''
# SyntaxError: EOL while scanning string literal
# 使用三个单/双引号的时候, 最右必须是三个单/双引号
```

- 若 `'` 和 `"` 不足以满足要求，还可以使用 `'''` 和 `"""`



```python
f'a\'b{"c"}'
# "a'bc"
# 使用双引号显示
```

```python
f'a{"\b"}'
# SyntaxError: f-string expression part cannot include a backslash
```

```python
f'a{"\b"}'
# SyntaxError: f-string expression part cannot include a backslash
```

- 大括号外可以使用`\`转义符

- 大括号内不仅不允许使用转义符, 也不允许`\`出现

- 只要大括号外使用转义符, 会用`""`显示输出结果

- 则应首先将包含 `\` 的内容用一个变量表示，再在f-string大括号内填入变量名

  ```python
  a = '\\'
  f'{a}'
  # '\\'
  ```

  

```python
f'{"a"}\
{"b"}\
{"c"}'
# 'abc'
# 多行字符串
```

```python
f'{"a"}\
  {"b"}\
  {"c"}'
# 'a  b  c'
```



### 格式转换

| **格式描述符** | **含义与作用**                                          | **适用变量类型**                       |
| -------------- | ------------------------------------------------------- | -------------------------------------- |
| s              | 普通字符串格式                                          | 字符串                                 |
| b              | 二进制整数格式                                          | 整数                                   |
| c              | 字符格式，按unicode编码将整数转换为对应字符             | 整数                                   |
| d              | 十进制整数格式                                          | 整数                                   |
| o              | 八进制整数格式                                          | 整数                                   |
| x              | 十六进制整数格式（小写字母）                            | 整数                                   |
| X              | 十六进制整数格式（大写字母）                            | 整数                                   |
| e              | 科学计数格式，以 e 表示 ×10^                            | 浮点数、复数、整数（自动转换为浮点数） |
| E              | 与 e 等价，但以 E 表示 ×10^                             | 浮点数、复数、整数（自动转换为浮点数） |
| f              | 定点数格式，默认精度（precision）是6                    | 浮点数、复数、整数（自动转换为浮点数） |
| F              | 与 f 等价，但将 nan 和 inf 换成 NAN 和 INF              | 浮点数、复数、整数（自动转换为浮点数） |
| g              | 通用格式，小数用 f，大数用 e                            | 浮点数、复数、整数（自动转换为浮点数） |
| G              | 与 G 等价，但小数用 F，大数用 E                         | 浮点数、复数、整数（自动转换为浮点数） |
| %              | 百分比格式，数字自动乘上100后按 f 格式排版，并加 % 后缀 | 浮点数、整数（自动转换为浮点数）       |

 ```python
f'{15:b}'
# '1111'
 ```

f-string采用 `{content:format}` 设置字符串格式，其中 `content` 是替换并填入字符串的内容，可以是变量、表达式或函数等，`format` 是格式描述符。采用默认格式时不必指定 `{:format}`如:  `f'{a}'`








### #

**`#`仅适用于数值类型**

`#` 对不同数值类型的作用效果不同，详见下表：

| **数值类型** | **不加#（默认）** | **加#**  | **区别**        |
| ------------ | ----------------- | -------- | --------------- |
| 二进制整数   | '1111'            | '0b1111' | 开头是否显示 0b |
| 八进制整数   | '17'              | '0o17'   | 开头是否显示 0o |
| 十进制整数   | '15'              | '15'     | 无区别          |
| 十六进制整数 | 'f'               | '0xf'    | 开头是否显示 0x |

```python
f'{15:o}'
# 17
```

```python
f'{15:#o}'
# '0o17' 零哦17
```





### 宽度与精度

| **格式描述符**  | **含义与作用**                                      |
| --------------- | --------------------------------------------------- |
| width           | 整数 width 指定宽度                                 |
| 0width          | 整数 width 指定宽度，开头的 0 指定高位用 0 补足宽度 |
| width.precision | 整数 width 指定宽度，整数 precision 指定显示精度    |

- `0width` 不可用于复数类型和非数值类型，`width.precision` 不可用于整数类型。

  ```python
  f'{"a":0s}'
  # ValueError: '=' alignment not allowed in string format specifier
  
  f'{"a":05s}'
  # ValueError: '=' alignment not allowed in string format specifier
  # 0width 不可用于复数类型和非数值类型
  ```

  ```python
  f'{123:5.1d}'
  # ValueError: Precision not allowed in integer format specifier
  # 5是宽度, .1是精度
  ```

  

- `width.precision` 用于不同格式类型的浮点数、复数时的含义也不同：用于 `f`、`F`、`e`、`E` 和 `%` 时 `precision` 指定的是小数点后的位数，用于 `g` 和 `G` 时 `precision` 指定的是有效数字位数（小数点前位数+小数点后位数）。

- `width.precision` 除浮点数、复数外还可用于字符串，此时 `precision` 含义是只使用字符串中前 `precision` 位字符。

  ```python
  f'{"abcdefg":5.1s}'
  # 'a    '
  # 字符串默认左对齐
  
  f'{"abcdefg":5.5s}'
  # 'abcde'
  ```

  

  

 ```python
f'{15:5d}'
# '   15'
# 数字默认右对齐
 ```

```python
f'{15:05d}'
# '00015'
```

```python
f'{3.1415:5.1f}'
# '  3.1'
# 5是宽度, .1是精度, f是指 定点数格式
```











### 对齐方式

| **格式描述符** | **含义与作用**               |
| -------------- | ---------------------------- |
| <              | 左对齐（字符串默认对齐方式） |
| >              | 右对齐（数值默认对齐方式）   |
| ^              | 居中                         |

 ```python
f'{12345:15d}'
# '          12345'
# 数字默认右对齐

f'{12345:<15d}'
# '12345          '

f'{12345:<15d}'
# '     12345     '
 ```





### 千位分隔符

| **格式描述符** | **含义与作用**      |
| -------------- | ------------------- |
| ,              | 使用,作为千位分隔符 |
| _              | 使用_作为千位分隔符 |

- 若不指定 `,` 或 `_`，则f-string不使用任何千位分隔符，此为默认设置。

- `,` 仅适用于浮点数、复数与十进制整数：对于浮点数和复数，`,` 只分隔小数点前的数位

  ```python
  f'{1.2345:,f}'
  # '1.234500'
  ```

- `_` 适用于浮点数、复数与二、八、十、十六进制整数：对于浮点数和复数，`_` 只分隔小数点前的数位；对于二、八、十六进制整数，固定从低位到高位每隔四位插入一个 `_`（十进制整数是每隔三位插入一个 `_`）

  ```python
  f'{0xffff:_b}'
  # '1111_1111_1111_1111'
  # 2, 8, 16进制整数, 每隔4位插入一个_
  ```



### 正负符号

| **格式描述符** | **含义与作用**                                |
| -------------- | --------------------------------------------- |
| +              | 负数前加负号（-），正数前加正号（+）          |
| -              | 负数前加负号（-），正数前不加任何符号（默认） |
| （空格）       | 负数前加负号（-），正数前加一个空格           |

**仅适用于数值类型**

 ```python
f'{123:+d}    {-123:+d}'
# '+123    -123'
 ```

```python
f'{+123:-d}    {-123:-d}'
# '123    -123'
```

```python
f'{123: d}    {-123: d}'
# ' 123    -123'
```





### 日期格式

[标准库 `datetime`](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior) 给定的用于排版时间信息的格式类型，适用于 [`date`](https://docs.python.org/3/library/datetime.html#date-objects)、[`datetime`](https://docs.python.org/3/library/datetime.html#datetime-objects) 和 [`time`](https://docs.python.org/3/library/datetime.html#time-objects) 对象

| **格式描述符** | **含义**                                                     | **显示样例** |
| -------------- | ------------------------------------------------------------ | ------------ |
| %a             | 星期几（缩写）                                               | 'Sun'        |
| %A             | 星期几（全名）                                               | 'Sunday'     |
| %w             | 星期几（数字，0 是周日，6 是周六）                           | '0'          |
| %u             | 星期几（数字，1 是周一，7 是周日）                           | '7'          |
| %d             | 日（数字，以 0 补足两位）                                    | '07'         |
| %b             | 月（缩写）                                                   | 'Aug'        |
| %B             | 月（全名）                                                   | 'August'     |
| %m             | 月（数字，以 0 补足两位）                                    | '08'         |
| %y             | 年（后两位数字，以 0 补足两位）                              | '14'         |
| %Y             | 年（完整数字，不补零）                                       | '2014'       |
| %H             | 小时（24小时制，以 0 补足两位）                              | '23'         |
| %I             | 小时（12小时制，以 0 补足两位）                              | '11'         |
| %p             | 上午/下午                                                    | 'PM'         |
| %M             | 分钟（以 0 补足两位）                                        | '23'         |
| %S             | 秒钟（以 0 补足两位）                                        | '56'         |
| %f             | 微秒（以 0 补足六位）                                        | '553777'     |
| %z             | UTC偏移量（格式是 ±HHMM[SS]，未指定时区则返回空字符串）      | '+1030'      |
| %Z             | 时区名（未指定时区则返回空字符串）                           | 'EST'        |
| %j             | 一年中的第几天（以 0 补足三位）                              | '195'        |
| %U             | 一年中的第几周（以全年首个周日后的星期为第0周，以 0 补足两位） | '27'         |
| %w             | 一年中的第几周（以全年首个周一后的星期为第0周，以 0 补足两位） | '28'         |
| %V             | 一年中的第几周（以全年首个包含1月4日的星期为第1周，以 0 补足两位） | '28'         |

 ```python
import datetime

help(datetime.date.today())
# Help on date object
# date 对象
 ```



```python
import datetime

f'{datetime.date.today():%a}'
# 'Mon'

f'{datetime.date.today():    %a}'
# '    Mon'
```

```python
import datetime

f'{datetime.date.today():%Y-%m-%d}'
# '2020-10-12'

f'{datetime.date.today():%Y-%m-%d (%a) %H:%M:%S}'
# '2020-10-12 (Mon) 00:00:00'
```









### lambda表达式

```python
n = lambda x: x * 1
n(1) # 1
n(11) # 11
# lambda [list] : 表达式
# lambda作为一个表达式，定义了一个匿名函数
```



f-string大括号内也可填入lambda表达式，但lambda表达式的 `:` 会被f-string误认为是表达式与格式描述符之间的分隔符，为避免歧义，需要将lambda表达式置于括号 `()` 内：

```python
f'{lambda x: x + 1 (1)}'
# SyntaxError: unexpected EOF while parsing
```

```python
f'{(lambda x: x + 1) (1)}'
# '2'
```

```python
f'{(lambda x: x  + 1) (1): <+5d}'
# '+2   '
```







## u

在Python2中，普通字符串是以8位ASCII码进行存储的，而Unicode字符串则存储为16位unicode字符串，这样能够表示更多的字符集。使用的语法是在字符串前面加上前缀 **u**。

在Python3中，所有的字符串都是Unicode字符串。



## r

```python
r'\n'
# \\n

r'\n\n\n'
# '\\n\\n\\n'
```

去掉反斜杠转义

即那些，反斜杠加上对应字母，表示对应的特殊含义的，比如最常见的”\n”表示换行，”\t”表示Tab等



## b

表示这是一个 bytes 对象

网络编程中，服务器和浏览器只认bytes 类型数据



## replace

```python
str.replace('', '') # 把参数1的内容替换为参数2, replace方法为str类的
```







# datetime.module

[`datetime`](https://docs.python.org/zh-cn/3.8/library/datetime.html#module-datetime) 模块提供用于处理日期和时间的类

| **类名**      | **功能说明**                                               |
| ------------- | ---------------------------------------------------------- |
| date          | 日期对象,常用的属性有year, month, day                      |
| time          | 时间对象                                                   |
| datetime      | 日期时间对象,常用的属性有hour, minute, second, microsecond |
| datetime_CAPI | 日期时间对象C语言接口                                      |
| timedelta     | 时间间隔，即两个时间点之间的长度                           |
| tzinfo        | 时区信息对象                                               |

# bytes.class

**Python 3最重要的新特性之一是对字符串和二进制数据流做了明确的区分**

- **编码就是把一个字符用一个二进制来表示**
- **二进制数据由 `bytes `类型表示。**
- **`bytes`**是一种比特流，它的存在形式是01010001110这种
- **图片、视频等文件通常是以bytes类型读写的，所以类型通常是rb, wb模式**



**`bytes`**是一种比特流，它的存在形式是01010001110这种。我们无论是在写代码，还是阅读文章的过程中，肯定不会有人直接阅读这种比特流，它必须有一个编码方式



bytes和str类型的区别
在将字符串存入磁盘和从磁盘读取字符串的过程中，Python自动地帮你完成了编码和解码的工作，你不需要关心它的过程。

- 从str到bytes，是编码

  ```python
  a = '我是字符串'
  a.encode()
  # b'\xe6\x88\x91\xe6\x98\xaf\xe5\xad\x97\xe7\xac\xa6\xe4\xb8\xb2'
  # 开头的b表示这是一个bytes类型。
  # \xe6是十六进制的表示方式，它占用1个字节的长度
  # 从字符串到比特流是编码的过程
  ```

  **从字符串到比特流是编码**

  

- 从bytes到str，是解码

  ```python
  a = '我是比特流'
  bit = bytes(a, 'utf-8') # py内置类
  bit.decode() # 这是解码
  # '我是比特流'
  # 解码必须指定一个格式, 如utf-8, 也就是encoding的参数，不可省略
  # 从比特流向字符串是解码的过程
  ```

  **从比特流向字符串是解码**

  

  

- Python3已经严格区分了bytes和str两种数据类型
- Python 3不会以任意隐式的方式混用**`str`**和**`bytes`**，你不能拼接字符串和字节流，也无法在字节流里搜索字符串（反之亦然）
- 你不能将字符串传入参数为字节流的函数（反之亦然）
- 你不能在需要bytes类型参数的时候使用str参数，反之亦然。这点在读写磁盘文件时容易碰到
- 使用bytes类型，实质上是告诉Python，不需要它帮你自动地完成编码和解码的工作，而是用户自己手动进行，并指定编码格式。







# os.module

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

Python是**强类型**的**动态**解释型语言

强类型: 不允许不同类型相加。如整型+字符串会报错

动态：不用显示声明数据类型，确定一个变量的类型是在第一次给它赋值的时候，也就是说: **变量的数据类型是由值决定的**





变量前面加一个`*`和`**`的区别
一个`*`会将参数转变为数组, `**`会将参数转变为字典,如果格式不对则无法转换成功



变量的交换

```python
a = 1
b = '2'

a, b = b, a
# a和b的值会交换, 也会改变ab两个变量的类型
```



py 销毁变量

```python
a = 10
del a
```



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







## number

- **int**

  通常被称为是整型或整数，是正或负整数，不带小数点。Python3 整型是没有限制大小的，可以当作 Long 类型使用，所以 Python3 没有 Python2 的 Long 类型。

  

- **float**

  浮点型由整数部分与小数部分组成，浮点型也可以使用科学计数法表示（2.5e2 = 2.5 x 102 = 250）

  

- **complex**

  复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都是浮点型。



## string

```python
a = 'a'
# 单引号和双引号没有区别
```



## list

```python
a = [1, 2, 3]
```

序列中的每个值都有对应的位置值，称之为索引，第一个索引是 0，第二个索引是 1，依此类推

- 与Python字符串不一样的是，列表中的元素是可以改变的
- List写在方括号之间，元素用逗号隔开
- 和字符串一样，list可以被索引和切片
- List可以使用+操作符进行拼接
- List中的元素是可以改变的



## tuple

```python
a = (1, 2, 3)
```

元组与列表类似，不同之处在于元组的元素不能修改

元组和列表, 下表都是从 `0` 开始

- 与字符串一样，元组的元素不能修改
- 元组也可以被索引和切片，方法一样
- 元组也可以使用+操作符进行拼接



## set

```python
a = {1, 2, 3}
```

```python
a = set()
```

```python
print({'a', 'b', 'c', 'c'})
# {'c', 'a', 'b'}
# 输出集合，重复的元素被自动去掉
```



集合（set）是一个无序的不重复元素序列。

可以使用大括号 **{ }** 或者 **set()** 函数创建集合，注意：创建一个空集合必须用 **set()** 而不是 **{ }**，因为 **{ }** 是用来创建一个空字典。

基本功能是进行成员关系测试和删除重复元素



## dict

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
  
  
  
## 变量标识符

变量标识符命名规则是Python中定义各种名字的时候的统⼀规范

- 只能由数字, 字母, 下划线组成
- 不能以数字开头
- 不能使用关键字
- **严格区分大小写**


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
| \(在行尾时)  | 一行未完，转到下一行继续写                                   |
| \\           | 反斜杠符号                                                   |
| \’           | 单引号                                                       |
| \”           | 双引号                                                       |
| \other       | 其它的字符以普通格式输出                                     |
| \n           | 换行符，将光标位置移到下一行开头                             |
| \r           | 回车符，将光标位置移到本行开头                               |
| \f           | 换页                                                         |
| \t           | 水平制表符，也即 Tab 键，一般相当于四个空格                  |
| \v           | 纵向制表符                                                   |
| \a           | 蜂鸣器响铃。注意不是喇叭发声，现在的计算机很多都不带蜂鸣器了，所以响铃不一定有效 |
| \b           | 退格（Backspace），将光标位置移到前一列                      |
| \x??         | 十六进制数，??代表的字符，例如：\x41代表A                    |

 

# 关键字

|        **关键字**         |                    **描述**                    |
| :-----------------------: | :--------------------------------------------: |
|          **and**          |                  逻辑运算符。                  |
|          **as**           |                   创建别名。                   |
|        **assert**         |                   用于调试。                   |
|    **[break](#break)**    |                   跳出循环。                   |
|         **class**         |                    定义类。                    |
| **[continue](#continue)** |             继续循环的下一个迭代。             |
|          **def**          |                   定义函数。                   |
|          **del**          |                   删除对象。                   |
|         **elif**          |       在条件语句中使用，等同于 else if。       |
|     **[else](#else)**     |                 用于条件语句。                 |
|        **except**         |          处理异常，发生异常时如何执行          |
|         **False**         |       布尔值，比较运算的结果 False == 0        |
|        **finally**        | 处理异常，无论是否存在异常，都将执行一段代码。 |
|      **[for](#for)**      |                创建 for 循环。                 |
|         **from**          |              导入模块的特定部分。              |
|        **global**         |                 声明全局变量。                 |
|       **[if](#if)**       |                写一个条件语句。                |
|        **import**         |                   导入模块。                   |
|          **in**           |     检查列表、元组等集合中是否存在某个值。     |
|          **is**           |             测试两个变量是否相等。             |
|        **lambda**         |                 创建匿名函数。                 |
|         **None**          |                 表示 null 值。                 |
|       **nonlocal**        |                声明非局部变量。                |
|          **not**          |                  逻辑运算符。                  |
|          **or**           |                  逻辑运算符。                  |
|     **[pass](#pass)**     |       null 语句，一条什么都不做的语句。        |
|         **raise**         |                   产生异常。                   |
|        **return**         |               退出函数并返回值。               |
|         **True**          |       布尔值，比较运算的结果。True == 1        |
|          **try**          |            编写 try...except 语句。            |
|    **[while](#while)**    |               创建 while 循环。                |
|         **with**          |               用于简化异常处理。               |
|         **yield**         |             结束函数，返回生成器。             |

```python
import keyword
print(keyword.kwlist)
# 打印关键字列表
```





## if

```python
if True:
    print('y')
# 'y'
```

```python
if False:
    print('n')
# 无输出
```

```python
if a > 5:
    print('y')
else:
    print('n')
# a = 1 结果为: n
# a = 15 结果为: y
```

```python
if a > 2:
    print('> 2')
    if a > 5:
        print('> 5')
```



### 三目运算符

```python
expression_True if expression else expression_False
# expression为真则执行e_True
# expression为家则执行e_False
```

```python
'a' if 2 > 1 else 'b'
# 'a'

'a' if 2 < 1 else 'b'
# 'b'
```



## else

while和for都可以配合else使⽤

else下⽅缩进的代码含义：当循环正常结束后执⾏的代码

```python
while False:
    print('a')
else:
    print('b')
# b
# 在 while … else 在条件语句为 false 时执行 else 的语句块
```









## while

```python
while True:
    print('\n')
# 会一直输出换行符
# CTRL+C 来退出当前的无限循环
```

```python
i = 1
while i <= 9:
    j = 1
    while j <= i:
        print(f'{j} * {j} = {j * i}', end='\t')
        j += 1
    print()
    i += 1
# 99乘法表
```





## break

**break** 语句用于跳出当前循环体

**break** 语句可以跳出 for 和 while 的循环体。如果你从 for 或 while 循环中终止，任何对应的循环 else 块将不执行

break终⽌循环不会执⾏else下⽅缩进的代码

```python
i = 0
while i <= 5:
    i += 1
    print(i, end)
    if i == 3:
        break
# 1       2       3
# 如果把break换成continue结果为
# 1       2       3       4       5       6
```

```python
while True:
    while True:
        print(1)
        break
# a(死循环)
# 两层while, 每层都为真
# 第2层为真执行print函数, 然后执行break
# 但是结果却是一直执行print的死循环
# 由此可以得知break不能跳出while嵌套
```





## continue

**continue** 语句被用来告诉 Python 跳过当前循环块中的剩余语句，然后继续进行下一轮循环

continue退出循环的⽅式执⾏else下⽅缩进的代码

```python
n = 0
while n < 5:
    if n == 3:
        continue
    print(n)
    n += 1
# 0
# 1
# 2
# |(光标闪烁)
# 这是一种错误的写法, 因为continue跳出本次循环并不执行后面的语句
# while为真继续往下走, if为假跳过continue语句继续往下走,
# 这时候输出n变量继续往下走, n += 1, 然后回到while判断, 如此循环
# 如果n为3, while为真继续往下走, if为真执行continue语句跳过本次循环
# 这时候需要注意的是跳过本次循环并不会往后执行语句,并不会执行print函数和n+1
# 这时候continue跳过了本次循环, 并返回while, 这里是重点
# 这时候n的值是3, while为真, 然后if为真, 然后跳过本次循环到while
# while为真, if为真, 然后回到while, 如此是个死循环
# 所以输出只有2, 因为print函数跟if是统一等级
```



```python
n = 0
while n < 5:
    print(n)
    if n == 3:
        continue
    n += 1
# 3, 一直输出3, 死循环
# 当n=3的时候, 执行continue跳过当前循环, 不执行n += 1 直接回到while
# 然后print函数, 然后if为真, 然后while为真, 如此死循环
```



```python
n = 0
while n < 5:
    n += 1
    if n == 3:
        continue
    print(n, end='\t')
# 1       2       4       5
```





## for

for循环可以遍历任何序列的项目，如一个列表或者一个字符串`

```python
for i in abc:
    print(i)
# a       b       c
```





## pass

Python pass是空语句，是为了保持程序结构的完整性。

pass 不做任何事情，一般用做占位语句，如下实例





# 魔法方法

## `__name__`

```python
if __name__ == '__main__'

# __name__ 是当前模块名，当模块被直接运行时模块名为 __main__  这句话的意思就是，当模块被直接运行时，以下代码块将被运行，当模块是被导入时，代码块不被运行
```





# 内置函数

|     **abs()**     |    **divmod()**     | **[input()](#input())** |       **open()**        |  **staticmethod()**   |
| :---------------: | :-----------------: | :---------------------: | :---------------------: | :-------------------: |
|     **all()**     |   **enumerate()**   |        **int()**        |        **ord()**        |       **str()**       |
|     **any()**     |     **eval()**      |    **isinstance()**     |        **pow()**        |       **sum()**       |
| **basestring()**  |   **execfile()**    |    **issubclass()**     | **[print()](#print())** |      **super()**      |
|     **bin()**     |     **file()**      |       **iter()**        |     **property()**      |      **tuple()**      |
|    **bool()**     |    **filter()**     |        **len()**        | **[range()](#range())** | **[type()](#type())** |
|  **bytearray()**  |     **float()**     |       **list()**        |     **raw_input()**     |     **unichr()**      |
|  **callable()**   |    **format()**     |      **locals()**       |      **reduce()**       |     **unicode()**     |
|     **chr()**     |   **frozenset()**   |       **long()**        |      **reload()**       |      **vars()**       |
| **classmethod()** |    **getattr()**    |        **map()**        |       **repr()**        |     **xrange()**      |
|     **cmp()**     |    **globals()**    |        **max()**        |      **reverse()**      |       **zip()**       |
|   **compile()**   |    **hasattr()**    |    **memoryview()**     |       **round()**       |     **import()**      |
|   **complex()**   |     **hash()**      |        **min()**        |   **[set()](#set())**   |  **exec 内置表达式**  |
|   **delattr()**   | **[help()](#help)** |       **next()**        |      **setattr()**      |                       |
|    **dict()**     |      **hex()**      |      **object()**       |       **slice()**       |                       |
|     **dir()**     |  **[id()](#id())**  |        **oct()**        |      **sorted()**       |                       |



## print()

```python
for i in range(0, 5):
    print(i)
# 0
# 1
# 2
# 3
# 4
```

```python
for i in range(0, 5):
    print(i, end='\t')
# 0       1       2       3       4
```



### VT100

VT100是一个终端类型定义,VT100控制码是用来在终端扩展显示的代码。比如果终端上任意坐标用不同的颜色显示字符。

所有的控制符是**\033**开头的, 即 ESC 的 8进制 ASCII 码

```python
print('\033[32;45mabc\033[0m')
# 32;45 设置前景色和背景色颜色
# \033[ 转义开始
# m 转义终止符
# abc 要显示的文本
# 0m 清空之前所有属性
```

|  **色彩**  | **黑** | **红** | **绿** | **黄** | **蓝** | **洋红** | **青** | **白** |
| :--------: | :----: | :----: | :----: | :----: | :----: | :------: | :----: | :----: |
| **前景色** | **30** | **31** | **32** | **33** | **34** |  **35**  | **36** | **37** |
| **背景色** | **40** | **41** | **42** | **43** | **44** |  **45**  | **46** | **47** |



## **help()**

```python
help(print)
# Help on built-in function print in module builtins:
# 查看一个函数是否是内置函数
```



**显示帮助信息**

**使用的时候要把括号去掉**

```python
help(bytes)
# Help on class bytes in module builtins:
# bytes 是一个类
```

```python
help(open)
# Help on built-in function open in module io:
# open是i个内置函数
```





## **input()**

```python
select = input('a, b, c')
# input 接收的任何数据默认是字符串数据类型
```



## **set()**





## **id()**

**id() 函数返回对象的唯一标识符，标识符是一个整数。**

**CPython 中 id() 函数用于获取对象的内存地址(10进制)**



## **range()**

**如果需要遍历数字序列，可以使用内置range()函数。它会生成数列**

```python
for i in range(0, 5):
    print(i)
# 0
# 1
# 2
# 3
# 4
```

```python
for i in range(0, 15, 3):
    print(i)
# 0
# 3
# 6
# 9
# 12
```



## open()

```python
if sys.version_info >= (3, 6):
    def open(file: Union[str, bytes, int, _PathLike[Any]], mode: str = ..., buffering: int = ..., encoding: Optional[str] = ...,
             errors: Optional[str] = ..., newline: Optional[str] = ..., closefd: bool = ...,
             opener: Optional[Callable[[str, int], int]] = ...) -> IO[Any]: ...
elif sys.version_info >= (3,):
    def open(file: Union[str, bytes, int], mode: str = ..., buffering: int = ..., encoding: Optional[str] = ...,
             errors: Optional[str] = ..., newline: Optional[str] = ..., closefd: bool = ...,
             opener: Optional[Callable[[str, int], int]] = ...) -> IO[Any]: ...
else:
    def open(name: Union[unicode, int], mode: unicode = ..., buffering: int = ...) -> BinaryIO: ...
```

```python
open(filename, mode, encoding)
# encoding='utf-8' 指定文件编码为utf-8,如果不设置此参数编码为系统默认编码
```



## type

```python
a = 1
type(a)
# int
# type函数会显示变量的类型
```












# **运算符**

## **算数运算符**

| **运算符** |                  **描述**                  |                       **实例**                       |
| :--------: | :----------------------------------------: | :--------------------------------------------------: |
|   **+**    |                   **加**                   |                                                      |
|   **-**    |          **减, 减法可以获得负数**          |                  **1 - 2 结果为-1**                  |
|   *****    |                   **乘**                   |                                                      |
|   **/**    |           **除, 可以获得浮点数**           |                                                      |
|   **%**    |                  **取模**                  |                  **-5 % 3 结果为1**                  |
|   ******   |                   **幂**                   |                                                      |
|   **//**   | **取整除    返回商的整数部分（向下取整）** |            **9 // 2 = 4    -9 // 2 = -5**            |
|   **()**   |                 **小括号**                 | **⼩括号⽤来提⾼运算优先级，即 (1 + 2) * 3 结果为9** |

### **%**

**取模和取余在目标上是一致的，但是因为语言对取余和取模上定义的不同，导致得到的结果不同**

- **取余运算在计算商值向0方向舍弃小数位**

- **取模运算在计算商值向负无穷方向舍弃小数位**

  ---

- **取余，遵循尽可能让商大的原则**

- **取模，遵循尽可能让商小的原则**

**经过测试，在C/C++, C#, JAVA, PHP这几门主流语言中，’%’运算符都是做取余运算，而在python中的’%’是做取模运算。**

**取余（rem）和取模（mod）在被除数、除数同号时，结果是等同的，异号时会有区别，所以要特别注意异号的情况**

```sh
# 以下是取模
5 mod 3 = 2
-5 mod 3 = 1
5 mod -3 = -1
-5 mod -3 = -2
```

```sh
# 以下是取余
5 rem 3 = 2
-5 rem 3 = -2
5 rem -3 = 2
-5 rem -3 = -2
```

**余数在数学中的定义是始终大于等于0的，而对于某些编程语言的取余运算不是按照上面的定义来执行的，因此才会出现余数小于0的情况。**

**说了这么多，我们在使用取模（取余）运算时要注意的地方有一个，当除数和被除数出现异号的情况时，程序移植的时候要特别小心，因为不同语言对取模和取余的定义可能是不同的**



### ******

```python
2 ** 3
# 8
# 2 ** 3 等价 2 * 2 * 2
```



### **//**

**向下取整**

```python
int(3.14)
# 3

int(3.9)
# 3
```

**向上取整**

```python
import math

match.ceil(3.1)
# 4
match.ceil(3.9)
# 4
```



**负数除法**

**12/3=4,：给我12个苹果（+12），每次给我3个（+3），需要4次；**

**12/4=3：给我12个苹果（+12），分4次给，每次给我3个（+3）；**

**13/3=4...1：给我13个苹果（+13），每次给我3个（+3），需要4次，还得再给我一个（+1）；**



**-12/-3=4：给你12个苹果（-12），每次给你3个（-3），需要4次；**

**-12/4=-3：给你12个苹果（-12），分4次给，每次给你3个（-3）；**

**-13/-3=4...-1：给你13个苹果（-13），每次给你3个（-3），需要4次，还得再给你一个（-1）。**

**12/-4=-3：给我12个苹果（+12），每次给你4个（-4），需要给我3次（-3）。**











## **赋值运算符**

|   **运算符**   |                   **描述**                    | **实例**                                  |
| :------------: | :-------------------------------------------: | ----------------------------------------- |
|     **=**      | **赋值, 将 = 右侧的结果赋值给等号左侧的变量** | **c = a + b 将 a + b 的运算结果赋值为 c** |
|     **+=**     |              **加法赋值运算符**               | **c += a 等效于 c = c + a**               |
|     **-=**     |              **减法赋值运算符**               | **c -= a 等效于 c = c - a**               |
|     ***=**     |              **乘法赋值运算符**               | **c *= a 等效于 c = c * a**               |
|     **/=**     |              **除法赋值运算符**               | **c /= a 等效于 c = c / a**               |
|     **%=**     |              **取模赋值运算符**               | **c %= a 等效于 c = c % a**               |
| **<b>**=</b>** |               **幂赋值运算符**                | **c = a 等效于 c = c  a**                 |
|    **//=**     |             **取整除赋值运算符**              | **c //= a 等效于 c = c // a**             |
|     **:=**     |                **海象运算符**                 |                                           |

- **海象运算符可在表达式内部为变量赋值。Python3.8  版本新增运算符**
- **因为很像海象的眼睛和象牙所以叫海象运算符(:=)**

```python
a = 'abc'
if (n := len(a)) > 1:
        print(f'{n}')
# 3
# 不使用print函数不输出变量值
```

```python
a = 'abc'
if n := len(a) > 1:
        print(f'{n}')
# True
# 小括号优先等级最高, 现计算小括号, 就变成了
# if n := 3 > 1:
# 又因为 > 比 := 优先级高, 所以变成了
# if n = True
```

```python
a = 'abc'
n = len(a)
if n > 1:
    print(f'{n}')
# 3
# 这是没有海象运算符的写法
```

```python
a = 'abc'
if len(a) > 1:
    print(f'{len(a)}')
# 3
# 没有海象运算符的写法
```

```python
while (block := f.read(256)) != '':
    process(block)
# 海象运算符的写法
```

```python
while 1:
    block = f.read(256)
    if block != '':
        process(block)
	else:        
		break
# 没有海象运算符的写法
```



## **比较运算符**

**比较运算符也叫关系运算符, 通常用来判断**

| **运算符** |   **描述**   |  **实例**  |
| :--------: | :----------: | :--------: |
|   **==**   |   **等于**   | **1 == 1** |
|   **!=**   |  **不等于**  | **1 != 2** |
|   **>**    |   **大于**   | **2 > 1**  |
|   **<**    |   **小于**   | **1 < 2**  |
|   **>=**   | **大于等于** | **2 >= 1** |
|   **<=**   | **小于等于** | **1 <= 2** |

- **<b>>=</b>, 无论是大于还是等于, 只要满足其一, 都返回True**
- **所有比较运算符返回1表示真，返回0表示假。这分别与特殊的变量True和False等价**





## **逻辑运算符**

| **运算符** |   **描述**   | **逻辑表达式** |
| :--------: | :----------: | :------------: |
|  **and**   | **布尔'与'** |  **x and y**   |
|   **or**   | **布尔'或'** |   **x or y**   |
|  **not**   | **不二'非'** |   **not x**    |

- **and**

  ```python
  True and False
  # False
  # True为真则计算False
  # 左值为真则计算右值, 右值为真则为真, 右值为假则为假
  
  True and 'abc'
  # 'abc'
  
  123 and 'abc'
  # 'abc'
  
  'a' and 'b'
  # 'b'
  
  print('a') and 'b'
  # 'b'
  
  False and True
  # False
  # 一方为False则返回False
  # False 为左值则无论如何都返回False
  # False 为左值则不用看右值, 因为左值为假则都为假
  
  True and 1
  # 1
  # and 的 左值 为 True 则返回右值, 右值支持表达式
  # 如果右值是一个表达式, 则计算表达式后返回
  # True and 1 + 2 结果为3
  
  123 and 1
  # 1
  # 左值为真则开始计算并返回右值
  
  False and 1
  # False
  # and 的 左值 为 False 则无论如何返回 False 
  ```

- **or**

  ```python
  True or False
  # True
  
  False or True
  # True
  # 哪方为True返回哪方
  # 任意一方为True则返回True
  
  1 or 123
  # 1
  # 两方都为True则优先返回左值
  
  False or 123
  # 123
  # 哪方为True则返回哪方
  ```

- **not**

  ```python
  not True
  # False
  
  not False
  # True
  
  not 123
  # False
  
  not -123
  # False
  
  not 0
  # True
  
  not (True and True)
  # False
  ```

  

  

 ## **运算优先级**

**在一个表达式中可能包含多个有不同运算符连接起来的、具有不同数据类型的数据对象**

**由于表达式有多种运算，不同的结合顺序可能得出不同结果甚至出现错误运算错误，因为当表达式中含多种运算时，必须按一定顺序进行结合，才能保证运算的合理性和结果的正确性、唯一性。**



**以下表格列出了从最高到最低优先级的所有运算符：**

|                     **运算符**                      |                          **描述**                          |
| :-------------------------------------------------: | :--------------------------------------------------------: |
|                       ******                        |                   **指数 (最高优先级)**                    |
|                   **`~` `+` `-`**                   | **按位翻转, 一元加号和减号 (最后两个的方法名为 +@ 和 -@)** |
|                **`*` `/` `%` `//`**                 |                 **乘，除，求余数和取整除**                 |
|                     **`+` `-`**                     |                        **加法减法**                        |
|                    **`>>` `<<`**                    |                    **右移，左移运算符**                    |
|                        **&**                        |                        **位 'AND'**                        |
|                     **`^` `|`**                     |                        **位运算符**                        |
|                    **<= < > >=**                    |                       **比较运算符**                       |
|                    **`==` `!=`**                    |                       **等于运算符**                       |
| **<b>`=` `%=` `/=` `//=` `-=` `+=` `*=` `**=`</b>** |                       **赋值运算符**                       |
|                  **`is` `is not`**                  |                       **身份运算符**                       |
|                  **`in` `not in`**                  |                       **成员运算符**                       |
|                **`not` `and` `or`**                 |                       **逻辑运算符**                       |

常用的运算符中: 算术  >  比较  >  逻辑  > 赋值



### **混合运算**

0. **()**
1. **<b>**</b>**
2. **<b>*</b>**
3. **/**
4. **//**
5. **%**
6. **+**
7. **-**

### **赋值运算**

0. **=**

1. **+=**

2. **-=**

   

0. **先算赋值运算符右边的值或者表达式**
1. **再算赋值运算的算术运算**
2. **最后在是赋值**



## **位运算符**

| **运算符** |                           **描述**                           |                           **实例**                           |
| :--------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|   **&**    | **按位与运算符：参与运算的两个值,如果两个相应位都为1,则该位的结果为1,否则为0** |       **(a & b) 输出结果 12 ，二进制解释： 0000 1100**       |
|   **\|**   | **按位或运算符：只要对应的二个二进位有一个为1时，结果位就为1** |      **(a \| b) 输出结果 61 ，二进制解释： 0011 1101**       |
|   **^**    |     **按位异或运算符：当两对应的二进位相异时，结果为1**      |       **(a ^ b) 输出结果 49 ，二进制解释： 0011 0001**       |
|   **~**    | **按位取反运算符：对数据的每个二进制位取反,即把1变为0,把0变为1。~x 类似于 -x-1** | **(~a ) 输出结果 -61 ，二进制解释： 1100 0011， 在一个有符号二进制数的补码形式** |
|   **<<**   | **左移动运算符：运算数的各二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补0** |       **a << 2 输出结果 240 ，二进制解释： 1111 0000**       |
|   **>>**   | **右移动运算符：把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数** |       **a >> 2 输出结果 15 ，二进制解释： 0000 1111**        |



### **&**















## **成员运算符**

| **运算符** |                         **描述**                          |                       **实例**                        |
| :--------: | :-------------------------------------------------------: | :---------------------------------------------------: |
|   **in**   |   **如果在指定的序列中找到值返回 True，否则返回 False**   |    **x 在 y 序列中 , 如果 x 在 y 序列中返回 True**    |
| **not in** | **如果在指定的序列中没有找到值返回 True，否则返回 False** | **x 不在 y 序列中 , 如果 x 不在 y 序列中返回 True。** |

 ```python
list = 'abc'
'a' in 'abc'
# True
 ```

```python
list = 'abc'
'd' not in 'abc'
# True
```







## **身份运算符**

**身份运算符用于比较两个对象的存储单元**

| **运算符** |                     **描述**                      |                           **实例**                           |
| :--------: | :-----------------------------------------------: | :----------------------------------------------------------: |
|   **is**   |   **`is` 是判断两个标识符是不是引用自一个对象**   | **x is y, 类似 id(x) == id(y) , 如果引用的是同一个对象则返回 True，否则返回 False** |
| **is not** | **`is not` 是判断两个标识符是不是引用自不同对象** | **x is not y ， 类似 id(a) != id(b)。如果引用的不是同一个对象则返回结果 True，否则返回 False** |



**is 与 == 区别：**

**is 用于判断两个变量引用对象是否为同一个， == 用于判断引用变量的值是否相等**



# edu

https://docs.python.org/zh-cn/3.9/



https://www.runoob.com/python3/python3-tutorial.html



https://www.liaoxuefeng.com/wiki/1016959663602400



# **pyenv**

**https://github.com/pyenv/pyenv**



**pyenv是一个python多版本管理工具，当服务器上存在不同版本的python项目时，使用pyenv可以做到多版本的隔离使用(类似虚拟化)，每个项目使用不同版本互不影响。**



```bash
dnf install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel libpcap-devel xz-devel -y
# 常见依赖包的问题
```

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
# 使用git clone下载安装到家目录的.pyenv目录
```

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> /etc/profile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> /etc/profile
su -
# 设置环境变量,并使之生效,这样才能直接使用pyenv命令
```

```python
pyenv install -l
# --或者使用pyenv install --list列出所有的python当前可用版本
```

```sh
pyenv versions
# 查看当前安装的版本,前面带*号的是默认使用的版本
```





## **pyenv-virtualenv**

**https://github.com/pyenv/pyenv-virtualenv**



**pyenv-virtualenv是pyenv的插件，为pyenv设置的python版本提供隔离的虚拟环境。不同版本的python在不同的虚拟环境里使用互不影响**



```sh
git clone https://github.com/pyenv/pyenv-virtualenv.git $PYENV_ROOT/plugins/pyenv-virtualenv
# 将pyenv-virtualenv插件复制到pyenv根目录下的plugins下
```

```sh
pyenv virtualenv 3.6.6  py36
# 把安装的3.6.6版本做一个隔离的虚拟化环境，取名为py36(这个取名是自定义的)
```

```python
pyenv activate py36
# 激活环境
```

```sh
echo 'eval "$(pyenv init -)"' >> /etc/profile
echo 'eval "$(pyenv virtualenv-init -)"' >> /etc/profile
su -
# 如果报错, 输入以上命令解决
# Failed to activate virtualenv.
# Perhaps pyenv-virtualenv has not been loaded into your shell properly.
# Please restart current shell and try again.
```





```sh
pyenv uninstall py36
# 删除创建的隔离环境
```









# **pypi**





# **iso.py**

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











# 注释



```python
# 单行注释
```

```python
'''
多行注释
3个单引号或者3个双引号都可以
'''
```
