# semanage

**semanage - SELinux Policy Management tool**



```sh
semanage fcontext -a -t <类型> <文件>
restorecon [-rv] 文件/文件夹
# 修改 文件的安全上下文
# 使用 semanage 之后要使用 restorecon 命令
# semanage 修改的是 selinux中的默认值
```







# chcon

**chcon - change file SELinux security context**



```sh
chcon [-R] [-t <类型>] [-u <user>] [-r <role>] <文件>
# 修改 文件的安全上下文
```

- -R：连同子目录一起进行修改
- -t：修改Type
- -u：修改Identify
- -r：修改Role
- -v：如果修改成功，会将变动的结果列出来



```sh
chcon -R --reference=<参考文件> <目标文件>
# --reference：参考文件，用参考文件的type修改目标文件的type
```









# setsebool

**setsebool - set SELinux boolean value**



```sh
setsebool -P boolean value
# 设置策略规则
# -P 设置永久, 重启有效
```

```sh
setsebool -P allow_ftpd_anon_write=1 # boolean on
# 允许 vsftp 匿名写入
```







# getsebool

**getsebool - get SELinux boolean value(s)**



```sh
getsebool -a
# 查看安全策略的具体内容
```











# restorecon

 **restorecon - restore file(s) default SELinux security contexts**



```sh
restorecon -rv /
# 还原 文件的安全上下文
# restorecon 可以恢复文件的上下文 说明有个存储所有文件默认type的位置
```

- -R, -r

  change files and directories file labels recursively (descend directories).

  递归

- -v

  show changes in file labels. Multiple -v options increase the  verbosity.  Note  that  the  -v  and  -p
  options are mutually exclusive.

  显示详细信息, 多个v增加详细程度, -v和-p是互斥的

- -p

  show  progress  by printing the number of files in 1k blocks unless relabeling the entire OS, that             will then show the approximate percentage complete. Note that the -p and -v options are mutually    exclusive.

  显示进度的大概百分比





# setenforce

**setenforce - modify the mode SELinux is running in**



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





# sestatus

 **sestatus - SELinux status tool**



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









# getenforce

 **getenforce - get the current mode of SELinux**



```sh
getenforce # 查看SElinux状态
```

- SELinux有三种模式

  | **Enforceing** |                  **强制模式, SElinux启动**                   |
  | :------------: | :----------------------------------------------------------: |
  | **Permissive** | **宽容模式，SELinux的安全策略不强制执行，但是会有相应的警告信息记录到日志文件中，一般用于SELinux的调试** |
  |  **Disabled**  |                           **关闭**                           |









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
# targeted：对部分网络应用进行保护（如dhcpd、httpd、named、nscd、ntpd、portmap、snmpd、squid以及 syslogd等）的限制较多，针对本机的限制较少，是默认的策略
# minimum：修改自targeted策略，只针对选择的进程进行保护
# mls：多层的安全防护，使用完整的SELinux限制
# 修改SELinux的防护策略，必须要对系统进行重启



