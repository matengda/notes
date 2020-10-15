```sh
dnf upgrade
# 更新系统
# update 和 upgrade 是等价的	
```

```sh
dnf instal emacs
# 安装emacs
```

```sh
dnf install ./xlockmore-5.31-2.el6.x86_64.rpm

dnf localinstall ./xlockmore-5.31-2.el6.x86_64.rpm
# 安装来自本地指定路径下的rpm包，而不是来自仓库
```

```sh
dnf remove emacs
# 删除emacs
```

```sh
dnf autoremove emacs
# 删除emacs并自动移除残留的各种文件, 比如配置文件
```

```sh
dnf erase
# 删除软件
```

```sh
dnf search emacs
# 搜索 emacs 相关的软件
```

```sh
dnf info php
# 查看软件信息
```

```sh
dnf clean all
# 清空列表缓存
```

```sh
dnf list
# 列出仓库里所有的软件包
```

```sh
dnf repolist
# 列出软件源
```

```sh
dnf grouplist
# 列出所有软件组
```

```sh
dnf groupinstall
# 安装软件组
```

```sh
dnf groupremove
# 删除软件组
```





# .repo

```shell
[local]
name=local # 仓库的名称
baseurl=http://
        ftp://
        # 可以写多个url
enabled=1 # 1表示启用仓库, 0表示不启用
gpgcheck=0 # 验证是否开启
# http://mirrors.ustc.edu.cn/centos/7/os/x86_64/RPM-GPG-KEY-CentOS-7
```