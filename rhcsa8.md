# s考试环境信息

总分为300分, 210分及以上合格



除了使用的物理台式机外, 还会使用多个虚拟系统, 虚拟机具有root权限, 但物理机没有root权限



## 虚拟系统

所有的虚拟系统都位于 **172.25.250.0/255.255.255.0** 的子网中

子网中的所有系统都位于 **lab.example.com**

除非另有指定, 否则密码默认为 **redhat**



|         系统          |    ip地址     |
| :-------------------: | :-----------: |
| mars.lab.example.com  | 172.25.250.10 |
| venus.lab.example.com | 172.25.250.11 |

- 练习题都是在 **mars.lab.example.com** 上执行的

- | **venus** |  **金星**  |
  | :-------: | :--------: |
  | **mars**  |  **火星**  |
  |  **lab**  | **实验室** |



## 软件存储库地址

http://content.example.com/rhel8.0/x86_64/dvd/BaseOS

http://content.exmple.com/rhel8.0/x86_64/dev/AppStream



## 重要信息

- 虚拟系统会在重新引导后进行评测
- **务必确保所有更改在虚拟机重启后仍有保留**
- 服务必须在没有人工干预的情况下启动
- **无法引导, 无法在无人工干预引导的系统上完成的所有造作都将判定为0分**



## rht-vmctl

```
Error: missing subcommand or VMNAME.

This utility manages the Red Hat Training supplied VMs on the local
hypervisor.

Usage: rht-vmctl [-y|--yes] VMCMD VMNAME [DATETIME]
       rht-vmctl [-i|--inquire] VMCMD VMNAME [DATETIME]
       rht-vmctl -h|--help

  where VMCMD is one of:
    view       - launches console viewer of VMNAME
    start      - obtain and start up VMNAME
    stop       - stop a running VMNAME
    restart    - if running, stop then start VMNAME
    poweroff   - if running, force stop VMNAME
    reset      - poweroff, return to saved or original state, start VMNAME
    save       - stop, save image, start VMNAME (to DATETIME)
    restore    - poweroff, restore to save (to DATETIME), start VMNAME
    listsaves  - list the saves of VMNAME
    status     - display libvirt status of VMNAME
    get        - if not here, obtain VMNAME from server
    remove     - remove VMNAME from system
    fullreset  - poweroff, reobtain from server, start VMNAME (bad save/image)

  -i|--inquire - confirm each VMNAME first
  -y|--yes     - confirm nothing, just do it

  VMNAME of "all" processes all VMs available in the course
```



```sh
rht-vmctl stop all
# 停止所有的虚拟机
# 经测试发现此命令显示已经全部停止, 但是 virt-manager 却显示有的正在运行
```











# 在 mars.lab.example.com 上执行以下任务



# 1. 配置网络设置

将mars配置为具有以下网络配置:

- 主机名: mars.lab.example.com

- ip地址: 172.25.250.10

- 子网掩码: 255.255.255.0

- 网关: 172.25.250.254

- dns服务器 172.25.250.254










```sh
hostnamectl set-hostname mars.lab.example.com
# 设置主机名为 mars.lab.example.com
```



```bash
nmcli connection modify 'Wired connection 1' ipv4.method manual ipv4.address 172.25.250.10/24 ipv4.gateway 172.25.0.254 ipv4.dns 172.25.250.254
# ipv4.method manual 更改ip获取方式为手动
# connection.autoconnect yes 自动连接
# W 大写
```
  ```bash
nmcli connection up 'Wired connection 1'
# 如果不输入此命令那么不会激活新的配置
  ```





# 2. 配置系统使用的默认存储库

YUM 存储库已经可以从以下地址使用

http://content.example.com/rhel8.0/x86_64/dvd/BaseOS

http://content.exmple.com/rhel8.0/x86_64/dev/AppStream

将这些位置用作默认存储库



```bash
vim /etc/yum.repos.d/rhel80.repo
```

```bash
[BaseOS]
name=BaseOS
baseurl=http://content.example.com/rhel8.0/x86_64/dvd/BaseOS
enabled=1
gpgcheck=0

[AppStream]
name=AppStream
baseurl=http://content.exmple.com/rhel8.0/x86_64/dev/AppStream
enabled=1
gpgcheck=0
```





# 3. 调试 SELinux

非标准端口 82 上运行的 Web 服务器在提供内容时遇到问题

根据需要调试解决问题, 并满足以下条件

- 系统上的 Web 服务器能够提供 /var/www/html 中所有的 HTML 文件
- 不得删除或以其他方式改变现有的文件内容
- Web 服务器在端口 82 上提供此内容
- Web 服务器在系统启动时自动启动





```sh
semanage port -a -t http_port_t 82 -p tcp
# semanage 是更改 selinux 安全上下文
# 所以修改后必须使用 restorecon 恢复安全上下文

resotrecon -rv /var/www/html
# 恢复文件默认安全上下文
# -p 递归
```

- -a, --add

  Add a record of the specified object type

  添加一条上下文

