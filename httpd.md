配置支持php

```
Define SRVROOT "$apache_dir"
LoadModule php7_module "$php_dir\php7apache2_4.dll"
PHPIniDir "$php_dir"
AddType application/x-httpd-php .php
<IfModule dir_module>
    DirectoryIndex index.html index.php
</IfModule>

Set-Location X:\$php_dir

Copy-Item .\php.ini-development .\php.ini
# php.ini-development 适合开发模式
# php.ini-production 有较高的安全设定,适合生产模式
```



配置虚拟机

```
<VirtualHost *:80>
    ServerAdmin $mail
    ServerName $domain
    DocumentRoot "$www_vhosts"
    <Directory "$www_vhosts">
        Options indexes FollowSymLinks
        DirectoryIndex index.html index.php
        AllowOverride All
        Require all granted # 这1行是2.4的写法
        # Order allow,deny
        # Allow from all 以上2行是2.2的写法
    </Directory>
    ErrorLog "$log_dir/$domain-error.log"
    CustomLog "$logs_dir/$domain-access.log" common
</VirtualHost>
```

