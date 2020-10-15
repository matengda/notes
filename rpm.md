```bash
rpm -qa # 查看所有已经安装的软件
```

```bash
rpm -e '' # 删除软件
```

```sh
rpm -ivh ./
# 安装本地rpm包
# --force 强制安装
```

```sh
rpm -Uvh
# 如果已安装老版本,则升级;如果没安装,则直接安装
```

```sh
rpm --nodeps
# 忽略依赖关系
```

```sh
rpm -ql
# 列出已经安装的软件列表
```

```sh
prm -qf httpd.conf
# 查看文件来自哪个rpm包
```

```sh
rpm -qi
# 查看rpm包的详细信息
```

```sh
rpm -qd
# 查看软件的文档列表
```

```sh
rpm -qc
# 查看软件的配置文件
```

```sh
rpm --import
# 导入公钥
```

```sh
rpm -checksig
# 检查rpm包的签名
```

