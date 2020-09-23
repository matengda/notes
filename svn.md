```bash
svnadmin create ./txt # 创建一个仓库
```



修改配置允许匿名修改文件
vm conf/svnserve.conf
在 [general] 下修改为以下内容
anon-access = write



```bash
svnserve -d -r ./ # 启动svn服务
```

-d, --daemon
            Causes svnserve to run in daemon mode.  svnserve backgrounds
            itself and accepts and serves TCP/IP connections on the svn port
            (3690, by default).



-r root, --root=root
     Sets the virtual root for repositories served by svnserve.  The
     pathname in URLs provided by the client will be interpreted
     relative to this root, and will not be allowed to escape this
     root.






```bash
svn checkout svn://192.168.150.200/txt # 克隆svn库
```



```bash
svn add ./*.txt # 添加文件到svn库
```



```bash
svn commit -m "20200503" # 上传文件
```