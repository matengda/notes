> https://pan.baidu.com/s/1M5DAbBdZFVGKEpV1T0FfiA 提取码: 3su5

虚拟机密码: redhat

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

















vmware player 端口转发

为虚拟机添加一个VMnet8(NAT 模式)的网卡

修改 C:\ProgramData\VMware\vmnetnat.conf 文件并在 [incomingtcp] 下添加

222 = 192.168.25.128:22

此修改将会转发本机的222端口到虚拟机(192.168.25.128)的22端口























1. 配置网络设置

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

2. 