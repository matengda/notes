```
# .bashrc

alias vm='vim'
alias em='emacs'
```





内置变量

```bash
#? # 上一条命令执行后的返回状态
: << comment
0    执行正常, 非0表示异常
1&2  没有那个文件或目录
126  无权限执行
127  command not found
comment
```

```bash
$$ # 当前shell进程id
```



查看一个命令是否是bash内置

```bash
type pwd # pwd is a shell builtin
```

```bash
type vi # vi is /usr/bin/vi
```



循环

```bash
for i in ./*; do echo $i; done # 如果要写成一行必须写成这样
```

```bash
for i in ./*; do
echo $i
done
```