# description

/etc/fstab 是启动时的配置文件不过，实际 filesystem 的挂载是记录到 /etc/mtab 与 /proc/mounts 这两个文件当中的。每次我们在更动 filesystem 的挂载时，也会同时更动这两个文件



系统挂载的一些限制：

- 根目录 / 是必须挂载的﹐而且一定要先于其它 mount point 被挂载进来。
- 其它 mount point 必须为已创建的目录﹐可任意指定﹐但一定要遵守必须的系统目录架构原则
- 所有 mount point 在同一时间之内﹐只能挂载一次。
- 所有 partition 在同一时间之内﹐只能挂载一次。
- 如若进行卸除﹐您必须先将工作目录移到 mount point(及其子目录) 之外。





<device>	<mountpoint>	<type>	<options>	<dump>	<pass>

- type 文件系统类型, 如: ext2, xfs

- options 文件系统参数, 默认为 defaults

  - `defaults` 使用默认设置, ext4 默认参数为
    - rw,suid,dev,exec,auto,nouser,async
  - `auto` 在启动或在终端中输入mount -a时自动挂载
  - `noauto` 设备（分区）只能手动挂载
  - `ro` 挂载为只读权限
  - `rw` 挂载为读写权限
  - `exec` 是一个默认设置项，它使在那个分区中的可执行的二进制文件能够执行
  - `noexec` 二进制文件不允许执行
    - 不要在你的root分区中用这个选项
  - `sync` 所有的I/O将以同步方式进行
  - `async` 所有的I/O将以非同步方式进行
  - `user` 允许任何用户挂载设备
  - `nouser` 只允许root用户挂载, 这是默认设置
  - `suid` 允许 suid 和 sgid 位
  - `nosuid` 阻止 suid 和 sgid 位
  - `noatime` 不更新文件系统上 inode 访问记录, 可以提升性能
  - `nodiratime` 不更新文件系统上的目录 inode 访问记录, 可以提升性能
  - `relatime` 实时更新 inode access 记录, 只有在记录中的访问时间早于当前访问才会被更新(与 noatime 相似，但不会打断如 mutt 或其它程序探测文件在上次访问后是否被修改的进程), 可以提升性能
  - `flush` vfat文件系统的选项, 更频繁的刷新数据, 复制对话框或进度条在全部数据都写入后才消失

- dump 挂载后的文件系统能否被dump备份命令作用；

  - 0表示不能
  - 1表示每天都进行dump备份
  - 2表示不定期进行dump操作

- pass 开机过程中是否校验扇区, fsck命令

  启动的过程中，系统默认会以fsck检验我们的 filesystem 是否完整 (clean)。 不过，某些 filesystem 是不需要检验的，例如内存置换空间 (swap) ，或者是特殊文件系统例如 /proc 与 /sys 等等。fsck会检查这个头目下的数字来决定检查文件系统的顺序

  - 0 不要检查
  - 1 最早检验, 一般只有根目录会配置为 1
  - 2  也要检验，1级别校验完成后再进行校验
  - 一般来说,根目录配置为1,其他的要检验的filesystem都配置为 2









# example

```sh
UUID=8c2cd88f-6dbd-4ca9-8ad7-88d310ae4ec9 /               xfs     defaults        0 0
# <device>	<mountpoint>	<type>	<options>	<dump>	<pass>
```

 



# name

fstab - static information about the filesystems

