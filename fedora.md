```
#!/usr/bin/env bash

dnf update -y
dnf install -y\
    tmux\
    vim\
    emacs\
    gcc\
    python3\
    platform-python-devel\
    nginx\
    mariadb-server\
    vsftpd\
    subversion\
    git\
    ntpdate\
    fish\
    clang\
    ca-certificates\

: << EOF
: 为bash内建命令, 本身没有副作用
使用反斜杠在命令没有完全输入之前不可以使用bash注释
php 依赖有 apache
mariadb-server 依赖有 mariadb(mariadb-client)
ca-sertificates mozillaca
pdo-mysqlnd pdo mysql 驱动(mariadb)
EOF

systemctl enable\
    httpd.service\
    php-fpm.service\
    mariadb.service\
    vsftpd\
    
firewall-cmd\
    --zone FedoraServer\
    --permanent\
    --add-service http\
    --add-service https\
    --add-service mysql\
    --add-service ftp\
    
firewall-cmd\
    --zone FedoraServer\
    --permanent\
    --add-port 3690/tcp\
    # add-service 和 add-port 不能同时使用
    # add-service 不允许使用 add-port
    
firewall-cmd --reload
```





**应用**

- apache http server
- mariadb
- php
- phpstorm





**添加静态IP**

/etc/sysconfig/network-scripts/ifcfg-$interfaceName

