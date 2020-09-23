设置本地用户名和用户名邮箱

```bash
git config user.name "$userName"
# 如果加 --global 就是设置全局用户名
```
```bash
git config user.email "$userMail"
```

删除已经Add的文件(撤销放入缓存区的文件)

```bash
git rm --cached "$filePath" # 路径不加双引号也可以
```

通过ssh方式克隆一个库

```bash
git clone git@github.com:matengda/notes.git # .git 可以省略
# 如果已经把自己的公钥添加到github里, 这样克隆就可以不用输入用户名和密码
```

