# 考试环境信息

总分为300分, 210分及以上合格



除了使用的物理台式机外, 还会使用多个虚拟系统, 虚拟机具有root权限, 但物理机没有root权限



# 虚拟系统

|            **系统**             |    **IP 地址**     |         **角色**         |
| :-----------------------------: | :----------------: | :----------------------: |
|   **bastion.lab.example.com**   | **172.25.250.254** | **Ansible bastion node** |
|   **servera.lab.example.com**   | **172.25.250.10**  | **Ansible managed node** |
|   **serverb.lab.example.com**   | **172.25.250.11**  | **Ansible managed node** |
|   **serverc.lab.example.com**   | **172.25.250.12**  | **Ansible managed node** |
|   **serverd.lab.example.com**   | **172.25.250.13**  | **Ansible managed node** |
| **workstation.lab.example.com** |  **172.25.250.9**  | **Ansible managed node** |

- |   **bastion**   |  **堡垒**  |
  | :-------------: | :--------: |
  | **workstation** | **工作站** |
  |    **node**     |  **节点**  |
  |   **managed**   |  **管理**  |

- 这些系统的 IP 地址采用静态
- **请勿更改这些设置**
- 主机名称解析已配置为解析上方列出的完全限定主机名, 同时也解析短名称

















# 初始化环境



```bash
# 以下是 rhce8.sh 文件
# 执行 rhce8.sh 文件初始化训练环境

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
for i in classroom bastion workstation server{a..d};
do
    wait_ssh $i
done
echo "All vms is running."

[[ $(id -u) -ne 0 ]] && echo "please run as root" && exit 1
cd /root

echo -n "setup bastion ......"
cat > /tmp/setup_bastion.sh << eof
echo "StrictHostKeyChecking no" >> /etc/ssh/ssh_config
id devops &> /dev/null || useradd devops
echo 'devops ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/devops
eof
rsync /tmp/setup_bastion.sh root@bastion: &> /dev/null
ssh root@bastion 'chmod +x setup_bastion.sh && ./setup_bastion.sh && rm -fr /root/setup_bastion.sh' && rm -f /tmp/setup_bastion.sh
rsync -a ~/.ssh root@bastion:~ && ssh root@bastion chown -R root:root /root/.ssh
ssh root@bastion 'rsync -a /root/.ssh /home/devops && chown -R devops:devops /home/devops/.ssh'
echo "DONE"

[[ -d /content/courses/rh294/rhel8.0/materials/laoma/ ]] || mkdir -p /content/courses/rh294/rhel8.0/materials/laoma/
rsync -a /root/exam_rhce8.zip /content/courses/rh294/rhel8.0/materials/laoma/
cd /content/courses/rh294/rhel8.0/materials/laoma/ && unzip -o exam_rhce8.zip &> /dev/null

echo -n "setup servera and serverb ......"
ssh root@servera vgcreate research /dev/vdb &> /dev/null
ssh root@serverb vgcreate research /dev/vdb &> /dev/null
echo "DONE"
```









# 评分脚本

有两个文件 `grade.sh` 和 `exam-rhce8.sh`

执行exam-rhce8.sh文件即可得分



