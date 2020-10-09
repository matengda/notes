增加磁盘容量

```sh
gmap show
# =>        3  209715189  da0  GPT  (100G)
#           3        111    1  freebsd-boot  (56K)
#         114    2097152    2  freebsd-swap  (1.0G)
#     2097266  165674886    3  freebsd-ufs  (79G)
#   167772152   41943040       - free -  (20G)
```

```sh
gmap recover da0
# da0 recovered
```

```sh
gmap resize -i 3 da0
# da0p3 resized
# 如果不使用-s参数指定大小则把磁盘全部剩余空间分配到当前的index
```

```sh
growfs /dev/gpt/rootfs
# Device is mounted read-write; resizing will result in temporary write suspension for /.
# It's strongly recommended to make a backup before growing the file system.
# OK to grow filesystem on /dev/gpt/rootfs, mounted on /, from 79GB to 99GB? [yes/no]
```

```sh
super-block backups (for fsck_ffs -b #) at:
# 166691392, 167973632, 169255872, 170538112, 171820352, 173102592, 174384832, 175667072, 176949312, 178231552, 179513792,
# 180796032, 182078272, 183360512, 184642752, 185924992, 187207232, 188489472, 189771712, 191053952, 192336192, 193618432,
# 194900672, 196182912, 197465152, 198747392, 200029632, 201311872, 202594112, 203876352, 205158592, 206440832
```





# link

www.mozilla.org/en-US/firefox/organizations/all
firefox-esr

virtualbox.org
Oracle VirtualBox是由德国InnoTek软件公司出品的虚拟机软件，现在则由甲骨文公司进行开发，是甲骨文公司xVM虚拟化平台技术的一部分。它提供用户在32位或64位的Windows、Solaris及Linux 操作系统上虚拟其它x86的操作系统。用户可以在VirtualBox上安装并且运行Solaris、Windows、DOS、Linux、OS/2 Warp、OpenBSD及FreeBSD等系统作为客户端操作系统。

www.7-zip.org
7z解压缩

www.audacityteam.org
Audacity是一款跨平台的音频编辑软件，用于录音和编辑音频，是自由、开放源代码的软件

gimp.org
位图图像编辑器

llvm.org
LLVM，一个自由软件项目，是一种编译器的基础建设，以C++写成。它是为了任意一种编程语言写成的程序，利用虚拟技术，创造出编译时期，链接时期，运行时期以及“闲置时期”的最优化。它最早是以C/C++为实现对象，目前它支持了包括ActionScript、Ada、D语言、Fortran、GLSL、Haskell、Java bytecode、Objective-C、Swift、Python、Ruby、Rust、Scala以及C#

clang.llvm.org
Clang 是一个C、C++、Objective-C和Objective-C++编程语言的编译器前端。它采用了底层虚拟机（LLVM）作为其后端。它的目标是提供一个GNU编译器套装（GCC）的替代品

tightvnc.com
VNC的实现,VNC（Virtual Network Computing），为一种使用RFB协议的屏幕画面分享及远程操作软件

eclipse.org
Eclipse是著名的跨平台开源集成开发环境（IDE）。Eclipse的本身只是一个框架平台，但是众多插件的支持，使得Eclipse拥有较佳的灵活性，所以许多软件开发商以Eclipse为框架开发自己的IDE

subversion.apache.org
SVN(subversion)版本控制器

postgresql.org
PostgreSQL是自由的对象-关系型数据库服务器（数据库管理系统），在灵活的BSD-风格许可证下发行。它在其他开放源代码数据库系统（比如MySQL和Firebird），和专有系统比如Oracle、Sybase、IBM的DB2和Microsoft SQL Server之外，为用户又提供了一种选择

curl.haxx.se
cURL是一个利用URL语法在命令行下工作的文件传输工具，1997年首次发行。它支持文件上传和下载，所以是综合传输工具，但按传统，习惯称cURL为下载工具。cURL还包含了用于程序开发的libcurl

tmux.github.io
tmux 是一个优秀的终端复用器类自由软件，功能类似 GNU Screen，但使用 BSD 许可发布。用户可以通过 tmux 在一个终端内管理多个分离的会话，窗口及面板，对于同时使用多个命令行，或多个任务时非常方便

