```bash
systemctl restart network.service
# 重启网络
```

```bash
systemctl start httpd.service
# 启动 httpd, 可以省略 .service 部分
```

```bash
systemctl stop firewalld.service
# 关闭防火墙
```

```bash
systemctl enable mariadb.service
# 开机启动数据库
# 不开机启动是 disable
```

```bash
systemctl list-unit-files # 显示所有服务
```



