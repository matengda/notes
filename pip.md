```python
import os

pip = [
    'ipython',
    'psutil',
    'IPy',
    'requests',
    'virtualenv',
    'fabric',
    'django==2.2',
    'uwsgi',
    ]
 
for i in pip:
    os.system('pip install' + ' ' + i)
```



```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
# 临时使用清华源
# 注意，simple 不能少, 是 https 而不是 http
```



pip镜像

|                   链接                   |  说明  |
| :--------------------------------------: | :----: |
| https://pypi.mirrors.ustc.edu.cn/simple/ | 中科大 |
| https://pypi.tuna.tsinghua.edu.cn/simple |  清华  |
|  http://mirrors.aliyun.com/pypi/simple/  | 阿里云 |



```bash
pip list # 列出所有软件包
```



```bash
pip install --upgrade $package
# 升级指定的软件包
```



```bash
pip list --outdate
# 列出可升级软件包
```



```bash
pip install -r $file
# -r, --requirement <file> # 安装文件列表
```



**https://www.lfd.uci.edu/~gohlke/pythonlibs/**

这个网站有很多whl安装包