- -t TYPE, --type TYPE

  SELinux type for the object

  对象类型

- -p PROTO, --proto PROTO

  Protocol for the specified port (tcp|udp) or  internet  protocol
  version for the specified node (ipv4|ipv6).

  指定端口协议





```sh
semanage port -l | grep http_port_t
# -l, --list, List records of the specified object type
# 看看是否有81端口
```



```sh
systemctl enabled --now httpd
# 开机启动, 并立即启动服务
```



```sh
firewall-cmd --add-port=82/tcp --permanent

firewall-cmd --reload
```



```sh
firefox 172.25.250.10:82
# 查看是否显示主页, 如果显示则说明正确
```





# 4. 创建用户账户

创建以下用户, 组和组成员

- 名为 sysmgrs 的组
- 用户 natasha, 作为次要组从属 sysmgrs
- 用户 harry, 作为次要组还从属 sysmgrs
- 用户 sarah, 无权访问系统上的交互式shell 且不是 sysmgrs 的成员
- natasha, harry, sarah 的密码应当都是 redhat





```sh
groupadd sysadms
# 创建名为 sysadms 的组
```



```sh
useradd natasha -G sysadms
# 创建 natasha 用户, 并附加到组 sysadms
```

- -G, --groups GROUP1[,GROUP2,...[,GROUPN]]]
             A list of supplementary groups which the user is also a member of. Each group is separated from the next
             by a comma, with no intervening whitespace. The groups are subject to the same restrictions as the group
             given with the -g option. The default is for the user to belong only to the initial group.

  用户也是其他组的成员



```sh
useradd harry -G sysadms
# 创建 harry 用户, 并附加到组 sysadms
```



```sh
useradd sarah -s /sbin/nologin
# 创建 sarah 用户 并设置非交互式shell
```



```sh
echo redhat | passwd -- stdin natasha
echo redhat | passwd -- stdin harry
echo redhat | passwd -- stdin sarah
# 设置 natasha, harry, sarah 的密码
```





# 5. 配置 cron 作业

用户natasha 配置一个 cron 作业, 该作业每天 14:23 运行并执行 /binecho hiya



```sh
crontab -e -u natasha
```

```sh
23 14 * * * /bin/echo hiya
# 分 时 日 月 周
```







# 6. 创建协作目录

创建具有以下特真正的协作目录 /home/managers

- /home/managers 组所有权是 sysmgrs
- 目录可被 sysmgrs 组的成员 读, 写, 访问, 但是其他任何用户不具这些权限(root除外)
- /home/managers 中创建的文件自动将组所有权设置到 sysmgrs组







```sh
mkdir /home/managers
```



```sh
chown .sysmgrs /home/managers
# 所属组是 sysmgrs
```



```sh
chmod g+rwx,o-rwx /home/managers
# 组的成员 读, 写, 访问
# 其他成员不能 读, 写, 执行
```



```sh
chmod g+s /home/managers
# 创建的文件自动所属组
# 设置 SGID(set group id)
```





# 7. 配置 NTP

- 配置系统, 使其成为

  classroom.example.com

  的 NTP 客户端

- classroom.example.com 是 sidecar.lab.example.com 的 DNS 别名













```sh
vi /etc/chrony.conf
```

```sh
server classroom.exmple.com iburst
# 根据实际时间计算出服务器增减时间的比率，然后记录到一个文件中，在系统重启后为系统做出最佳时间补偿调整。
```



```sh
systemctl restart chronyd
```

```sh
chronyc sources -v
# sidecar.lab.example.com
# 如果配置正确则会看到以上信息
```





# 8. 配置 autofs

按照以下所述自动挂载远程用户的主目录

- classroom.example. (172.25.254.254)NFS 导出 /rhome 到系统, 此文件系统包含为用户 laoma 预配置的主目录
- laoma 的目录是 classroom.example.com:/rhome/laoma
- laoma 的主目录应自动挂载到本地 /rhome 下的 laoma
- 主目录必须可供用户写入













```sh
dnf install -y autofs
```



```
vi /etc/auto.master.d/netdir.autofs
```

```sh
/rhome    /etc/auto.netdir
# /rhome 这个是 挂载目录
# /etc/auto.netdir 这个是 子配置文件
# /etc/auto.netdir 这个是 自动挂载的配置文件, 文件名必须以 auto 开头
```

- autofs服务程序的**主配置文件**中需要按照**“挂载目录 子配置文件”**的格式写入参数
- 挂载目录是设备要**挂载位置的上一级目录**
- 如挂载目录是 /rhome/laoma, 那么此处应该写 /rhome
- 而对应的子配置文件则是**对这个目录内挂载设备信息的进一步说明**





```sh
vi /etc/auto.netdir
# /etc/auto.netdir 这个是 子配置文件
```

```sh
laoma -fstype=nfs,rw,sync classroom.example.com:/rhome/laoma
# laoma 挂载点
# loama 子配置文件中的 挂载点
# -fstype 文件类型
# classroom.example.com:/rhome/laoma 需要挂载的设备名称
```



