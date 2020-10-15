```sh
sysctl -p # 默认从/etc/sysctl.conf 加载sysctl设置, 配置内核参数
```





# sysctl.conf

```sh
echo 'net.ipv4.tcp_congestion_control=bbr' >> /etc/sysctl.conf && sysctl -p
# 临时开启bbr
```

