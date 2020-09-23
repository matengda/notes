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





# str 类

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



# 变量类型

| 类型  | 说明 |
| ----- | ---- |
| int   |      |
| str   |      |
| list  |      |
| tuple |      |
| dict  |      |
| set   |      |

bytes和str类型的区别

Python 3最重要的新特性之一是对字符串和二进制数据流做了明确的区分。文本总是Unicode，由str类型表示，二进制数据则由bytes类型表示。Python 3不会以任意隐式的方式混用str和bytes，你不能拼接字符串和字节流，也无法在字节流里搜索字符串（反之亦然），也不能将字符串传入参数为字节流的函数（反之亦然）。
在将字符串存入磁盘和从磁盘读取字符串的过程中，Python自动地帮你完成了编码和解码的工作，你不需要关心它的过程。
使用bytes类型，实质上是告诉Python，不需要它帮你自动地完成编码和解码的工作，而是用户自己手动进行，并指定编码格式。
Python已经严格区分了bytes和str两种数据类型，你不能在需要bytes类型参数的时候使用str参数，反之亦然。这点在读写磁盘文件时容易碰到
从str到bytes，是编码,   比特流=str(串,encoding='utf-8')
从bytes到str，是解码，串=bytes(比特流,encoding='utf-8')







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

