```bash
tar xvf jdk.tar -C /usr/local # 将jdk解压到/usr/local目录下
```

```bash
tar xvf apache-tomcat -C /usr/local # 将tomcat解压到/usr/local目录下
```

```bash
export JAVA_HOME=/usr/local/jdk # 设置jdk路径
```

```bash
export PATH=$PATH:$JAVA_HOME/bin # path路径分隔符为:
```

```bash
export TOMCAT_HOME=/usr/local/apache-tomcat # 设置tomcat路径
```

```bash
export PATH=$PATH:$TOMCAT_HOME/bin # 把tomcat脚本添加到可执行路径
```

```bash
bash /usr/local/apache-tomcat/bin/startup.sh
# 启动tomcat, 不可以用点(./)形式启动脚本, 会退出当前登录
```

```bash
bash bin/catalina.sh run# 以debug模式启动tomcat
```

```bash
bash /usr/local/apache-tomcat/bin/shutdown.sh # 关闭tomcat
```

localhost:8080
tomcat默认端口为8080





| 目录 | 说明 |
| :--: | :--: |
|bin        |存放tomcat启动和关闭的脚本|
|conf       |存放相关的配置文件|
|lib        |存放所需的jar文件,也就是库文件|
|logs       |存放日志|
|temp       |存放临时文件|
|webapps    |存放应用程序目录,类似于nginx的html目录|
|work       |存放jsp程序编译后产生的class文件|





```
在webapps/ROOT 创建time.jsp文件,并输入以下内容
<h1 /><%= new java.util.Data()%>
并访问localhost:8080/a.jsp 如果显示时间则证明tomcat可以解析jsp代码
```



```
访问 serverAddr:8080/managerstatus 则查看服务器状态,如果提示 403 Access Denied 是因为tomcat默认只允许本机查看状态
注释以下文件的以下内容则需要配置配置用户名与密码才可以访问服务器状态
webapps/manager/META-INF/context.xml
<!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
<!--多行注释符-->
配置以下 conf/tomcat-users.xml 文件的以下内容添加可以访问服务器状态的用户
<role rolename="manager-gui"/>
<user username="userName" password="pass" roles="manager-gui"/>
<!--用户名区分大小写-->
```



```
访问 serverName:8080/manager 访问应用管理,此页面需要登录
在webapps下每一个目录都是一个应用,选择相应目录,点击stop后此目录无法访问,如果访问此目录会提示404,点击start重新访问
```



```
修改以下文件的以下内容 conf/server.xml
<Connector port="8080" protocol="HTTP/1.1"
            onnectionTimeout="20000"
            redirectPort="8443" />
<!-- port="" 此值为tomcat端口值-->
<!--Connector标签的port属性值为tomcat端口的值,修改此属性则修改tomcat监听值-->
<!--修改端口后需要重启tomcat才能生效-->
```



```
部署应用
删除 webapps/ROOT 目录下所有文件在解压项目到 webapps/ROOT 目录下即可,部署应用需要重启tomcat
```



```
war包部署
公司的开发人员开发好应用后,有时候会打包成 .war 结尾的包.
只要tomcat启动状态,把war包直接拷贝到 webapps/ 下就可以自动解压
解压的目录名为war包的文件名
```



```
nginx+tomcat架构
修改 /etc/nginx/nginx.conf
upstream tomcat {
	server 10.1.1.12:8080 weight=1;
	server 10.1.1.13:8080 weight=1;
    # 将下面一段加到http {}配置段里但是不要在server {}配置段里
}
```



```
location ~ .*\.jsp$ {
	proxy_pass   http://tomcat;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $remote_addr;
}
```