```sh
systemctl enable --now autofs
# 设置开机启动, 并立即启动服务
```



```sh
ssh laoma@mars.lab.example.com
```





# 9. 配置 /var/tmp/fstab 权限

将文件 /etc/fstab 复制到 /var/tmp/fstab

配置 /var/tmp/fstab 的权限以满足以下条件

- /var/tmp/fstab 由 root 用户所有
- /var/tmp/fstab 属于 root 组
- /var/tmp/fstab 不能被任何人执行
- 用户 natasha 能够读取和写入 /var/tmp/fstab
- 用户 harry 无法写入读取 /var/tmp/fstab
- 所有其他用户能够读取 /var/tmp/fstab













```sh
cp /etc/fstab /var/tmp
# fstab 属于root用户和root组
```



```sh
setfacl -m u:natasha:rw, u:harry:-wr, o:r /var/tmp/fstab
# o:r 不能被任何人执行, 且其他用户可以读取
# u:natasha:rw natasha 用户可以读写
# u:harry harry 用户无法写入和读取
```

-  -M, --modify-file=file  read ACL entries to modify from file







# 10. 配置用户账户

配置用户 laoniu

- id为: 3533
- 密码为: redhat















```sh
usermod -u 3533 laoniu
# -u, --uid UID
```

```sh
echo redhat | passwd --stdin laoniu
```







# 11. 查找文件

查找用户 jacques 所有的文件并将其 副本 放入 /root/findfiles







```sh
mkdir /root/findfiles
```

```sh
find / -user jacques -exec cp -apr {} /root/findfiles \;
```





# 12. 查找字符串

- 查找 /usr/share/doc/words/readme.txt 中包含 crosswords 所有的行
- 将所有这些行的副本按照原始顺序放入 /root/list
- /root/list 不得包含空行
- 且所有的行必须是 /usr/share/doc/words/readme.txt 中的原始行











```sh
grep crosswords /usr/share/doc/words/readme.txt > /root/list
# grep crossword ./readme.txt
# 113,809 official crosswords
# 4,160 official crosswords delta
# When combined with the 113,809 crosswords file, it produces the
```





# 13. 创建存档

- 创建一个名为 /root/backup.tar.gz 的 tar 存档
- 存档应包含 /usr/local 的内容
- 该 tar 存档必须使用 gzip 压缩









```sh
tar cvzf /root/backup.tar.gz /usr/local
```







---

# 在 venus.lab.example.com 上执行以下任务





# 1. 设置 root 密码

- 将 venus 的 root 用户密码 设置为 redhat
- 必须获得系统的访问权限才能进行此操作
- 当前的 root 的密码并不知道













在启动的时候按e, 编辑启动选项

```sh
console=tty0 rd.break
# 在 linux 开头的行, 末尾加入如上
# tty Teletypes 电传打字机
# 通过向内核添加 rd.break 参数来以单用户模式启动
# C-x 启动进入 switch_root
```

```sh
mount
# dev/vda1 on /sysroot type xfs (ro,relatime,attr2,inode64,noquota)
# 通过命令可以得知vda1挂载到了/sysroot, 只读挂载
# ro=readonly
```

```sh
mount -o remount,rw /sysroot
# 读写模式挂载
```

```sh
chroot /sysroot
# 改变系统根分区
```

```sh
echo redhat | passwd --stdin root
```

```sh
touch /.autorelabel
# 默认使用 SELinux，因此创建下面的隐藏文件，这个文件会在下一次启动时重新标记所有文件
```

```sh
C-d C-d
# 重新启动
```









# 2. 配置系统以使用默认存储库

YUM 存储库已经可以从以下地址使用

http://content.example.com/rhel8.0/x86_64/dvd/BaseOS

http://content.example.com/rhel8.0/x86_64/dvd/AppStream

将这些位置用作默认存储库













```bash
vim /etc/yum.repos.d/rhel80.repo
```

```bash
[BaseOS]
name=BaseOS
baseurl=http://content.example.com/rhel8.0/x86_64/dvd/BaseOS
enabled=1
gpgcheck=0

[AppStream]
name=AppStream
baseurl=http://content.example.com/rhel8.0/x86_64/dvd/AppStream
enabled=1
gpgcheck=0
```





# 3. 设置逻辑卷大小

- 将逻辑卷 laomalv 及其文件系统的大小调整到 230M
- 确保文件系统内容保持不变
- 分区大小与请求的大小完全相同, 因此范围是 217 - 243M













```sh
lvresize -rL 230M /dev/laomavg/laomalv
# lvresize - Resize a logical volume
# -r|--resizefs
# -L|--size [+|-]Size[m|UNIT]
# M 必须是大写的
```



```sh
lvs
# lvs - Display information about logical volumes
# 使用lvs命令查看lv空间
```





# 4. 添加交换分区

- 向系统添加一个额外的交换分区, 大小为 756M
- 交换分区应在系统启动时自动挂载
- **不要删除或以任何方式改动系统上的任何现有交换分区**















