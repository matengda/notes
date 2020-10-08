```bash
getenforce # 查看SElinux状态
# Enforcing 开启 
# Permissive 关闭
```

```bash
setenforce 1 # 开启SELinux, 如果想要永久更改需要修改配置文件
```

```bash
setenforce 0 # 关闭SELinux, 这些是临时操作
```

