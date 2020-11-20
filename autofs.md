# files

| **/etc/auto.master** |      **主配置文件**      |
| :------------------: | :----------------------: |
|  **/etc/auto.misc**  | **光盘 自动挂配置文件**  |
|  **/etc/auto.net**   | **nfs自 动挂载配置文件** |



autofs服务程序的**主配置文件**中需要按照**“挂载目录 子配置文件”**的格式写入参数

- ```sh
  vim /etc/auto.master
  ```

- ```sh
  /home /etc/auto.netdir
  # /home 是挂载目录
  # /etc/auto.netdir 是子配置文件
  ```

  





挂载目录是设备要**挂载位置的上一级目录**

- ```sh
  /home /etc/auto.netdir
  # 如果需要挂载 /home/sa 目录, 则只需要写 /home 即可
  # 自动挂载文件名必须以 auto 开头
  ```







对应的子配置文件则是**对这个目录内挂载设备信息的进一步说明**

- ```sh
  sa -fstype=nfs,rw,sync 172.31.0.1:/home/sa
  # /etc/auto.netdir
  ```

  





**子配置文件**中应按照**“挂载目录 挂载文件类型及权限 :设备名称”**的格式写入参数

- ```sh
  sa -fstype=nfs,rw,sync 172.31.0.1:/home/sa
  # sa 是挂载目录
  # -fstype 是 挂载文件类型及权限
  # 172.31.0.1:/home/sa 是 设备地址
  ```









# 简介



autofs - Service control for the automounter





与mount的不同，autofs是一个守护进程，如果它检测到用户正在访问一个尚未挂载的文件系统，如果存在，autofs会自动将其挂载；如果它检测到某个已经挂载的文件系统在一段时间内没有被使用，那么autofs会自动将其卸载