```bash
# 以下是 grade.sh


#!/bin/bash
#定义函数
function print_PASS () {
  echo -e '\033[1;32m正确\033[0;39m'
}

function print_FAIL() {
  echo -e '\033[1;31m失败\033[0;39m'
}

function print_DONE() {
  echo -e '\033[1;35m完成\033[0;39m'
}

function check_apache_role() {
  echo "检查Apache角色配置: "

  echo -n "    检查软件包httpd是否安装 ------ "
  ansible $1 -a 'yum list httpd' &>/dev/null && print_PASS && let grade++ || print_FAIL

  echo -n "    检查服务httpd是否开机启动并启用 ------ "
  ansible $1 -m shell -a 'systemctl is-enabled httpd && systemctl is-active httpd' &>/dev/null && print_PASS && let grade++ || print_FAIL

  echo -n "    检查软件包firewalld是否安装 ------ "
  ansible $1 -a 'yum list firewalld' &>/dev/null && print_PASS && let grade++ || print_FAIL

  echo -n "    检查服务firewalld是否开机启动并启用 ------ "
  ansible $1 -m shell -a 'systemctl is-enabled firewalld && systemctl is-active firewalld' &>/dev/null && print_PASS && let grade++ || print_FAIL

  echo -n "    检查firewalld是否放行web ------ "
  ansible $1 -m shell -a "firewall-cmd --list-all|egrep 'services:.* http ' && firewall-cmd --list-all --permanent |egrep 'services:.* http '" &>/dev/null && print_PASS && let grade++ || print_FAIL

  echo -n "    检查模板文件index.html.j2及其内容 ------ "
  [[ "$(cat roles/apache/templates/index.html.j2 2>/dev/null )" == "Welcome to {{ ansible_fqdn }} on {{ ansible_default_ipv4.address }}" ]] && print_PASS && let grade++ || print_FAIL

  echo -n "    检查webservers站点内容 ------ "
  (curl serverc |grep  'Welcome to serverc.lab.example.com on 172.25.250.12' && curl serverd |grep  'Welcome to serverd.lab.example.com on 172.25.250.13') &>/dev/null && print_PASS && let grade++ || print_FAIL
}

function check_passwd() {
  user=$1
  passwd=$2
  shadow_file=$3
  
  FULLHASH=$(grep "${user}:" ${shadow_file} | cut -d: -f 2)
  SALT=$(grep "^${user}:" ${shadow_file} | cut -d'$' -f3)
  PERLCOMMAND="print crypt(\"${passwd}\", \"\\\$6\\\$${SALT}\");"
  NEWHASH=$(perl -e "${PERLCOMMAND}")
  [[ "${FULLHASH}" == "${NEWHASH}" ]] && echo 0	
}
#评分部分

#切换到对应目录
cd /home/devops/ansible &>/dev/null || (echo "/home/devops/ansible is not exit" && exit 1)

#定义grade收集成绩
export grade=1

echo -e "\n###################### 第一题 ######################"
echo "Ansible安装和配置： "

echo -n "检查Ansible是否安装 ------ "
if yum list ansible &>/dev/null ;then
  print_PASS && let grade++
else
  print_FAIL
  exit 2
fi

echo -n "检查Ansible配置-ansible.cfg ------ "
[[ -f /home/devops/ansible/ansible.cfg ]] && ansible --version |grep -q 'config file = /home/devops/ansible/ansible.cfg' && print_PASS && let grade++ || print_FAIL

#临时导出ansible配置
ansible-config dump > /tmp/ansible-config

echo -n "检查Ansible配置-roles_path ------ "
[[ -d /home/devops/ansible/roles ]] && cat /tmp/ansible-config|grep DEFAULT_ROLES_PATH | grep -q '/home/devops/ansible/roles' && print_PASS && let grade++ || print_FAIL

echo -n "检查Ansible配置-提权 ------ "
[[ $(ansible all -a id|grep -c root) -eq 5 ]] && print_PASS && let grade++ || print_FAIL

echo -n "检查Ansible配置-inventory ------ "
[[ -f /home/devops/ansible/inventory ]] && cat /tmp/ansible-config | grep DEFAULT_HOST_LIST | grep -q '/home/devops/ansible/inventory' && print_PASS && let grade++ || print_FAIL

#清理临时导出的ansible配置
rm -f /tmp/ansible-config

#临时导出inventory配置
ansible-inventory --graph > /tmp/inventory

echo "检查inventory文件配置: "
echo -n "    检查servera是否在组dev中 ------ "
cat /tmp/inventory | grep -A1 dev | grep servera &>/dev/null && print_PASS && let grade++ || print_FAIL

echo -n "    检查serverb是否在组test中 ------ "
cat /tmp/inventory | grep -A1 test | grep serverb &>/dev/null && print_PASS && let grade++ || print_FAIL

echo -n "    检查serverc和serverd是否在组prod中 ------ "
(cat /tmp/inventory | grep -A2 prod | grep serverc && cat /tmp/inventory | grep -A2 prod | grep serverd ) &>/dev/null && print_PASS && let grade++ || print_FAIL

echo -n "    检查workstation是否在组balancers中 ------ "
cat /tmp/inventory | grep -A1 balancers | grep workstation &>/dev/null && print_PASS && let grade++ || print_FAIL

echo -n "    检查prod是否在组webservers中 ------ "
cat /tmp/inventory | grep -A1 webservers| grep prod &>/dev/null && print_PASS && let grade++ || print_FAIL

#清理临时导出的inventory配置
rm -f /tmp/inventory


echo -e "\n###################### 第二题 ######################"
echo "执行Ansible Ad hoc命令："

echo -n "检查文件adhoc.sh是否存在并且可执行 ------ " 
if [[ -x /home/devops/ansible/adhoc.sh ]];then
  print_PASS && let grade++

  echo -n "应用adhoc.sh脚本 ------ "
  /home/devops/ansible/adhoc.sh &>/dev/null && print_DONE || print_FAIL

  echo -n "检查所有节点EX294_BASE仓库 ------ "
  [[ $(ansible all -a 'cat /etc/yum.repos.d/EX294_BASE.repo' | grep '\[EX294_BASE\]' |wc -l) -eq 5 ]] && [[ $(ansible all -a 'cat /etc/yum.repos.d/EX294_BASE.repo' | grep 'baseurl = http://content.example.com/rhel8.0/x86_64/dvd/BaseOS' |wc -l) -eq 5 ]] && [[ $(ansible all -a 'cat /etc/yum.repos.d/EX294_BASE.repo' | grep 'enabled = 1' |wc -l) -eq 5 ]] && [[ $(ansible all -a 'cat /etc/yum.repos.d/EX294_BASE.repo' | grep 'gpgcheck = 1' |wc -l) -eq 5 ]] && [[ $(ansible all -a 'cat /etc/yum.repos.d/EX294_BASE.repo' | grep 'gpgkey = http://content.example.com/rhel8.0/x86_64/dvd/RPM-GPG-KEY-redhat-release' |wc -l) -eq 5 ]] && [[ $(ansible all -a 'cat /etc/yum.repos.d/EX294_BASE.repo' | grep 'name = EX294 base software' |wc -l) -eq 5 ]] && print_PASS && let grade++ || print_FAIL

  echo -n "检查所有节点EX294_STREAM仓库 ------ "
  [[ $(ansible all -a 'cat /etc/yum.repos.d/EX294_STREAM.repo' | grep '\[EX294_STREAM\]' |wc -l) -eq 5 ]] && [[ $(ansible all -a 'cat /etc/yum.repos.d/EX294_STREAM.repo' | grep 'baseurl = http://content.example.com/rhel8.0/x86_64/dvd/AppStream' |wc -l) -eq 5 ]] && [[ $(ansible all -a 'cat /etc/yum.repos.d/EX294_STREAM.repo' | grep 'enabled = 1' |wc -l) -eq 5 ]] && [[ $(ansible all -a 'cat /etc/yum.repos.d/EX294_STREAM.repo' | grep 'gpgcheck = 1' |wc -l) -eq 5 ]] && [[ $(ansible all -a 'cat /etc/yum.repos.d/EX294_STREAM.repo' | grep 'gpgkey = http://content.example.com/rhel8.0/x86_64/dvd/RPM-GPG-KEY-redhat-release' |wc -l) -eq 5 ]] && [[ $(ansible all -a 'cat /etc/yum.repos.d/EX294_STREAM.repo' | grep 'name = EX294 stream software' |wc -l) -eq 5 ]] && print_PASS && let grade++ || print_FAIL

else
  print_FAIL
fi


echo -e "\n###################### 第三题 ######################"
echo "使用Ansible部署软件："

echo -n "检查文件packages.ym是否存在并且可执行 ------ " 
if [[ -x /home/devops/ansible/adhoc.sh ]];then
  print_PASS && let grade++ 

  echo -n "应用playbook packages.yml ------ "
  ansible-playbook packages.yml &>/dev/null && print_DONE || print_FAIL
 
 echo -n "检查软件包php和mariadb是否正确安装 ------ "  
  (ansible dev,test,prod -a 'yum list php' && ansible dev,test,prod -a 'yum list mariadb') &>/dev/null && print_PASS && let grade++ || print_FAIL

  echo -n "检查软件包组-RPM Development Tools是否正确安装 ------ " 
  ansible dev -a 'yum grouplist --installed "RPM Development Tools"' &>/dev/null && print_PASS && let grade++ || print_FAIL

  echo -n "检查主机组dev上所有软件包是否是最新版本 ------ " 
  [[ $(ansible dev -a 'yum list updates' 2>/dev/null |wc -l) -le 2 ]] && print_PASS && let grade++ || print_FAIL

else
  print_FAIL
fi

echo -e "\n###################### 第四题 ######################"
echo "使用RHEL系统角色："

echo -n "检查软件包rhel-system-roles是否安装 ------ "  
yum list rhel-system-roles &>/dev/null && print_PASS && let grade++ || print_FAIL

echo -n "检查角色timesync是否存在 ------ " 
ansible-galaxy list|grep '\- timesync,' &>/dev/null && print_PASS && let grade++ || print_FAIL

#清理默认配置
ansible all -m shell -a "systemctl disable chronyd && systemctl stop chronyd && sed -i 's/172.25.254.250/172.25.254.200/' /etc/chrony.conf" &>/dev/null

echo -n "检查文件timesync.yml是否存在 ------ "
if [[ -f /home/devops/ansible/timesync.yml ]];then
  print_PASS && let grade++ 

  echo -n "应用playbook timesync.yml ------ "
  ansible-playbook timesync.yml &>/dev/null && print_DONE || print_FAIL

  echo -n "检查chronyd服务是否开机启用并启动 ------ "
  [[ $(ansible all -a 'systemctl is-enabled chronyd' | grep '^enabled$' |wc -l) -eq 5 ]] && [[ $(ansible all -a 'systemctl is-active chronyd' | grep '^active$' |wc -l) -eq 5 ]] && print_PASS && let grade++ || print_FAIL

  echo -n "检查chronyd服务配置 ------ "
  [[ $(ansible all -a "grep '^server 172.25.254.254 iburst$' /etc/chrony.conf"| grep '^server 172.25.254.254 iburst$'|wc -l) -eq 5 ]] && print_PASS && let grade++ || print_FAIL

else
  print_FAIL
fi


echo -e "\n###################### 第五题 ######################"
echo "使用Ansible Galaxy安装角色："

echo -n "检查文件requirements.yml是否存在 ------ "
if [[ -f /home/devops/ansible/roles/requirements.yml ]];then
  print_PASS && let grade++ 

  echo -n "应用playbook requirements.yml ------ "
  ansible-galaxy install -r roles/requirements.yml &>/dev/null && print_DONE || print_FAIL

  echo -n "检查安装的角色名是否正确 ------ "
  ls /home/devops/ansible/roles/{balancer,phpinfo} &>/dev/null && print_PASS && let grade++ || print_FAIL 

  #根据tar包中文件数量判定角色内容是是否一致
  echo -n "检查角色是否下载了正确文件 ------ "
  rm -f haproxy.tar phpinfo.tar && ( wget http://materials.example.com/laoma/haproxy.tar && wget http://materials.example.com/laoma/phpinfo.tar) &>/dev/null
  [[ $(find roles/balancer/ |wc -l) -eq $(tar tf haproxy.tar |wc -l) ]] && [[ $(find roles/phpinfo/ |wc -l) -eq $(tar tf phpinfo.tar |wc -l) ]] && print_PASS && let grade++ || print_FAIL
  #清理文件
  rm -f haproxy.tar phpinfo.tar

else
  print_FAIL
fi


echo -e "\n###################### 第六题 ######################"
echo "创建和使用角色："

echo -n "检查文件newrole.yml是否存在 ------ "
[[ -f /home/devops/ansible/newrole.yml ]] && print_PASS && let grade++ || print_FAIL

echo -n "检查角色apache是否存在 ------ "
[[ -d /home/devops/ansible/roles/apache ]] && print_PASS && let grade++ || print_FAIL

if [[ -f /home/devops/ansible/newrole.yml ]] && [[ -d /home/devops/ansible/roles/apache ]];then
  echo -n "应用playbook newrole.yml ------ "
  ansible-playbook newrole.yml &>/dev/null && print_DONE || print_FAIL

  #根据是否只有serverc和serverd生成/var/www/html/index.html文件判断
  echo -n "检查playbook是否只应用到组webservers ------ "
  [[ $(ansible all -a 'ls /var/www/html/index.html'|grep '^/var/www/html/index.html$' |wc -l) -eq 2 ]] && (ansible serverc -a 'ls /var/www/html/index.html' && ansible serverd -a 'ls /var/www/html/index.html' ) &>/dev/null && print_PASS && let grade++ || print_FAIL
  
  #检查Apache角色配置
  check_apache_role webservers

else
  echo -n "部署本题评分环境 ------ "
  #生成newrole-tmp.yml用于验证apache角色
  cat << EOF > newrole-tmp.yml
---
- name: use apache roles
  hosts: webservers 
  roles:
    - apache
EOF

  #应用newrole-tmp.yml
  ansible-playbook newrole-tmp.yml &>/dev/null && print_DONE || print_FAIL

  #检查Apache角色配置
  check_apache_role webservers

  #清理newrole-tmp.yml
  rm -f newrole-tmp.yml
fi


echo -e "\n###################### 第七题 ######################"
echo "使用来自Ansible Galaxy的角色："

echo -n "检查文件roles.yml是否存在 ------ "
if [[ -f /home/devops/ansible/roles.yml ]];then
  print_PASS && let grade++ 
  echo -n "应用playbook roles.yml ------ "
  ansible-playbook roles.yml &>/dev/null && print_DONE || print_FAIL

  echo "检查balancers角色对应的playbook配置："
  echo -n "    检查balancers角色是否只应用到组balancers ------ "
  ([[ $(ansible all -a 'yum list --installed haproxy'|grep haproxy|wc -l) -eq 1 ]] && ansible balancers -a 'yum list --installed haproxy') &>/dev/null && print_PASS && let grade++ || print_FAIL

  echo -n "    检查balancers角色应用效果 ------ "
  [[ "$(curl workstation 2>/dev/null)" != "$(curl workstation 2>/dev/null)" ]] && print_PASS && let grade++ || print_FAIL

  echo "检查phpinfo角色对应的playbook配置："
  echo -n "    检查phpinfo角色是否只应用到组webservers ------ "
  ([[ $(ansible all -a 'ls /var/www/html/hello.php' |grep '^/var/www/html/hello.php$'|wc -l) -eq 2 ]] && ansible webservers -a 'ls /var/www/html/hello.php') &>/dev/null && print_PASS && let grade++ || print_FAIL

  echo -n "    检查phpinfo角色应用效果 ------ "
  [[ "$(curl workstation/hello.php 2>/dev/null|grep 'Hello PHP World')" != "$(curl workstation/hello.php 2>/dev/null|grep 'Hello PHP World')" ]] && [[ "$(curl workstation/hello.php 2>/dev/null|grep 'PHP Version')" == "$(curl workstation/hello.php 2>/dev/null|grep 'PHP Version')" ]] && print_PASS && let grade++ || print_FAIL

else
  print_FAIL
fi


echo -e "\n###################### 第八题 ######################"
echo "创建和使用逻辑卷："

echo -n "检查文件lv.ym是否存在 ------ "
if [[ -f /home/devops/ansible/lv.yml ]];then
  print_PASS && let grade++
  echo -n "应用playbook lv.ym ------ "
  ansible-playbook lv.yml >/tmp/lv.log 2>/dev/null && print_DONE || print_FAIL

  echo "检查servera逻辑卷配置："
  echo -n "    检查逻辑卷名称 ------ "
  ssh root@servera ls /dev/research/data &>/dev/null && print_PASS && let grade++ || print_FAIL

  echo -n "    检查逻辑卷大小 ------ "
  ssh root@servera lvdisplay /dev/research/data|grep -q 'Current LE.*1500$' && print_PASS && let grade++ || print_FAIL

  echo -n "    检查逻辑卷文件系统格式 ------ "
  ssh root@servera blkid /dev/research/data | grep -q ext4 && print_PASS && let grade++ || print_FAIL

  echo "检查serverb逻辑卷配置："
  echo -n "    检查逻辑卷名称 ------ "
  ssh root@serverb ls /dev/research/data &>/dev/null && print_PASS && let grade++ || print_FAIL

  echo -n "    检查逻辑卷大小 ------ "
  ssh root@serverb lvdisplay /dev/research/data|grep -q 'Current LE.*200$' && print_PASS && let grade++ || print_FAIL

  echo -n "    检查逻辑卷文件系统格式 ------ "
  ssh root@serverb blkid /dev/research/data | grep -q ext4 && print_PASS && let grade++ || print_FAIL

  echo -n "    检查无法创建指定逻辑卷大小时的输出信息 ------ "
  grep -B1 "Could not create logical volume of that size" /tmp/lv.log |grep -q serverb && print_PASS && let grade++ || print_FAIL

  echo -n "检查卷组research不存在时的输出信息 ------ "
  [[ $(grep -B1 "Volume group done not exist" /tmp/lv.log |egrep '(serverc|serverd|workstation)' |wc -l) -eq 3 ]] && print_PASS && let grade++ || print_FAIL

else
  print_FAIL
fi


echo -e "\n###################### 第九题 ######################"
echo "生成主机文件："

echo -n "检查文件hosts.j2是否存在 ------ "
[[ -f /home/devops/ansible/hosts.j2 ]] && print_PASS && let grade++ || print_FAIL

echo -n "检查角色hosts.yml是否存在 ------ "
[[ -f /home/devops/ansible/hosts.yml ]] && print_PASS && let grade++ || print_FAIL

if [[ -f /home/devops/ansible/hosts.j2 ]] && [[ -f /home/devops/ansible/hosts.yml ]] ;then
  echo -n "应用playbook hosts.yml ------ "
  ansible-playbook hosts.yml &>/dev/null && print_DONE || print_FAIL

  echo -n "检查文件hosts.j2是否采用jinja2格式 ------ "
  (egrep '^{% +for.*%}$' hosts.j2 && egrep '^{% +endfor +%}$' hosts.j2) &>/dev/null && print_PASS && let grade++ || print_FAIL

  echo -n "检查文件/etc/myhosts内容 ------ "

  #准备比对文件
  ansible dev -m shell -a 'cat <<EOF > /tmp/myhosts
172.25.250.10 servera.lab.example.com servera
172.25.250.11 serverb.lab.example.com serverb
172.25.250.12 serverc.lab.example.com serverc
172.25.250.13 serverd.lab.example.com serverd
172.25.250.9 workstation.lab.example.com workstation
EOF
' &>/dev/null

  #比对文件内容
  [[ "$(ansible dev -a 'cat /etc/myhosts'|grep '^172'|sort -n)" == "$(ansible dev -a 'cat /tmp/myhosts'|grep '^172')" ]] && print_PASS && let grade++ || print_FAIL

fi


echo -e "\n###################### 第十题 ######################"
echo "修改文件内容："

echo -n "检查文件issue.yml是否存在 ------ "
if [[ -f /home/devops/ansible/issue.yml ]];then
  print_PASS && let grade++ 
  echo -n "应用playbook issue.yml ------ "
  ansible-playbook issue.yml &>/dev/null && print_DONE || print_FAIL

  echo -n "检查主机组dev应用效果 ------ "
  [[ "$(ansible dev -a 'cat /etc/issue' | grep Development)" == "Development" ]] && print_PASS && let grade++ || print_FAIL

  echo -n "检查主机组test应用效果 ------ "
  [[ "$(ansible test -a 'cat /etc/issue' | grep Test)" == "Test" ]] && print_PASS && let grade++ || print_FAIL

  echo -n "检查主机组prod应用效果 ------ "
  [[ "$(ansible serverc -a 'cat /etc/issue' | grep Production)" == "Production" ]] && [[ "$(ansible serverd -a 'cat /etc/issue' | grep Production)" == "Production" ]] && print_PASS && let grade++ || print_FAIL

else
  print_FAIL
fi


echo -e "\n##################### 第十一题 #####################"
echo "创建Web内容目录："

echo -n "检查文件webcontent.yml是否存在 ------ "
if [[ -f /home/devops/ansible/webcontent.yml ]];then
  print_PASS && let grade++ 
  echo -n "应用playbook webcontent.yml ------ "
  ansible-playbook webcontent.yml &>/dev/null && print_DONE || print_FAIL

  echo -n "检查playbook是否只应用到组dev ------ "
  [[ $(ansible all -a 'ls -d /webdev' |grep -B1 '^/webdev$'|egrep '(server|workstation)'|wc -l) -eq 1 ]]&& ansible all -a 'ls -d /webdev' |grep -B1 '^/webdev$'|grep -q servera && print_PASS && let grade++ || print_FAIL

  echo -n "检查目录/webdev权限 ------ "
  [[ "$(ansible dev -a 'ls -ld /webdev' | grep webdev | awk '{print $4}')" == "webdev" ]] && [[ "$(ansible dev -a 'stat -c %a /webdev' |grep -v servera)" == "2775" ]] && print_PASS && let grade++ || print_FAIL

  echo -n "检查目录/var/www/html/webdev链接 ------ "
  ansible dev -a 'ls -ld /var/www/html/webdev' |grep -q '/var/www/html/webdev -> /webdev' && print_PASS && let grade++ || print_FAIL

  echo -n "检查文件/webdev/index.html内容 ------ "
  [[ "$(ansible dev -a 'cat /webdev/index.html' |grep -v servera )" == "Development" ]] && print_PASS && let grade++ || print_FAIL

  echo -n "检查站点访问内容 ------ "
  [[ "$(curl http://servera.lab.example.com/webdev/ 2>/dev/null)" == "Development" ]] && print_PASS && let grade++ || print_FAIL

else
  print_FAIL
fi


echo -e "\n##################### 第十二题 #####################"
echo "生成硬件报告："

echo -n "检查文件hwreport.yml是否存在 ------ "
if [[ -f /home/devops/ansible/hwreport.yml ]];then
  print_PASS && let grade++ 
  echo -n "应用playbook hwreport.yml ------ "
  ansible-playbook hwreport.yml &>/dev/null && print_DONE || print_FAIL

  echo -n "部署本题评分环境 ------ "
  #准备正确的playbook
  cat << EOF >  /tmp/hwreport-new.yml
---
- name: hwreport.txt
  hosts: all
  vars:
    hw_all:
      - hw_name: hostname=
        hw_cont: "{{ inventory_hostname | default('NONE', true) }}"
      - hw_name: memory=
        hw_cont: "{{ ansible_memtotal_mb | default('NONE', true) }}"
      - hw_name: bios_version=
        hw_cont: "{{ ansible_bios_version | default('NONE', true) }}"
      - hw_name: vdasize=
        hw_cont: "{{ ansible_devices.vda.size | default('NONE', true) }}"
      - hw_name: vdbsize=
        hw_cont: "{{ ansible_devices.vdb.size | default('NONE', true) }}"

  tasks:
    - name: download 
      get_url:
        url: http://materials.example.com/laoma/hwreport.empty
        dest: /root/hwreport-new.txt
    - name: deploy hwreport
      lineinfile:
        path: /root/hwreport-new.txt
        regexp: '^{{ item.hw_name }}'
        line: "{{ item.hw_name }}{{ item.hw_cont }}"
      loop: "{{ hw_all }}"
EOF

  #使用正确的playbook，生成正确的比对文件
  ansible-playbook /tmp/hwreport-new.yml &>/dev/null && print_DONE || print_FAIL

  #检查主机应用效果
  for host in server{a..d} workstation
  do
    echo "检查$host上hwreport.txt内容："
    echo -n "    检查$host的清单主机名称 ------ "
    [[ "$(ssh root@$host cat /root/hwreport.txt | grep 'hostname=')" == "$(ssh root@$host cat /root/hwreport-new.txt | grep 'hostname=')" ]] && print_PASS && let grade++ || print_FAIL

    echo -n "    检查$host的总内存大小 ------ "
    [[ "$(ssh root@$host cat /root/hwreport.txt | grep 'memory=')" == "$(ssh root@$host cat /root/hwreport-new.txt | grep 'memory=')" ]] && print_PASS && let grade++ || print_FAIL

    echo -n "    检查$host的BIOS版本 ------ "
    [[ "$(ssh root@$host cat /root/hwreport.txt | grep 'bios_version=')" == "$(ssh root@$host cat /root/hwreport-new.txt | grep 'bios_version=')" ]] && print_PASS && let grade++ || print_FAIL

    echo -n "    检查$host的磁盘设备vda的大小 ------ "
    [[ "$(ssh root@$host cat /root/hwreport.txt | grep 'vdasize=')" == "$(ssh root@$host cat /root/hwreport-new.txt | grep 'vdasize=')" ]] && print_PASS && let grade++ || print_FAIL

    echo -n "    检查$host的磁盘设备vdb的大小 ------ "
    [[ "$(ssh root@$host cat /root/hwreport.txt | grep 'vdbsize=')" == "$(ssh root@$host cat /root/hwreport-new.txt | grep 'vdbsize=')" ]] && print_PASS && let grade++ || print_FAIL
    
  done

else
  print_FAIL
fi


echo -e "\n##################### 第十三题 #####################"
echo "创建密码库："

echo -n "检查文件secret.txt是否存在 ------ "
[[ -f /home/devops/ansible/secret.txt ]] && print_PASS && let grade++ || print_FAIL

echo -n "检查文件secret.txt内容 ------ "
[[ "$( cat /home/devops/ansible/secret.txt)" == "laoma" ]] && print_PASS && let grade++ || print_FAIL

echo -n "检查文件locker.yml是否存在 ------ "
[[ -f /home/devops/ansible/locker.yml ]] && print_PASS && let grade++ || print_FAIL

echo -n "检查文件locker.yml内容 ------ "
ansible-vault view /home/devops/ansible/locker.yml --vault-password-file=/home/devops/ansible/secret.txt |egrep -q 'pw_developer: +Imadev' && ansible-vault view /home/devops/ansible/locker.yml --vault-password-file=/home/devops/ansible/secret.txt |egrep -q 'pw_manager: +Imamgr' && print_PASS && let grade++ || print_FAIL


echo -e "\n##################### 第十四题 #####################"
echo "创建用户帐户："

echo -n "检查文件user_list.yml是否存在 ------ "
[[ -f /home/devops/ansible/user_list.yml ]] && print_PASS && let grade++ || print_FAIL

echo -n "检查文件users.yml是否存在 ------ "
[[ -f /home/devops/ansible/users.yml ]] && print_PASS && let grade++ || print_FAIL

echo -n "部署本题评分环境 ------ "
sudo yum -y module install perl &>/dev/null && print_DONE || print_FAIL

if [[ -f /home/devops/ansible/user_list.yml ]] && [[ -f /home/devops/ansible/users.yml ]];then
  echo -n "应用playbook users.yml ------ "
  ansible-playbook users.yml --vault-password-file=/home/devops/ansible/secret.txt &>/dev/null && print_DONE || print_FAIL

  for SERVER in servera serverb serverc serverd
  do
    if [[ "${SERVER}" == "servera" || "${SERVER}" == "serverb" ]];then
      user=john
      passwd=Imadev
      group=devops
	  
      echo "检查主机${SERVER}中用户和组配置："
      echo -n "    检查组${group}名称 ------ "
      ansible ${SERVER} -a 'cat /etc/group'|grep -q "^${group}:" && print_PASS && let grade++ || print_FAIL
	  
      echo -n "    检查用户${user}名称 ------ "
      ansible ${SERVER} -a "id ${user}" &>/dev/null && print_PASS && let grade++ || print_FAIL
	  
      echo -n "    检查用户${user}补充组 ------ "
      [[ "$(ansible ${SERVER} -a 'cat /etc/group'|grep ${user}|egrep -v "^[[:alnum:]]*${user}[[:alnum:]]*:"|awk -F ':' '{print $1}')" == "${group}" ]] && print_PASS && let grade++ || print_FAIL
	  
      echo -n "    检查用户${user}密码 ------ "
      ansible ${SERVER} -m fetch -a 'src=/etc/shadow dest=/tmp' &>/dev/null
	  [[ "$(check_passwd ${user} ${passwd} /tmp/${SERVER}/etc/shadow)" -eq 0 ]] && print_PASS && let grade++ || print_FAIL
    
	else
      user="james mary"
      passwd=Imamgr
      group=opsmgr
	  
	  echo "检查主机${SERVER}中用户和组配置："
      echo -n "    检查组${group}名称 ------ "
      ansible ${SERVER} -a 'cat /etc/group'|grep -q "^${group}:" && print_PASS && let grade++ || print_FAIL
	  
	  for username in ${user}
	  do
        echo -n "    检查用户${username}名称 ------ "
        ansible ${SERVER} -a "id ${username}" &>/dev/null && print_PASS && let grade++ || print_FAIL
	    
        echo -n "    检查用户${username}补充组 ------ "
        [[ "$(ansible ${SERVER} -a 'cat /etc/group'|grep ${username}|egrep -v "^[[:alnum:]]*${username}[[:alnum:]]*:"|awk -F ':' '{print $1}')" == "${group}" ]] && print_PASS && let grade++ || print_FAIL
	    
        echo -n "    检查用户${username}密码 ------ "
        ansible ${SERVER} -m fetch -a 'src=/etc/shadow dest=/tmp' &>/dev/null
	    [[ "$(check_passwd ${username} ${passwd} /tmp/${SERVER}/etc/shadow)" -eq 0 ]] && print_PASS && let grade++ || print_FAIL
	  done
	fi
  done

else
  print_FAIL
fi

echo -e "\n##################### 第十五题 #####################"
echo "更新Ansible库的密钥："

echo -n "检查文件salaries.yml是否存在 ------ "
[[ -f /home/devops/ansible/salaries.yml ]] && print_PASS && let grade++ || print_FAIL

#准备新的密码文件
echo redhat > /tmp/secret-new.txt

echo -n "检查文件salaries.yml加密密码 ------ "
[[ "$(ansible-vault view salaries.yml --vault-password-file=/tmp/secret-new.txt)" == "laoma" ]] && print_PASS && let grade++ || print_FAIL

#定义考点数量变量
all=119
echo -e "\n****************************************************"
echo "scale=2;(${grade}-1)/${all}*300 " | bc |awk -F'.' '{print $1}' > /tmp/grade
echo -e "*******************   成绩： \033[1;31m$(cat /tmp/grade)\033[0;39m  ******************"
echo "****************************************************"
```



