ethanschoonover.com/solarized
保护眼睛配色方案

github.com/seebi/dircolors-solarized
GNU ls的颜色主题（由GNU dircolors设置）

github.com/wklken/k-tmux
k-tmux,tmux配置

freebsd.org/doc
freebsd doc

spacemacs.org
github.com/syl20bnr/spacemacs
Spacemacs 是一份 emacs 的配置文件，可以从vim无缝的迁移到emacs并且可以提供一整套插件解决方案

https://ethanschoonover.com/solarized/
护眼主题

mooz.github.io/org-js
github.com/mooz/org-js
用JavaScript编写的org-mode解析器和转换器

libevent.org
libevent是一个异步事件处理软件函式库，以BSD许可证发布

he.net
Hurricane Electric 是一家位于美国的全球互联网服务提供商。该公司提供IPv4和IPv6接入（dns）以及位于美国圣荷西（公司总部地址）的数据中心服务

duckduckgo.com
DuckDuckGo是一个互联网搜索引擎，其总部位于美国宾州保利市。DuckDuckGo着重在传统搜索引擎的基础上引入各大Web 2.0网站（如维基百科）的内容，主张维护用户的隐私权，并承诺不监控、不记录用户的搜索内容。

marc.info
mailing list archives

letsencrypt.org
免费SSL证书

mozilla.github.io/server-side-tls/ssl-config-generator
HTTPS 配置文件自动生成器，支持 Apache，Nginx 等多种服务器

freenode.net
到2010年为止，它是世界上最大的IRC网络

elpa.gnu.org
GNU Emacs Lisp Package Archive

stable.melpa.org
稳定的elpa

melpa.org
ELPA(Emacs Lisp Package Archive)

varnish-cache.org
Varnish cache，或称Varnish，是一套高性能的反向网站缓存服务器（reverse proxy server）





# cli

```sh
sockstat -4 # 显示ipv4的监听端口和本地端口和远程端口连接的情况
```



## portsnap

```sh
portsnap fetch
# 拉取portsnap快照
```

```sh
portsnap extract
# 解压portsnap快照位置在 /usr/ports
```



## gmap, gpart

```sh
show          Show current partition information for the specified geoms,
              or all geoms if none are specified.  The default output
              includes the logical starting block of each partition, the
              partition size in blocks, the partition index number, the
              partition type, and a human readable partition size.  Block
              sizes and locations are based on the device's Sectorsize as
              shown by gpart list.
              # 显示当前分区信息

list          See geom(8). # 显示更加详细的硬盘信息

recover       Recover a corrupt partition's scheme metadata on the geom
              geom.  See the section entitled RECOVERING below for the
              additional information.
              在geom上恢复受损分区
              GEOM： 模块化磁盘变换框架
              通过 GEOM 支持的 RAID 类型
              如何通过 GEOM 使用镜像、 条带、 加密和挂接在远程的磁盘设备。



resize        Resize a partition from geom geom and further identified by
              the -i index option.  If the new size is not specified it
              is automatically calculated to be the maximum available
              from geom.
              调整geom分区大小
              geom类似于linux的lvm
              The resize command accepts these options:
              -a alignment  If specified, then gpart utility tries to
                            align partition size to be a multiple of the
                            alignment value.
              -f flags      Additional operational flags.  See the
                            section entitled OPERATIONAL FLAGS below for
                            a discussion about its use.
              -i index      Specifies the index of the partition to be
                            resized.
              -s size       Specifies the new size of the partition, in
                            logical blocks.  A SI unit suffix is allowed.
```





# file

```sh
# /etc/rc.conf.local
# 此文件为 /etc/rc.conf 复制,因为系统升级时可能会覆盖 /etc/rc.conf 文件

sshd_enable="YES" # 启动sshd

ifconfig_em0="inet 192.168.150.200 netmask 255.255.255.0"
# 设置ip地址和子网掩码,em0为网卡名称

defaultrouter="192.168.150.2" # 设置默认路由
```



```sh
# /etc/portsnap.conf

SERVERNAME=portsnap.FreeBSD.org
# 默认拉取地址
```

