编辑 -> 虚拟网络编辑器 -> 更改设置(C)

VMnet8 -> 子网 IP(I): 172.31.0.0

​                   子网掩码(M): 255.255.255.0

DHCP 设置 -> 起始 IP 地址(S): 172.31.0.1

​                         结束IP 地址(E): 172.31.0.253

​                         默认租用时间(D): 63    23    59

​                         最长租用时间(M): 63    23    59

NAT 设置 -> 网关 IP(G): 172.31.0.254

​                      端口转发 -> 添加

​                      主机端口(H): 22

​                      虚拟机 IP 地址(A): 172.31.0.1

​                      虚拟机端口(P): 22

​                      描述(D): ssh









设置 -> CD/DVD(IDE) -> 使用 ISO 硬盘映像文件(M): 

C:\Program Files (x86)\VMware\VMware Workstation\linux.iso

```bash
mount /dev/sr0 /mnt
```

```bash
cp /mnt/VMwareTools-10.3.21-14772444.tar.gz ~
```

```bash
tar xvf VMwareTools-10.3.21-14772444.tar.gz
```

```bash
cd vmware-tools-distrib
```

```bash
dnf update -y && dnf install perl -y
```

- The installer has detected an existing installation of open-vm-tools packages 
  on this system and will not attempt to remove and replace these user-space 
  applications. It is recommended to use the open-vm-tools packages provided by 
  the operating system. If you do not want to use the existing installation of 
  open-vm-tools packages and use VMware Tools, you must uninstall the 
  open-vm-tools packages and re-run this installer.
  The packages that need to be removed are:
  open-vm-tools
  The installer will next check if there are any missing kernel drivers. Type yes
  if you want to do this, otherwise type no [yes] `RET`

- Creating a new VMware Tools installer database using the tar4 format.

  Installing VMware Tools.

  In which directory do you want to install the binary files? 
  [/usr/bin] `RET`

- What is the directory that contains the init directories (rc0.d/ to rc6.d/)? 
  [/etc/rc.d] `RET`

- What is the directory that contains the init scripts? 
  [/etc/rc.d/init.d] `RET`

- In which directory do you want to install the daemon files? 
  [/usr/sbin] `RET`

- In which directory do you want to install the library files? 
  [/usr/lib/vmware-tools] `RET`

- The path "/usr/lib/vmware-tools" does not exist currently. This program is 
  going to create it, including needed parent directories. Is this what you want?
  [yes] `RET`

- In which directory do you want to install the documentation files? 
  [/usr/share/doc/vmware-tools] `RET`

- The path "/usr/share/doc/vmware-tools" does not exist currently. This program 
  is going to create it, including needed parent directories. Is this what you 
  want? [yes] `RET`

- The installation of VMware Tools 10.3.21 build-14772444 for Linux completed 
  successfully. You can decide to remove this software from your system at any 
  time by invoking the following command: "/usr/bin/vmware-uninstall-tools.pl".

  Before running VMware Tools for the first time, you need to configure it by 
  invoking the following command: "/usr/bin/vmware-config-tools.pl". Do you want 
  this program to invoke the command for you now? [yes] `RET`

- You have chosen to install VMware Tools on top of an open-vm-tools package.  
  You will now be given the option to replace some commands provided by 
  open-vm-tools.  Please note that if you replace any commands at this time and 
  later remove VMware Tools, it may be necessary to re-install the open-vm-tools.

  The file /usr/bin/vmware-hgfsclient that this program was about to install 
  already exists.  Overwrite? [no] `RET`

- The file /usr/bin/vmhgfs-fuse that this program was about to install already 
  exists.  Overwrite? [no] `RET`

- Initializing...


  Making sure services for VMware Tools are stopped.

  Stopping vmware-tools (via systemctl):                     [  OK  ]


  The module vmci has already been installed on this system by another installer 
  or package and will not be modified by this installer.

  The module vsock has already been installed on this system by another installer
  or package and will not be modified by this installer.

  The module vmxnet3 has already been installed on this system by another 
  installer or package and will not be modified by this installer.

  The module pvscsi has already been installed on this system by another 
  installer or package and will not be modified by this installer.

  The module vmmemctl has already been installed on this system by another 
  installer or package and will not be modified by this installer.

  The VMware Host-Guest Filesystem allows for shared folders between the host OS 
  and the guest OS in a Fusion or Workstation virtual environment.  Do you wish 
  to enable this feature? [yes] `RET`

- The vmxnet driver is no longer supported on kernels 3.3 and greater. Please 
  upgrade to a newer virtual NIC. (e.g., vmxnet3 or e1000e)


  Skipping configuring automatic kernel modules as no drivers were installed by 
  this installer.


  Skipping rebuilding initrd boot image for kernel as no drivers to be included 
  in boot image were installed by this installer.

  The configuration of VMware Tools 10.3.21 build-14772444 for Linux for this 
  running kernel completed successfully.

  Warning no default label for /tmp/vmware-block-restore-1975.0/tmp_file
  VMware Tools installed on top of open-vm-tools has not added anything of 
  significance or potential benefit.  VMware Tools is not needed.

  Would you like to recover the wasted disk space by uninstalling VMware Tools at
  this time? (yes/no) [yes] `RET`

- Uninstalling the tar installation of VMware Tools.

  Stopping services for VMware Tools

  Stopping vmware-tools (via systemctl):                     [  OK  ]

  The removal of VMware Tools 10.3.21 build-14772444 for Linux completed 
  successfully.  Thank you for having tried this software.







扩展vmdk磁盘容量

```powershell
vmware-vdiskmanager -x 100G fileName.vmdk
```







启动时进入bios

虚拟机 -> 电源 -> 打开电源时进入固件(F)