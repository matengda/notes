```bash
dnf config-manager --add-port=https://download.docker.com/linux/centos/docker-ce.repo
# 添加docker源
```



```bash
dnf install -y docker-ce docker-cli # 安装docker
```



```bash
systemctl start docker && systemctl enable docker # 启动docker并设置为开机启动
```



```bash
docker run centos:latest /bin/echo 'e' # 打印e, latest为容器的最新版
```



```bash
docker run -ti centos:latest /bin/bash # 与容器进行交互ho
```



```bash
docker ps -a # 查看全部容器的状态
```



```bash
docker top $CONTAINER_ID # 优雅的停止容器
```



```bash
docker kill $CONTAINER_ID # 强制停止容器
```