```







# 安全上下文

Security Context

在selinux中，访问控制属性叫做安全上下文，所有客体（文件、进程间通讯通道、套接字、网络主机等）和主体（进程）都有与其关联的安全上下文，一个安全上下文由三部分组成：用户（u）、角色（r）、和类型（t）标识符。但我们最关注的是第三部分

当程序访问资源时 ，主体程序必须要通过selinux策略内的规则放行后，就可以与目标资源进行安全上下文的比对，若比对失败则无法存取目标，若比对成功则可以开始存取目标，最终能否存取目标还要与文件系统的rwx权限的设定有关



由于历史原因，一个进程的类型通常被称为一个域（domain），"域"和"域类型"意思都一样，我们不必苛刻地去区分或避免使用术语域，通常，我们认为【域】、【域类型】、【主体类型】和【进程类型】都是同义的，即都是安全上下文中的“TYPE”。



Security Context是一种面向进程的读写权限管理

任何一个进程想要访问一个资源

1. 经过 **SELinux的安全策略检验**
2. 经过 **安全上下文的比对**
3. 最后经过 文件rwx权限 检验才能最终确认是否可以对资源进行访问



## 查看安全上下文

```sh
id -Z
# 查看 shell 的上下文
```



```sh
ps -Z
# 查看 进程 的上下文
```



```sh
ls -Z
# 查看 文件 的上下文
```





## 安全上下文的构成

```sh
Identify：Role：Type/Domain
```

安全上下文存放在文件的 inode 中

安全上下文由 3部分 构成



**进程的安全上下文**

|                     **User(用户)**                      |                     **角色(Role)**                      |                        **Type(类型)**                        |
| :-----------------------------------------------------: | :-----------------------------------------------------: | :----------------------------------------------------------: |
|          **system_u, 系统服务进程, 受到管理**           |            **system_r, 系统进程, 收到管理**             |           **在 targeted 模式下唯一需要关注的字段**           |
| **unconfined_u, 不受管制进程, 通常用户自己开启如 bash** | **unconfined_r, 不受管制进程, 通常用户自己开启如 bash** | **在 targeted 模式下, 只有两者的类型对应上了(不代表相同), 进程才能访问文件** |

```sh
unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 1318 pts/1 00:00:00 bash
```





**文件的安全上下文**

|            **User(用户)**            |         **角色(Role)**         |                        **Type(类型)**                        |
| :----------------------------------: | :----------------------------: | :----------------------------------------------------------: |
|   **system_u, 系统服务创建的文件**   | **均为 object_r 表示一个文件** |           **在 targeted 模式下唯一需要关注的字段**           |
| **unconfined_u, 用户自己创建的文件** | **均为 object_r 表示一个文件** | **在 targeted 模式下, 只有两者的类型对应上了(不代表相同), 进程才能访问文件** |

```sh
system_u:object_r:selinux_config_t:s0 /etc/selinux/config
```









### Identify

Identity用来标识身份, 一般以 **`_u`** 结尾

| **unconfined_u** | **不受限用户, 也就是说该文件来自于不受限制的进程** |
| :--------------: | :------------------------------------------------: |
|   **system_u**   |                    **系统用户**                    |





### Role

Role字段用来标识角色, 一般用 **`_r`** 结尾

| **object_r** | **代表文件或目录等资源** |
| :----------: | :----------------------: |
| **system_r** |    **代表进程和用户**    |



### Type

Type字段用来标识类型, 一般用 **`_t`** 结尾

在默认的 targeted 策略中, 最为重要的是 type类型

一个进程能不能访问这个资源, 主要取决于 type类型

|  **type**  | **在文件资源上（也就是`ls -Z`中看到的）称为类型（Type）**     user_home_t |
| :--------: | :----------------------------------------------------------: |
| **domain** | **在主体进程（Subject，应该就是在`ps -Z`）称为域（Domain）**    **crond_t** |

**只有当domain与type对应，才能对资源进行访问**





## 例子

```sh
ls -Z /etc/selinux/config
# system_u:object_r:selinux_config_t:s0 /etc/selinux/config
```

- 





# 工作流程

| **Subjects** | **主体** |
| :----------: | :------: |
| **Objects**  | **目标** |
|  **Policy**  | **策略** |
|   **Mode**   | **模式** |

当一个主体Subject（如一个程序）尝试访问一个目标Object（如一个文件），SELinux安全服务器SELinux Security Server（在内核中）从策略数据库Policy Database中运行一个检查。基于当前的模式mode，如果 SELinux 安全服务器授予权限，该主体就能够访问该目标。如果SELinux安全服务器拒绝了权限，就会在/var/log/messages中记录一条拒绝信息

永远不推荐关闭 SELinux。为什么？当你这么做了，就会出现这种可能性：你磁盘上的文件可能会被打上错误的权限标签，需要你重新标记权限才能修复。而且你无法修改一个以 Disabled 模式启动的系统的模式。你的最佳模式是Enforcing或者Permissive





# 策略

SELinux中默认有三种策略

- targeted：针对网络服务（如dhcpd、httpd、named、nscd、ntpd、portmap、snmpd、squid以及 syslogd等）的限制较多，针对本机的限制较少，是默认的策略
- minimum：修改自targeted策略，只针对选择的进程进行保护
- mls：多层的安全防护，使用完整的SELinux限制

如果要**修改SELinux的防护策略，必须要对系统进行重启**。











# dac模式

Discretionary Access Control

自主访问控制



在DAC中控制的主体（subject）是用户



所谓DAC主要是指在没有启用SELinux的Linux系统中，系统会依据资源的**rwx权限**以及**用户身份（进程所有者）**来判断是否具有资源访问的权限。这种权限管理机制的主体是**用户**，也称为**自主访问控制（DAC）**。

使用DAC的缺点在于：

- root具有最高权限：对于root用户而言，其具有系统最高的权限，因此各种rwx设置对于root用户而言形同虚设。
- 用户可以获取进程修改文件资源的访问权限：如果将某个资源的权限设置为777，那么该目录可以被任何用户读写操作





# mac模式

Mandatory Access Control

强制访问控制



Selinux则是基于MAC（强制访问机制），简单的说，就是程序和访问对象上都有一个安全标签（即selinux上下文）进行区分，只有对应的标签才能允许访问，否则即使权限是777，也是不能访问

MAC中控制的对象是进程

MAC基于保密性和完整性强制信息的隔离以限制破坏，该限制单元独立于传统的Linux安全机制运作，并且没有超级用户的概念









# 简介

Security Enhanced Linux

安全增强的linux



SELinux主要是红帽Red Hat Linux以及它的衍生发行版上的一个工具。类似地， Ubuntu 和 SUSE（以及它们的衍生发行版）使用的是 AppArmor。SELinux和AppArmor 有显著的不同。你可以在SUSE，openSUSE，Ubuntu等等发行版上安装SELinux，但这是项难以置信的挑战



由美国国家安全局（National Security Agency，NSA）贡献的一个Linux内核模块

它为Linux内核子系统引入了一个健壮的强制控制访问Mandatory Access Control架构

之所以开发这个模块，是因为NSA发现在系统的安全问题中，绝大多数问题并不是由于外部的攻击多么强大，更多的问题是由于所谓的**内部员工的资源误用**

例如，在Apache中，默认的服务目录是/var/www/html，如果一个无经验的系统运维人员在运维过程中，将其权限设置为777，那么所有的进程都可以在该目录中进行读写，这将造成极大的安全隐患