```bash
# 以下是 exam-rhce8.sh


#!/bin/bash
function wait_ssh {
WAITIME=0
while true; do
  if $(echo "laoma" >/dev/tcp/$1/22) &>/dev/null; then
    echo "  $1 is running."
    break
  else
    if [[ "$1" == "mars" ]];then
      VMNAME=servera
    else
      VMNAME=serverb
    fi
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

function print_DONE() {
  echo -e '\033[1;35m完成\033[0;39m'
}

echo -n "重置节点：workstation servera serverb serverc serverd ------ "
for host in workstation servera serverb serverc serverd
do
  rht-vmctl fullreset $host -q &>/dev/null
done && print_DONE

echo -n "检查节点是否运行：workstation servera serverb serverc serverd ------ "
for host in workstation server{a..d}
do
  wait_ssh $host &>/dev/null
done

#删除原先的vdc磁盘
sudo rm /var/lib/libvirt/images/rh294-servera-vdc.{ovl,qcow2}

#给servera添加vdc磁盘，容量5G
( sudo qemu-img create -f qcow2 /var/lib/libvirt/images/rh294-servera-vdc.qcow2 5G && sudo qemu-img create -f qcow2 -b /var/lib/libvirt/images/rh294-servera-vdc.qcow2 /var/lib/libvirt/images/rh294-servera-vdc.ovl 5G && sudo virsh attach-disk --domain servera --source /var/lib/libvirt/images/rh294-servera-vdc.ovl --target vdc --subdriver qcow2 --live --config ) &> /dev/null


#为servera和serverb创建卷组research
while :
do
  sleep 3
  if ssh root@servera ls /dev/vdc &> /dev/null;then
    ssh root@servera vgcreate research /dev/vdb /dev/vdc &> /dev/null && break
  fi
done
ssh root@serverb vgcreate research /dev/vdb &> /dev/null && print_DONE


echo -e "\n********************* 成 * 绩  *********************"

for host in bastion
do
  scp /home/kiosk/grade.sh devops@$host: &>/dev/null
  ssh devops@$host 'chmod +x ~/grade.sh;~/grade.sh;rm -f ~/grade.sh'
done
```

