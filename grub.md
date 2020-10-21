启动win10

```sh
set root=(hd0,gpt1) # 这步可以省略
```

```sh
chainloader (hd0,gpt1)/efi/Microsoft/Boot/bootmgfw.efi
```

```sh
boot
```





忘记密码

在启动的时候按e, 编辑启动选项

```sh
console=tty0 rd.break
# 在 linux 开头的行, 末尾加入如上
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
echo abc | passwd --stdin root
```

```sh
touch /.autorelabel
# 默认使用 SELinux，因此创建下面的隐藏文件，这个文件会在下一次启动时重新标记所有文件
```

```sh
C-d C-d
# 重新启动
```











# grub2-mkpasswd-pbkdf2

```sh
grub2-mkpasswd-pbkdf2
# Enter password: 
# Reenter password: 
```

- 如果密码是 abc 则显示
- PBKDF2 hash of your password is grub.pbkdf2.sha512.10000.B5A7CC6D4FDF8DFE4AEA0B02A348A1BC8AF9F2F4EBAD5074AAC5929C3B27C5262B02C054F972D7E97DDCEA43B921F987613D7FE18C7E32C5E013FE6F6B4C47B4.DEB91B5EF8F9FE9957762ABCB53F7D8139BB0DF6B98A83FF0F803765713B463781BE0359DE3B85757CC338E17A7DA0C793B179B93B5ECB124CA69A8C0F01CE8A













# grub2-setpassword

设置grub2的启动密码

```sh
grub2-setpassword
# Enter password:
# Confirm password:
# 直接命令和密码一起输不行, 还是让输入2次密码
```



# 内置变量

## cmdpath

当前被加载的"core.img"（bios的core.img，uefi的BOOTX64.EFI或grubx64.efi等镜像）所在目录的绝对路径。例如：UEFI启动可能是'(hd0,gpt1)/EFI/GRUB或'(cd0)/EFI/BOOT'，BIOS启动可能是'(hd0)'



## prefix

这个变量是指GRUB2的安装目录，即绝对路径形式的'/boot/grub'目录位置，如例如'(hd0,gpt1)/grub'或'(hd0,msdos2)/boot/grub'。初始值由GRUB在启动时根据"grub-install"在安装时提供的信息自动设置，这些信息是由grub-install脚本直接写入到grub镜像（bios的core.img，uefi的BOOTX64.EFI或grubx64.efi等镜像）的，当然，也可以在grub的rescue模式下用grub的命令直接修改，如:set prefix=(hd1,gpt2)/grub。

GRUB2的安装目录中存在着这么几个文件夹和文件

文件夹：

fonts:存在着一些字体

locale:区域化相关

x86_64-efi:模块的二进制文件，以.mod为后缀。你insmod所操作的模块，都在这里面有，比如ext4.mod、fat.mod这些文件系统模块，gzio等解压支持的模块。

themes:一些主题，就是你grub界面的背景啥的。

文件：

grub.cfg:这是grub的启动shell脚本，对于用户来说，是最重要的文件，几乎是用户配置grub的唯一的配置文件。通过这个文件用户可以控制grub加载操作系统的行为，比如添加一个menuentry就是添加一个操作系统启动选项，每个选项中可以指定操作系统内核尽享和initramfs镜像等等





## root

root变量一般在grub.cfg执行的过程中设定的，指定了后续脚本中所有文件的其实位置，比如root=(hd0,gpt3)，那么/boot/xxx.img 就是在第一块硬盘的第三个分区的boot目录下，如果boot是单独的分区，比如是第2块硬盘的第2个分区，那么就是 `set root=(hd1,gpt2)； linux vmlinuz-linux；` 这两行代码来启动一个linux内核了