```sh
swapon --show
# 显示 swap 大小
# --show 比 -s 获得更好的显示效果
```



```sh
fdisk /dev/vdb
```

- 分区的时候选择 主分区
- First sector (1050624-10485759, default 1050624): 这里默认 直接回车即可
- Last sector, +sectors or +size{K,M,G,T,P} (1050624-10485759, default 10485759):  +756m(不区分大小写)
- 然后按 t 更改分区类型
- 分区类型为 82 Linux swap / Solaris



```sh
mkswap /dev/vdb3
```

```sh
swapon /dev/vdb3
# 激活交换分区
```



```sh
vi /etc/fstab
```

```sh
/dev/vdb3    swap    swap    defaults    0    0
# 设置开机挂载
# <device>	<mountpoint>	<type>	<options>	<dump>	<pass>
```



```sh
swapon --show
# 查看是否增加
```





# 5. 创建逻辑卷

- 创建一个逻辑卷, 逻辑卷名为qa, 属于 qagroup 卷组, 大小为 60 个扩展块
- qagroup 卷组中的逻辑卷的扩展块大小为 16M
- 使用 ext3 格式化新逻辑卷
- 逻辑卷在系统启动时自动挂载到 /mnt/qa















```sh
fdisk /dev/vdb
# 将 /dev/vdb 所有的剩余空间分配给 扩展分区/dev/vdb4
# 在扩展分区中创建一个容量为 1G 的分区 /dev/vdb5, 类型为 8e
```

```sh

```



```sh
udevadm settle
# 如果内科更新失败, 重启系统后继续
```



```sh
vgcreate -s 16M qagroup /dev/vdb5
```



```sh
lvcreate -n qa -l 60 qagroup
```



```sh
mkdir /mnt/qa
# 也可以使用 UUID 挂载
```



```
vi /etc/fstab
```

```sh
/dev/qagroup/qa    /mnt/qa    ext3    defaults    0 0
```



```sh
mount -a
```

```sh
df
```





# 6. 创建 VDO 卷

- 创建一个新的 VDO 卷
- 使用未分区的磁盘
- 改卷的名称为 vdough
- 改卷的逻辑大小为 50G
- 改卷使用 xfs 文件系统格式化
- 在系统启动时, 改卷自动挂载到 /vbread 下



```sh
dnf install vdo kmod-kvdo
```



```sh
systemctl enable vdo --now
```



```sh
vdo create --name=vdough --device=/dev/vdc --vdoLogicalSize=50G
```



```sh
mkfs.xfs -k /dev/mapper/vdough
```



```sh
mkdir /vbread
```



```sh
vim /etc/fstab
```

```sh
/dev/mapper/vdough    /vbread    xfs    defaults,x-systemd.requires=vdo.service    0 0
```



```sh
mount -a
```



```sh
df
```





# 7. 配置系统调优

- 选择 tuned 配置集, 并将它设为默认设置



```sh
tuned-adm recommend
# virtual-guest
# recommend 推荐的意思
```

- RHEL在 6.3 版本以后引入了一套新的系统调优工具 tuned/tuned-adm
- tuned 是服务端程序，用来监控和收集系统各个组件的数据，并依据数据提供的信息动态调整系统设置，达到动态优化系统的目的
- tuned-adm 是客户端程序，用来和 tuned 打交道





```sh
tuned-adm profile virtual-guest
```



```sh
tuned-adm active
# Current active profile: virtual-guest
```







# 评分脚本

**必须在mars上做第一道题, 否则脚本会卡死**



