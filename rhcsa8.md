# 考试环境信息

总分为300分, 210分及以上合格



除了使用的物理台式机外, 还会使用多个虚拟系统, 虚拟机具有root权限, 物理机没有root权限



## 虚拟系统

所有的虚拟系统都位于 **172.25.250.0/255.255.255.0** 的子网中

子网中的所有系统都位于 **lab.example.com**

除非另有指定, 否则密码默认为 **redhat**



|         系统          |    ip地址     |
| :-------------------: | :-----------: |
| mars.lab.example.com  | 172.25.250.10 |
| venus.lab.example.com | 172.25.250.11 |

- 练习题都是在 **mars.lab.example.com** 上执行的

- | **mars**  |  **火星**  |
  | :-------: | :--------: |
  | **venus** |  **金星**  |
  |  **lab**  | **实验室** |



## 软件存储库地址

http://content.example.com/rhel8.0/x86_64/dvd/BaseOS

http://content.exmple.com/rhel8.0/x86_64/dev/AppStream



## 重要信息

- 虚拟系统会在重新引导后进行评测
- **务必确保所有更改在虚拟机重启后人有保留**
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







# 初始化测试环境

将 rhcas8.sh 上传到 foundation0 的 root家目录, 执行并初始化练习环境

```sh
chmod +x rhcsa8.sh && ./rhcsa8.sh
```



```bash
# 这是 rhcsa8.sh 文件

#!/bin/bash

function wait_ssh {
WAITIME=0
while true; do
	if $(echo "laoma" >/dev/tcp/$1/22) &>/dev/null; then
        echo "  $1 is running."
		break
	else
		VMNAME=$1
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

echo -n "fullreset classroom ... ... "
echo y | rht-vmctl fullreset classroom &> /dev/null
echo "DONE"

echo -n "fullreset all ... ... "
echo y | rht-vmctl fullreset all &> /dev/null
echo "DONE"

echo -n "Waiting for all vms starting ..... "
sleep 60
echo 
for i in classroom bastion workstation server{a,b};
do
    wait_ssh $i
done
echo "All vms is running."

if [[ `id -u` -ne 0 ]];then
    echo "please run as root..."
    exit 1
fi
##### prepare foundation ##########
echo -n "prepare foundation ... ... "
sed -i -e 's/servera/mars/g' -e 's/serverb/venus/g' /etc/hosts
echo DONE

##### prepare classroom #####
echo -n "prepare classroom ... ... "
cat > /tmp/prepare_classroom.sh <<eof
mkdir /rhome
useradd laoma -d /rhome/laoma -u 2001
echo redhat | passwd --stdin laoma
echo laoma > /rhome/laoma/laoma.txt
echo '/rhome	*(rw,sync)' > /etc/exports
systemctl enable nfs-server.service --now
firewall-cmd --set-default-zone=trusted

eof
scp /tmp/prepare_classroom.sh root@classroom: &> /dev/null && rm -f /tmp/prepare_classroom.sh 
ssh root@classroom 'chmod +x /root/prepare_classroom.sh;/root/prepare_classroom.sh; rm -f /root/prepare_classroom.sh' &> /dev/null
echo DONE

##### prepare mars #####
echo -n "prepare mars ... ... "
cat > /tmp/prepare_mars.sh <<eof
sed -i -e 's/servera/mars/g' -e 's/serverb/venus/g' /etc/hosts

yum install -y httpd
rm -fr /etc/yum.repos.d/rhel_dvd.repo
echo 'hello laoma' > /var/www/html/file1
chcon -t samba_share_t /var/www/html/file1
sed -i 's/^Listen 80/Listen 82/' /etc/httpd/conf/httpd.conf

sed -i '/server 172.25.254.254 iburst/d' /etc/chrony.conf
systemctl restart chronyd.service

useradd laoma -M -d /rhome/laoma -u 2001
echo redhat | passwd --stdin laoma

useradd laoniu
useradd jacques
rm -fr /home/jacques /var/spool/mail/jacques
touch /tmp/jacques-file{1..10}
chown jacques /tmp/jacques-file{1..10}

nmcli connection modify "Wired connection 1" ipv4.address 172.25.250.20/24
nmcli connection up "Wired connection 1" &

eof
scp /tmp/prepare_mars.sh root@mars:  &> /dev/null && rm -f /tmp/prepare_mars.sh
ssh root@mars chmod +x /root/prepare_mars.sh  &> /dev/null
(ssh root@mars /root/prepare_mars.sh  &> /dev/null ) & 
echo DONE

##### prepare venus #####
echo -n "prepare venus ... ... "
cat > /tmp/prepare_venus.sh <<eof
hostnamectl set-hostname venus.lab.example.com
sed -i -e 's/servera/mars/g' -e 's/serverb/venus/g' /etc/hosts
rm -fr /etc/yum.repos.d/rhel_dvd.repo

echo "n
p
1

+256M
t
8e
n
p
2

+256M
t
2
82
w" | fdisk /dev/vdb
vgcreate laomavg /dev/vdb1 
lvcreate -L 128M -n laomalv laomavg
mkfs.xfs /dev/laomavg/laomalv
mkdir /laoma
echo "/dev/laomavg/laomalv /laoma xfs defaults 0 0" >> /etc/fstab
mount -a

mkswap /dev/vdb2
echo "/dev/vdb2 swap swap defaults 0 0" >> /etc/fstab 
swapon -a

tuned-adm profile desktop

eof
scp /tmp/prepare_venus.sh root@venus:  &> /dev/null && rm -f /tmp/prepare_venus.sh
ssh root@venus 'chmod +x /root/prepare_venus.sh;/root/prepare_venus.sh;rm -f /root/prepare_venus.sh'  &> /dev/null
echo DONE
```













# 1. 配置网络设置

将mars配置为具有以下网络配置:

- 主机名: mars.lab.example.com

  ```bash
  hostnamectl set-hostname mars.lab.example.com
  ```

- ip地址: 172.25.250.10

- 子网掩码: 255.255.255.0

- 网关: 172.25.250.254

- dns服务器 172.25.250.254





```bash
  nmcli connection modify 'Wired connection 1' ipv4.method manual ipv4.address 172.25.250.10/24 ipv4.gateway 172.25.0.254 ipv4.dns 172.25.250.254
  # ipv4.method manual 更改ip获取方式为手动
  # connection.autoconnect yes 自动连接
  # W 大写
```
  ```bash
  nmcli connection up 'Wired connection 1'
  # 设置完必须输入此命令以激活新的配置
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

  不得删除或以其他方式改变现有的文件内容

- Web 服务器在端口 82 上提供此内容

- Web 服务器在系统启动时自动启动





```sh
semanage port -a -t http_port_t 82 -p tcp
```



```sh
semanage port -l | grep 82
```



```sh
resotrecon -Rv /var/www/html
```



```sh
systemctl restart httpd
```



```sh
firewall-cmd --add-port=82/tcp --permanent
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
# 创建 natasha 用户, 并从组 sysadms
```



```sh
useradd harry -G sysadms
# 创建 harry 用户, 并从组 sysadms
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







# 评分脚本

```bash
# 以下是是 test-rhcsa8.sh 文件
# 直接执行即可得分

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

链接: https://pan.baidu.com/s/1leLa__r0fdteKZrvqX8T0g 

提取码: pii8

虚拟机密码: redhat







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