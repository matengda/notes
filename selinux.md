# 简介

Security Enhanced Linux

安全增强的linux



由美国国家安全局（National Security Agency，NSA）贡献的一个Linux内核模块主要用于Redhat及其衍生版本中，为整个系统提供更加健壮的安全服务

之所以开发这个模块，是因为NSA发现在系统的安全问题中，绝大多数问题并不是由于外部的攻击多么强大，更多的问题是由于所谓的**内部员工的资源误用**



例如，在Apache中，默认的服务目录是/var/www/html，如果一个无经验的系统运维人员在运维过程中，将其权限设置为777，那么所有的进程都可以在该目录中进行读写，这将造成极大的安全隐患



# dac模式

Discretionary Access Control

自主访问控制



所谓DAC主要是指在没有启用SELinux的Linux系统中，系统会依据资源的**rwx权限**以及**用户身份（进程所有者）**来判断是否具有资源访问的权限。使用DAC的缺点在于：

- root具有最高权限：对于root用户而言，其具有系统最高的权限，因此各种rwx设置对于root用户而言形同虚设。
- 用户可以获取进程修改文件资源的访问权限：如果将某个资源的权限设置为777，那么该目录可以被任何用户读写操作





# mac模式

Mandatory Access Control

强制访问控制



一种可以针对特定的**进程**与**文件资源**来进行权限的权限。因此即便拥有了root权限，当执行特定的进程时，也未必拥有相应的读写权限。而SELinux采用的正是MAC方式。

简单而言，在DAC中控制的主体（subject）是用户，而MAC中控制的对象是进程



# getenforce

```sh
getenforce # 查看SElinux状态
```

- SELinux有三种模式

  | **Enforceing** |                  **强制模式, SElinux启动**                   |
  | :------------: | :----------------------------------------------------------: |
  | **Permissive** | **宽容模式，SELinux的安全策略不强制执行，但是会有相应的警告信息记录到日志文件中，一般用于SELinux的调试** |
  |  **Disabled**  |                           **关闭**                           |







# sestatus

 sestatus - SELinux status tool



```sh
sestatus
# 查看 selinux 状态
```

- SELinux status:                 enabled
  SELinuxfs mount:                /sys/fs/selinux
  SELinux root directory:         /etc/selinux
  Loaded policy name:             targeted
  Current mode:                   enforcing
  Mode from config file:          enforcing
  Policy MLS status:              enabled
  Policy deny_unknown status:     allowed
  Memory protection checking:     actual (secure)
  Max kernel policy version:      31





# setenforce

setenforce - modify the mode SELinux is running in



```sh
setenforce 1
# 开启SELinux, 如果想要永久更改需要修改配置文件
# 1为enforcing模式
```



```sh
setenforce 0
# 关闭SELinux, 这些是临时操作
# 0为permissive模式
```



# restorecon

 restorecon - restore file(s) default SELinux security contexts



```sh
restorecon -rv /
# 还原 selinux 上下文
```

-  -R, -r change files and directories file labels recursively (descend directories).

  递归

-  -v     show changes in file labels. Multiple -v options increase the  verbosity.  Note  that  the  -v  and  -p
                options are mutually exclusive.

  显示详细信息, 多个v增加详细程度, -v和-p是互斥的

- -p     show  progress  by printing the number of files in 1k blocks unless relabeling the entire OS, that             will
                     then show the approximate percentage complete. Note that the -p and -v options are mutually    exclusive.
            
            显示进度的大概百分比





# getsebool

getsebool - get SELinux boolean value(s)



```sh
getsebool -a
# 查看安全策略的具体内容
```





# setsebool

setsebool - set SELinux boolean value



```sh
setsebool -P boolean value
# 设置策略规则
# -p 重启有效
```

```sh
setsebool -P allow_ftpd_anon_write=1
# 允许 vsftp 匿名写入
```







# /etc/selinux/config

```sh

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=enforcing
# 将SELINUX=enforcing改为SELINUX=disabled, 将彻底关闭selinux
# 从enforcing或permissive模式切换为disabled模式，或者反过来，则需要重启系统（从disabled模式修改为其他两种模式时，需要重启两次，因为要重新写入上下文信息）才能生效。在disabled模式下，无法使用`setenforce`命令



# SELINUXTYPE= can take one of these three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
# 在SELinux中默认有三种策略
# targeted：针对网络服务（如dhcpd、httpd、named、nscd、ntpd、portmap、snmpd、squid以及 syslogd等）的限制较多，针对本机的限制较少，是默认的策略
# minimum：修改自targeted策略，只针对选择的进程进行保护
# mls：多层的安全防护，使用完整的SELinux限制
# 修改SELinux的防护策略，必须要对系统进行重启



```











