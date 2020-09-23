```
server {
    charset utf-8; # 设置编码为utf-8
    autoindex; # 显示目录列表
    autoindex_exact_size on; # 显示文件确切大小
    auth_basic "please input user and password"; # 提示信息
    auth_basic_user_file /etc/nginx/passwd; # 用户认证文件位置, 这里必须输入绝对路径, 如果使用相对路径即使输入正确信息依旧报403
}
```



nginx接受服务时会创建很多环境变量，这些变量可在/etc/nginx/fastcgi_params中查看，nginx将每一次http请求时URL中的参数字符串赋值给QUERY_STRING，如果URL的参数中只有一个参数名字(命令名)，那么QUERY_STRING就是要执行的shell命令了



```bash
echo "username:$(openssl passwd password)" >> passwd # 为nginx生成用户验证文件
```