```bash
# rhcsa8.sh
# 将 rhcsa8.sh 文件上传到 kiosk 家目录, 并执行


#!/bin/bash
function wait_ssh {
WAITIME=0
while true; do
  if $(echo "laoma" >/dev/tcp/$1/22) &>/dev/null; then
    echo "  $1 is running."
    break
#  else
#    if [[ "$1" == "mars" ]];then
#      VMNAME=servera
#    else
#      VMNAME=serverb
#    fi
    rht-vmctl status ${VMNAME}|egrep "${VMNAME}.*RUNNING" -q
    result=$?
    if [[ ${result} -eq 0 ]] ; then
      sleep 30
    else
      rht-vmctl reset ${VMNAME}
    fi
    [[ $WAITIME -gt 300 ]] && break
  fi
  WAITIME=$(expr $WAITIME + 5 )
done
}

function print_PASS() {
  echo -e '\033[1;32m正确\033[0;39m'
}

function print_FAIL() {
  echo -e '\033[1;31m错误\033[0;39m'
}

function print_DONE() {
  echo -e '\033[1;35m完成\033[0;39m'
}

echo -n "重启节点：mars venus ------ "
#venus主机卸载vdo磁盘后重启。
ssh root@venus umount -a &>/dev/null
for host in mars venus
do
  rht-vmctl restart $host -q &>/dev/null
done && print_DONE

echo -n "检查节点是否运行：mars venus ------ "

for host in mars venus
do
  wait_ssh $host &>/dev/null
done
print_DONE

echo -e "\n*******************  成 * 绩  *******************"

cat <<EOF > /tmp/grade.sh
#!/bin/bash

function print_PASS() {
  echo -e '\033[1;32m正确\033[0;39m'
}

function print_FAIL() {
  echo -e '\033[1;31m错误\033[0;39m'
}

function print_DONE() {
  echo -e '\033[1;35m完成\033[0;39m'
}

VMNAME=\$(hostname -s)
case \$VMNAME in
  mars)
      cat <<EOF1 >/etc/yum.repos.d/rhel80-test.repo
[BaseOS]
name=BaseOS
baseurl=http://content.example.com/rhel8.0/x86_64/dvd/BaseOS
enabled=1
gpgcheck=0
[AppStream]
name=AppStream
baseurl=http://content.example.com/rhel8.0/x86_64/dvd/AppStream
enabled=1
gpgcheck=0
EOF1
      yum install -y perl &>/dev/null && rm -f /etc/yum.repos.d/rhel80-test.repo

#用于统计成绩
grade=1
      echo -e "\n################## \$VMNAME 第一题 ##################"
      echo "配置主机名和网络："
      echo -n "检查主机名 ------ "
      [[ "\$(hostname)" == "mars.lab.example.com" ]]  && print_PASS && let grade++ || print_FAIL	
      echo -n "检查IP地址 ------ "
      ip a| grep '172.25.250.10/24' &>/dev/null  &&  print_PASS && let grade++ || print_FAIL	
      echo -n "检查网关 ------ "
      route -n|egrep '0.0.0.0.*172.25.250.254' &>/dev/null && print_PASS && let grade++ || print_FAIL
      echo -n "检查名称服务器 ------ "
      egrep 'nameserver.*172.25.250.254' /etc/resolv.conf &>/dev/null && print_PASS && let grade++ || print_FAIL	
      	
      echo -e "\n################## \$VMNAME 第二题 ##################"
      echo "配置yum仓库："
      echo -n "检查yum仓库文件语法 ------ "
      [[ \$(yum repolist all 2>/dev/null | grep enabled |wc -l ) -ge 2 ]] && print_PASS && let grade++ || print_FAIL
      echo -n "检查BaseOS仓库配置 ------ "	
      grep -rqs  "http://content.example.com/rhel8.0/x86_64/dvd/BaseOS" /etc/yum.repos.d/ && print_PASS && let grade++ || print_FAIL
      echo -n "检查AppStream仓库配置 ------ "	
      grep -rqs  "http://content.example.com/rhel8.0/x86_64/dvd/AppStream" /etc/yum.repos.d/ && print_PASS && let grade++ || print_FAIL	

      echo -e "\n################## \$VMNAME 第三题 ##################"
      echo "调试SELinux："
      echo -n "检查SELinux模式 ------ "
      [[ "\$(getenforce)" == "Enforcing" ]] &>/dev/null && grep 'SELINUX=enforcing' /etc/selinux/config &>/dev/null && print_PASS && let grade++ || print_FAIL
      echo -n "检查端口82标签 ------ "
      semanage port -l | grep ^http_port_t | grep '82,' &>/dev/null && print_PASS && let grade++ || print_FAIL
      echo -n "检查文件/var/www/html/*标签 ------ "
      ls -lZ /var/www/html/file1 |grep httpd_sys_content_t &>/dev/null && print_PASS && let grade++ || print_FAIL
      echo -n "检查httpd服务是否开机启用并启动 ------ "
      ( systemctl is-enabled httpd && systemctl is-active httpd ) &>/dev/null && print_PASS && let grade++ || print_FAIL
      echo -n "检查防火墙规则 ------ "
      ( firewall-cmd --list-all | grep 'ports:.*82/tcp' && firewall-cmd --list-all --permanent|grep 'ports:.*82/tcp' ) &>/dev/null && print_PASS && let grade++ || print_FAIL
      echo -n "检查站点是否可以访问 ------ "
      curl http://mars:82/file1  &>/dev/null && print_PASS && let grade++ || print_FAIL

    
      echo -e "\n################## \$VMNAME 第四题 ##################"
      echo "创建用户和组："
      echo -n "检查组sysmgrs及其成员 ------ "
      (grep '^sysmgrs.*natasha' /etc/group && grep '^sysmgrs.*harry' /etc/group) &>/dev/null && print_PASS && let grade++ || print_FAIL
      echo -n "检查用户sarah shell ------ "
      (grep sarah /etc/passwd | grep '/sbin/nologin') &>/dev/null && print_PASS && let grade++ || print_FAIL
      for user in natasha harry sarah
      do
        echo -n "检查用户\$user密码 ------ "
        FULLHASH=\$(grep "\${user}:" /etc/shadow | cut -d: -f 2)
        SALT=\$(grep "^\${user}:" /etc/shadow | cut -d'\$' -f3)
        PERLCOMMAND="print crypt(\"redhat\", \"\\\\\\\$6\\\\\\$\${SALT}\");"
        NEWHASH=\$(perl -e "\${PERLCOMMAND}")
        [[ "\${FULLHASH}" == "\${NEWHASH}" ]] && print_PASS && let grade++ || print_FAIL	
      done
  
      echo -e "\n################## \$VMNAME 第五题 ##################"
      echo "配置cron作业："
      echo -n "检查crond服务是否开机启用并启动 ------ "
      ( systemctl is-enabled crond && systemctl is-active crond ) &>/dev/null && print_PASS && let grade++ || print_FAIL
      echo -n "检查natasha作业 ------ "
      (crontab -l -u natasha | grep '23.*14.*\*.*\*.*\*.*/bin/echo.*hiya' ) &>/dev/null && print_PASS && let grade++ || print_FAIL
      
      echo -e "\n################## \$VMNAME 第六题 ##################"
      echo "创建协作目录： "
      echo -n "检查目录组拥有者 ------ "
      [[ "\$(ls -ld /home/managers/ 2> /dev/null | awk '{print \$4}')" == "sysmgrs" ]] && print_PASS && let grade++ || print_FAIL
      echo -n "检查目录权限 ------ "
      [[ "\$(stat -c %a /home/managers/ 2> /dev/null )" == "2770" ]] && print_PASS && let grade++ || print_FAIL
    
      echo -e "\n################## \$VMNAME 第七题 ##################"
      echo "配置对时："
      echo -n "检查chronyd服务是否开机启用并启动 ------ "
      ( systemctl is-enabled chronyd && systemctl is-active chronyd ) &>/dev/null && print_PASS && let grade++ || print_FAIL
      echo -n "检查是否成功对时 ------ "
      grep 'server.*classroom.example.com' /etc/chrony.conf &>/dev/null && print_PASS && let grade++ || print_FAIL
    
      echo -e "\n################## \$VMNAME 第八题 ##################"
      echo "配置Autofs："
      echo -n "检查autofs服务是否开机启用并启动 ------ "
      ( systemctl is-enabled autofs && systemctl is-active autofs ) &>/dev/null && print_PASS && let grade++ || print_FAIL
      echo -n "检查autofs配置 ------ "
      (sudo -u laoma touch /rhome/laoma/\$(basename \$(mktemp)) && df | grep 'classroom.example.com:/rhome/laoma.*/rhome/laoma') &>/dev/null && print_PASS && let grade++ || print_FAIL
    
      echo -e "\n################## \$VMNAME 第九题 ##################"
      echo "配置文件权限："
      echo -n "检查文件拥有者和组拥有者 ------ "
      [[ -f /var/tmp/fstab ]] && [[ "\$(ls -ld /var/tmp/fstab |awk '{print \$4}')" == "root" && "\$(ls -ld /var/tmp/fstab|awk '{print \$3}')" == "root" ]] && print_PASS && let grade++ || print_FAIL
      echo -n "检查文件acl ------ "
      [[ -f /var/tmp/fstab ]] && ( getfacl /var/tmp/fstab | grep 'user:natasha:rw-' && getfacl /var/tmp/fstab | grep 'user:harry:---' && getfacl /var/tmp/fstab | grep 'other::r--' && [[ "\$(ls -l /var/tmp/fstab |cut -c4)" == "-" ]] && [[ "\$(ls -l /var/tmp/fstab |cut -c7)" == "-" ]] && [[ "\$(ls -l /var/tmp/fstab |cut -c10)" == "-" ]] ) &>/dev/null && print_PASS && let grade++ || print_FAIL
    
      echo -e "\n################## \$VMNAME 第十题 ##################"
      echo "配置用户账户："
      echo -n "检查用户id ------ "
      [[ "\$(id -u laoniu)" == "3533" ]] && print_PASS && let grade++ || print_FAIL
      echo -n "检查用户密码passwd ------ "
      FULLHASH=\$(grep '^laoniu:' /etc/shadow | cut -d: -f 2)
      SALT=\$(grep '^laoniu:' /etc/shadow | cut -d'\$' -f3)
      PERLCOMMAND="print crypt(\"redhat\", \"\\\\\\\$6\\\\\\$\${SALT}\");"
      NEWHASH=\$(perl -e "\${PERLCOMMAND}")
      [[ "\${FULLHASH}" == "\${NEWHASH}" ]] && print_PASS && let grade++ || print_FAIL

      echo -e "\n################# \$VMNAME 第十一题 #################"
      echo -n "查找文件 ------ "
      ls /root/findfiles/jacques{1..5} /root/findfiles/jacquesdir &>/dev/null && print_PASS && let grade++ || print_FAIL
  
      echo -e "\n################# \$VMNAME 第十二题 #################"
      echo -n "查找字符串 ------ "
      [[ -f /root/list ]] && grep crosswords /usr/share/doc/words/readme.txt > /tmp/strings && [[ "\$(md5sum /root/list | awk '{print \$1}')" == "\$(md5sum /tmp/strings | awk '{print \$1}')" ]] && rm -f /tmp/strings && print_PASS && let grade++ || print_FAIL
      
      echo -e "\n################# \$VMNAME 第十三题 #################"
      echo -n "创建存档 ------ "
      [[ -f /root/backup.tar.gz ]] && tar tf /root/backup.tar.gz | grep -q '^usr/local' && [[ \$(tar tf /root/backup.tar.gz |grep -v '^usr/local' |wc -l) -eq 0 ]] && print_PASS && let grade++ || print_FAIL

      echo \$grade > /tmp/grade
      ;;

  venus)
      cat <<EOF2 >/etc/yum.repos.d/rhel80-test.repo
[BaseOS]
name=BaseOS
baseurl=http://content.example.com/rhel8.0/x86_64/dvd/BaseOS
enabled=1
gpgcheck=0
[AppStream]
name=AppStream
baseurl=http://content.example.com/rhel8.0/x86_64/dvd/AppStream
enabled=1
gpgcheck=0
EOF2
      yum install -y perl &>/dev/null && rm -f /etc/yum.repos.d/rhel80-test.repo

#用于统计成绩
grade=1
      echo -e "\n################## \$VMNAME 第一题 #################"
      echo -n "设置root密码 ------ "
      FULLHASH=\$(grep '^root:' /etc/shadow | cut -d: -f 2)
      SALT=\$(grep '^root:' /etc/shadow | cut -d'\$' -f3)
      PERLCOMMAND="print crypt(\"redhat\", \"\\\\\\\$6\\\\\\$\${SALT}\");"
      NEWHASH=\$(perl -e "\${PERLCOMMAND}")
      [[ "\${FULLHASH}" == "\${NEWHASH}" ]] && print_PASS && let grade++ || print_FAIL	
  
      echo -e "\n################## \$VMNAME 第二题 #################"
      echo "配置yum仓库： ------ "
      echo -n "检查yum仓库文件语法 ------ "
      [[ \$(yum repolist all 2>/dev/null | grep enabled |wc -l ) -ge 2 ]] && print_PASS && let grade++ || print_FAIL
      echo -n "检查BaseOS仓库配置 ------ "	
      grep -rqs  "http://content.example.com/rhel8.0/x86_64/dvd/BaseOS" /etc/yum.repos.d/ && print_PASS && let grade++ || print_FAIL
      echo -n "检查AppStream仓库配置 ------ "	
      grep -rqs  "http://content.example.com/rhel8.0/x86_64/dvd/AppStream" /etc/yum.repos.d/ && print_PASS && let grade++ || print_FAIL

      echo -e "\n################## \$VMNAME 第三题 #################"
      echo "设置逻辑卷大小："
      echo -n "检查文件系统容量 ------ "
      size=\$(df -h | grep '/dev/mapper/laomavg-laomalv' |awk '{print \$2}' |cut -c1-3)
      [[ \$size -ge 217 && \$size -le 243 ]] && print_PASS && let grade++ || print_FAIL
      echo -n "检查文件系统内容 ------ "
      [[ \$(md5sum /laoma/laoma.txt|awk '{print \$1}') == "8fd8a6300080b99cce28c63b967c6317" ]] && ls -1 /laoma/|egrep '^laoma.txt\$' &>/dev/null && [[ \$(ls -1 /laoma/|egrep -v '^laoma.txt\$'|wc -l) -eq 0 ]] && print_PASS && let grade++ || print_FAIL

      echo -e "\n################## \$VMNAME 第四题 #################"
      echo "添加swap分区："
      swapon -a >/dev/null
      if [[ \$(swapon -s|grep -v Filename|wc -l) -eq 2 ]] && swapon -s|grep /dev/vdb3 &>/dev/null;then
        echo -n "检查新增swap分区是否存在 ------ " && print_PASS && let grade++
        echo -n "检查新增swap分区标签 ------ "	    
        newswap="\$(swapon -s|grep -v Filename|grep -v vdb2|awk '{print \$1}')"
        newswapdisk="\$(swapon -s|grep -v Filename|grep -v vdb2|awk '{print \$1}'|cut -c1-8)"
        fdisk -l \$newswapdisk|grep \$newswap|grep 'Linux swap' &>/dev/null && print_PASS && let grade++ || print_FAIL
        echo -n "检查新增swap分区容量 ------ "
        size="\$(swapon -s|grep -v Filename|grep -v vdb2|awk '{print \$3}')"
        [[ \$size -ge 768000 && \$size -le 779000 ]] && print_PASS && let grade++ || print_FAIL
        echo -n "检查新增swap分区持久过载 ------ "
        swapoff -a && swapon -a && swapon -s | grep \$newswap &>/dev/null && print_PASS && let grade++ || print_FAIL            
      else
        echo -n "检查新增swap分区是否存在 ------ " && print_FAIL
      fi	    

      echo -e "\n################## \$VMNAME 第五题 #################"
      echo "创建逻辑卷："
      if ls /dev/qagroup/qa &>/dev/null;then
        echo -n "检查逻辑卷是否创建 ------ " && print_PASS && let grade++
        echo -n "检查PE大小 ------ "	    
        [[ "\$(vgdisplay qagroup|grep 'PE Size'|awk '{print \$3" "\$4}')" == "16.00 MiB" ]] && print_PASS && let grade++ || print_FAIL
        echo -n "检查PE数量 ------ "
        [[ \$(lvdisplay /dev/qagroup/qa | grep 'Current LE'|awk '{print \$3}') -eq 60 ]] && print_PASS && let grade++ || print_FAIL
        echo -n "检查文件系统格式 ------ "
        blkid /dev/qagroup/qa | grep -q ext3 && print_PASS && let grade++ || print_FAIL
        echo -n "检查新增逻辑卷持久挂载 ------ "
        (umount /mnt/qa && mount /mnt/qa && df |grep '/mnt/qa') &>/dev/null && print_PASS && let grade++ || print_FAIL
      else
        echo -n "检查逻辑卷是否创建 ------ " && print_FAIL
      fi

      echo -e "\n################## \$VMNAME 第六题 #################"
      echo "创建VDO卷："
      echo -n "检查VDO软件包是否安装 ------ "
      [[ \$(yum list installed vdo kmod-kvdo|grep vdo|wc -l) -eq 2 ]] && print_PASS && let grade++ || print_FAIL
      echo -n "检查VDO服务是否开机启用并启动 ------ "
      ( systemctl is-enabled vdo.service && systemctl is-active vdo.service ) &>/dev/null && print_PASS && let grade++ || print_FAIL
      echo -n "检查VDO使用的磁盘 ------ "
      [[ "\$(vdo status|grep 'Storage device' |awk '{print \$3}')" == "/dev/vdc" ]] && print_PASS && let grade++ || print_FAIL
      echo -n "检查VDO卷名称 ------ "
      [[ "\$(vdo list)" == "vdough" ]] && print_PASS && let grade++ || print_FAIL
      echo -n "检查VDO容量 ------ "
      [[ "\$(vdo status -n vdough 2>/dev/null |grep 'Logical size' | awk '{print \$3}')" == "50G" ]] && print_PASS && let grade++ || print_FAIL
      echo -n "检查VDO文件系统格式 ------ "
      blkid|grep '/dev/mapper/vdough'|grep -q xfs && print_PASS && let grade++ || print_FAIL
      echo -n "检查VDO卷持久挂载 ------ "
      (umount /vbread && mount /vbread && df |grep '/vbread' && mount| grep '/dev/mapper/vdough' |grep 'x-systemd.requires=vdo.service') &>/dev/null && print_PASS && let grade++ || print_FAIL

      echo -e "\n################## \$VMNAME 第七题 #################"
      echo -n "配置系统调优 ------ "
      [[ "\$(tuned-adm recommend)" == "\$(tuned-adm active | awk '{print \$4}')" ]] && print_PASS && let grade++ || print_FAIL

      echo \$grade > /tmp/grade
      ;;
  *)
      echo "脚本应该在mars或venus上运行。"
      ;;
esac
EOF

sum=0
all=56
for host in mars venus;
do
  scp /tmp/grade.sh root@$host: &>/dev/null
  ssh root@$host 'chmod +x /root/grade.sh;/root/grade.sh;rm -f /root/grade.sh'
  grade=$[ $(ssh root@$host cat /tmp/grade) - 1 ]
  sum=$[ $sum + $grade ]
  echo
done

echo -e "\n*************************************************"
echo "scale=2;${sum}/${all}*300 " | bc |awk -F'.' '{print $1}' > /tmp/grade
echo -e "******************   成绩：\033[1;31m$(cat /tmp/grade)\033[0;39m  *****************"
echo "*************************************************"
```









# 环境

链接: https://pan.baidu.com/s/1wA7rabTot5P68tvd3uivFg

提取码: iddx

虚拟机密码: redhat



此环境是马老师的 2020年9月份的环境







# 端口转发

vmware player 端口转发

为虚拟机添加一个VMnet8(NAT 模式)的网卡

修改 C:\ProgramData\VMware\vmnetnat.conf 文件并在 [incomingtcp] 下添加

222 = 192.168.25.128:22

此修改将会转发本机的222端口到虚拟机(192.168.25.128)的22端口







# 关于无法进行磁盘解密的问题

> 磁盘“C:\Users\sa\Documents\Virtual Machines\rhce8-202009\rhce8-000006.vmdk”已加密，并且找不到所需的密钥。
>
> 无法对磁盘进行解密，因为密钥或密码错误
>
> 打不开磁盘“C:\Users\sa\Documents\Virtual Machines\rhce8-202009\rhce8-000006.vmdk”或它所依赖的某个快照磁盘。
>
> 模块“Disk”启动失败。
>
> 未能启动虚拟机。

如果遇到以上情况把虚拟机`完全`克隆, 然后使用克隆后的虚拟机解密