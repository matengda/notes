```python
import os, urllib.request

url = [
    ['https://www.7-zip.org/a/7z1900-extra.7z', '7zip'],
    ['https://dl.google.com/android/repository/platform-tools_r30.0.3-windows.zip', 'adb'],
    ['http://download.qt.io/archive/qt/5.12/5.12.9/qt-opensource-windows-x86-5.12.9.exe', 'qt-lts']
    ['https://mirrors.bfsu.edu.cn/apache/tomcat/tomcat-9/v9.0.36/bin/apache-tomcat-9.0.36-windows-x64.zip', 'tomcat'],
    ['https://download.calibre-ebook.com/4.19.0/calibre-portable-installer-4.19.0.exe', 'calibre'],
    ['https://github.com/MathewSachin/Captura/releases/download/v8.0.0/Captura-Portable.zip', 'captura'],
    ['https://github.com/cmderdev/cmder/releases/download/v1.3.15/cmder_mini.zip', 'cmder'],
    ['https://download.docker.com/win/stable/Docker%20Desktop%20Installer.exe', 'docker'],
    ['https://mirrors.nju.edu.cn/eclipse//technology/epp/downloads/release/2020-06/R/eclipse-java-2020-06-R-win32-x86_64.zip', 'eclipse-java'],
    ['http://ftp.gnu.org/gnu/emacs/windows/emacs-26/emacs-26.3-i686.zip', 'emacs'],
    ['https://www.voidtools.com/Everything-1.4.1.969.x64.zip', 'everything'],
    ['https://justgetflux.com/flux-setup.exe', 'f.lux']
    ['https://ffmpeg.zeranoe.com/builds/win64/static/ffmpeg-20200628-4cfcfb3-win64-static.zip', 'ffmpeg'],
    ['https://download-installer.cdn.mozilla.net/pub/firefox/releases/68.10.0esr/win64/en-US/Firefox%20Setup%2068.10.0esr.exe', 'firefox-esr'],
    ['https://golang.google.cn/dl/go1.12.12.windows-amd64.zip', 'golang'],
    ['https://www.heidisql.com/downloads/releases/HeidiSQL_11.0_64_Portable.zip', 'heidisql'],
    ['http://nginx.org/download/nginx-1.18.0.zip', 'nginx'],
    ['https://ftp.nluug.nl/pub/vim/pc/vim82w32.zip', 'vim'],
    ['https://eternallybored.org/misc/netcat/netcat-win32-1.12.zip', 'nc'],
    ['https://download.jetbrains.8686c.com/fonts/JetBrainsMono-1.0.3.zip', 'jetbrains-mono'],
    ['https://ftp.openbsd.org/pub/OpenBSD/LibreSSL/libressl-2.5.5-windows.zip', 'libressl'],
    ['https://github.com/mstorsjo/llvm-mingw/releases/download/20200325/llvm-mingw-20200325-i686.zip', 'llvm-mingw'],
    ['https://www.nasm.us/pub/nasm/releasebuilds/2.15.05/win64/nasm-2.15.05-win64.zip', 'nasm']
    ['https://jaist.dl.sourceforge.net/project/mingw-w64/mingw-w64/mingw-w64-release/mingw-w64-v7.0.0.zip', 'mingw-w64'],
    ['https://mirrors.nju.edu.cn/tdf/libreoffice/stable/6.3.6/win/x86_64/LibreOffice_6.3.6_Win_x64.msi', 'libreoffice'],
    ['http://download.documentfoundation.org/libreoffice/stable/6.3.6/win/x86_64/LibreOffice_6.3.6_Win_x64_helppack_zh-CN.msi', 'libreoffice-help'],
    ['https://github.com/visualfc/liteide/releases/download/x37.1/liteidex37.1-2.windows-qt5.9.5.zip', 'liteidex'],
    ['https://mirrors.ustc.edu.cn/mysql-ftp/Downloads/MySQL-8.0/mysql-8.0.20-winx64.zip', 'mysql'],
    ['https://github.com/msys2/msys2-installer/releases/download/2020-06-29/msys2-x86_64-20200629.exe', 'msys2'],
    ['https://nmap.org/dist/nmap-7.80-win32.zip', 'nmap'],
    ['https://download.java.net/java/GA/jdk9/9.0.4/binaries/openjdk-9.0.4_windows-x64_bin.tar.gz', 'openjdk'],
    ['https://download.gimp.org/mirror/pub/gimp/v2.10/windows/gimp-2.10.20-setup.exe', 'gimp'],
    ['https://download.gimp.org/mirror/pub/gimp/help/windows/2.10/gimp-help-2.10.0-zh_CN-setup.exe', 'gimp-help'],
    ['http://windows.metasploit.com/metasploitframework-latest.msi', 'metasploit'],
    ['https://nodejs.org/dist/v12.18.2/node-v12.18.2-win-x64.zip', 'nodejs'],
    ['https://github.com/PowerShell/Win32-OpenSSH/releases/download/v8.1.0.0p1-Beta/OpenSSH-Win64.zip', 'openssh'],
    ['https://github.com/PowerShell/PowerShell/releases/download/v7.0.2/PowerShell-7.0.2-win-x64.zip', 'powershell'],
    ['https://download.virtualbox.org/virtualbox/6.1.10/VirtualBox-6.1.10-138449-Win.exe', 'virtualbox'],
    ['https://download.virtualbox.org/virtualbox/6.1.10/Oracle_VM_VirtualBox_Extension_Pack-6.1.10.vbox-extpack', 'virtualbox-extpack'],
    ['https://the.earth.li/~sgtatham/putty/latest/w64/putty.zip', 'putty'],
    ['https://www.winpcap.org/install/bin/WinPcap_4_1_3.exe', 'winpcap'],
    ['https://get.enterprisedb.com/postgresql/postgresql-11.8-1-windows-x64-binaries.zip', 'postgresql'],
    ['https://sqlite.org/2020/sqlite-tools-win32-x86-3320300.zip', 'sqlite3'],
    ['https://github.com/CovenantEyes/sqlcipher-windows/releases/download/v3.0.1/sqlcipher-3.0.1.zip', 'sqlcipher'],
    ['https://sourceforge.net/projects/x64dbg/files/snapshots/snapshot_2020-06-25_21-54.zip/download', 'x64dbg'],
    ['https://github.com/rjpcomputing/luaforwindows/releases/download/v5.1.5-52/LuaForWindows_v5.1.5-52.exe', 'lua'],
    ['https://ftp.postgresql.org/pub/pgadmin/pgadmin4/v4.23/windows/pgadmin4-4.23-x64.exe', 'pgadmin'],
    ['https://www.tightvnc.com/download/1.3.10/tightvnc-1.3.10_x86.zip', 'tightvnc'],
    ['https://windows.php.net/downloads/releases/php-7.4.7-nts-Win32-vc15-x64.zip', 'php'],
    ['https://www.python.org/ftp/python/3.6.8/python-3.6.8-amd64.exe', 'python'],
    ['https://qemu.weilnetz.de/w64/2020/qemu-w64-setup-20200612.exe', 'qemu'],
    ['https://github.com/oneclick/rubyinstaller2/releases/download/RubyInstaller-2.7.1-1/rubyinstaller-2.7.1-1-x64.7z', 'ruby'],
    ['https://rakudo.org/dl/rakudo/rakudo-moar-2020.06-01-win-x86_64.zip', 'rakudo'],
    ['http://strawberryperl.com/download/5.30.2.1/strawberry-perl-5.30.2.1-64bit.zip', 'perl'],
    ['https://github.com/pbatard/rufus/releases/download/v3.11/rufus-3.11p.exe', 'rufus'],
    ['https://static.rust-lang.org/dist/rust-1.44.1-i686-pc-windows-gnu.msi', 'rust'],
    ['https://typora.io/windows/typora-setup-x64.exe', 'typora'],
    ['https://download-installer.cdn.mozilla.net/pub/thunderbird/releases/68.9.0/win32/en-US/Thunderbird%20Setup%2068.9.0.exe', 'thunderbird'],
    ['https://github.com/microsoft/terminal/releases/download/v1.0.1401.0/Microsoft.WindowsTerminal_1.0.1401.0_8wekyb3d8bbwe.msixbundle', 'windows-terminal'],
    ['https://github.com/microsoft/winget-cli/releases/download/v0.1.4331-preview/Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.appxbundle', 'winget'],
    ['https://github.com/mpc-hc/mpc-hc/releases/download/1.7.13/MPC-HC.1.7.13.x64.zip', 'mpc-hc'],
    ['https://launchpad.net/veracrypt/trunk/1.24-update6/+download/VeraCrypt%20Portable%201.24-Update6.exe', 'veracrypt'],
    ['https://dl.bintray.com/rime/weasel/weasel-0.14.3.0-installer.exe', 'rime'],
    ['https://1.na.dl.wireshark.org/win32/WiresharkPortable_3.2.4.paf.exe', 'wireshark'],
    ['https://jaist.dl.sourceforge.net/project/sbcl/sbcl/2.0.0/sbcl-2.0.0-x86-64-windows-binary.msi', 'sbcl'],
    ['https://github.com/commercialhaskell/stack/releases/download/v2.3.1/stack-2.3.1-windows-x86_64.zip', 'stack'],
    ['https://www.fosshub.com/Audacity.html/audacity-win-2.4.2.zip', 'audacity'],
    ['https://github.com/git-for-windows/git/releases/download/v2.27.0.windows.1/PortableGit-2.27.0-64-bit.7z.exe', 'git'],
    ['https://mirrors.nju.edu.cn/CTAN/systems/texlive/Images/texlive2020-20200406.iso', 'texlive'],
    ['https://github.com/texstudio-org/texstudio/releases/download/2.12.22/texstudio-2.12.22-win-qt5.exe', 'texstudio'],
    ['https://community.download.adacore.com/v1/966801764ae160828c97d2c33000e9feb08d4cce?filename=gnat-2020-20200429-x86_64-windows-bin.exe', 'gnat'],
    ['https://cdn-fastly.obsproject.com/downloads/OBS-Studio-25.0.8-Full-Installer-x64.exe', 'obs'],
    ['https://curl.haxx.se/windows/dl-7.71.1/curl-7.71.1-win64-mingw.zip', 'curl']
    ] # 这些是开源软件

if not os.path.exists('./wint'):
    os.mkdir('./wint', 0o755)

for i in url:
    urllib.request.urlretrieve(i[0], './' + os.path.basename(i[0]))
```





# 生产力工具

|      |      |
| ---- | ---- |
|      |      |



# 快捷键

| win-e   | 文件资源管理器 |
| ------- | -------------- |
| C-z     | 撤销           |
| CS-z    | 反撤销         |
| win-d   | 新建一个桌面   |
| win-tab | 切换桌面       |
|         |                |
|         |                |
|         |                |
|         |                |
|         |                |



# powershell

```powershell
# C:\Users\sa\Documents\PowerShell\Microsoft.PowerShell_profile.ps1

# 引入 posh-git
Import-Module posh-git

# 引入 oh-my-posh
Import-Module oh-my-posh

# 设置 PowerShell 主题
Set-Theme Paradox

# 设置 Ctrl+d 为菜单补全和 Intellisense
Set-PSReadLineKeyHandler -Key "Tab" -Function MenuComplete

# 设置C-p为后向搜索历史记录
Set-PSReadLineKeyHandler -Key "Ctrl+p" -Function HistorySearchBackward

# 设置C-n为前向搜索历史纪录
Set-PSReadLineKeyHandler -Key "Ctrl+n" -Function HistorySearchForward
```



```powershell
# 1. 安装 PSReadline 包，该插件可以让命令行很好用，类似 zsh
Install-Module -Name PSReadLine -AllowPrerelease -Force

# 2. 安装 posh-git 包，让你的 git 更好用
Install-Module posh-git -Scope CurrentUser

# 3. 安装 oh-my-posh 包，让你的命令行更酷炫、优雅
Install-Module oh-my-posh -Scope CurrentUser
```



```powershell
Start-Process PowerShell -Verb runas
rem 以管理员权限运行 powershell
```

```powershell
$VariableName = Value # 定义一个变量
# PowerShell中的变量只存在PowerShell内部会话中, 一旦PowerShell关闭, 定义的变量将失效
```

```powershell
Get-ChildItem env: # 获取全部变量
```

```powershell
$env:VariableName # 获取指定变量
```

```powershell
setx /M path "$env:path;C:\example"
<#
把指定目录添加到$env:path (设置环境变量)
变量用";"分隔路径
变量名不要包含"-"否则PowerShell会以为是减号
#>
```



set和setx命令的区别
set 用于设置临时环境变量,批处理脚本中经常用到
setx 用于设置用户环境变量和系统环境变量
setx 设置用户环境变量,默认记录在 HKEY_CURRENT_USER
setx /M 设置系统环境中的变量,记录在 HKEY_LOCAL_MACHINE





```powershell
code $Profile # 编辑pwsh配置文件
```

```powershell
Get-Service '' # 获取全部服务
```

```powershell
Stop-Service '' # 停止一个服务
```

```powershell
Start-Service '' # 启动一个服务
```

```powershell
mysqld --remove # 卸载mariadb服务
httpd -k instal # 安装httpd服务
mysqld --instal # 安装mariadb服务
```

```powershell
echo %VariableName%
<#
在cmd中显示变量值
在cmd中注释是 ::
echo %VariableName% :: 这时候::就不管用了,不知道原因
#>
```

```powershell
$psversiontable # 查看当前版本、Windows版本、.NET版本信息
```



```powershell
whoami # 显示域和用户名
```

```powershell
net user userName # 查看用户信息
```

```powershell
net localgroup groupName # 查看组内成员
```

```powershell
net localgroup groupName /add # 添加一个组
```

```powershell
net user userName password /add # 添加一个用户
```

```powershell
net localgroup groupName userName /add # 将用户添加到组里
```

```powershell
net user userName password # 修改用户密码
```

```powershell
wmic os get caption # 获取系统版本
```

```powershell
chkdsk H:/F
<#
修复磁盘和tf卡错误
H:就是你的SD卡盘符，/F 是修复参数
#>
```





## diskpart

```powershell
list partition # 列出所选的磁盘分区
```

```powershell
select disk diskNumber # 选择磁盘,磁盘编号从0开始
```

```powershell
help delete partition # 显示有关帮助文档
```

```powershell
select partition 1 # 选择磁盘分区,这条命令的前提是已经选择了硬盘
```

```powershell
delete partition # 删除分区
```

```powershell
delete partition override
# 删除任何分区,无论该分区属于何种类型。通常，DiskPart 只允许删除已知的数据分区
```

```powershell
convert gpt # 将磁盘从mbr转换为gpt
```

```powershell
detail disk # 显示选中的磁盘属性
```

```powershell
clean # 从带焦点的磁盘删除任意和所有分区或格式化卷。
```

```powershell
clean all #  指定将磁盘上的每个字节\扇区都设置为 0
<#
    这会完全删除该磁盘上包含的所有数据
    在主启动记录(MBR)磁盘上，仅会覆盖 MBR 分区信息和隐藏的
    扇区信息。在 GUID 分区表(GPT)磁盘上，包括
    保护性 MBR 在内的 GPT 分区信息都会被覆盖。如果不使用 ALL 参数，
    则磁盘的第一个 1MB 和最后一个 1MB 都将为零。这将擦除以前应用到
    该磁盘的任何格式化磁盘。清除该磁盘后的磁盘状态为 'UNINITIALIZED'
#>
```

```powershell
exit # 退出diskpart
```









# msc

什么是mmc
微软管理控制台（英语：Microsoft Management Console，简称MMC）是从Windows 2000开始提供的一个组件，它为系统管理员和高级用户提供一个配置和监控系统的界面



```powershell
diskmgmt.msc REM 磁盘管理
REM 是注释符
```







使用utf-8为默认编码
控制面板->时钟和区域->区域->管理-更改系统区域设置



启动电脑按dell键进入bios
点击F7键或Advanced进入高级选项
cpu configuration->svm mode->enabled->F10保存并退出
以前找virtual,现在找svm,因为amd的虚拟化技术叫svm



















https://www.faststone.org/FSCaptureDetail.htm
截图与录屏软件

https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise
windows 10 enterprise download
此版本是测试版本,尝试各种方法均无法激活

https://www.netsarang.com/
xshell和xftp

https://dl.google.com/android/repository/platform-tools-latest-windows.zip
adb

www.ezbsystems.com/ultraiso/index.html
软碟通(创建、修改和转换ISO文件)

https://www.google.cn/chrome/index.html
chrome浏览器

www.teamviewer.com
TeamViewer是一个远程控制软件

get2.adobe.com/cn/flashplayer
adobe flash player

www.7-zip.org
7z解压缩

code.visualstudio.com
Visual Studio Code（VS Code）是一个由微软开发的，同时支持Windows、Linux和OS X操作系统的开源文本编辑器。它支持调试（调试功能仅限于 ASP.NET 和 Node.js 项目），内置了Git 版本控制功能，同时也具有开发环境功能，例如代码补全（类似于 IntelliSense）、代码片段等。该编辑器支持用户自定义配置，例如改变主题颜色、键盘快捷方式、编辑器属性和其他参数。

www.foobar2000.org
音频播放器软件

www.voidtools.com/downloads
everything搜索软件

msdn.itellyou.cn
ed2k 微软资源

https://www.jetbrains.com/idea/
idea文本编辑器

justgetflux.com
f.lux(自动调节色温,保护眼睛)

https://ethanschoonover.com/solarized/
护眼主题

www.aida64.com
查看硬件信息

www.cpuid.com/softwares/cpu-z.html
cpu-z

www.techpowerup.com/gpuz
gpu-z

filezilla-project.org
FileZilla是FTP软件，分为客户端版本和服务器版本

mpc-hc.org
Media Player Classic Home Cinema，简称MPC-HC，是一款简洁的媒体播放器，Media Player Classic 的后续版本

cheatengine.org
查找与修改内存地址

www.hex-rays.com/products/ida/support/download.shtml
ida(反组译与除错工具的产品，常用于逆向工程)

www.vmware.com/cn/products/workstation.html
VMware Workstation允许一台真实的电脑在一个操作系统中同时打开并运行数个操作系统

ollydbg.de
动态反汇编分析调试工具

x64dbg.com
调试工具

rime.im
中州韵输入法引擎

www.cgsoftlabs.ro/studpe.html
pe综合工具

www.mwsl.org.cn
挂马、色情、赌博、低俗广告等多种类型的hosts

www.piriform.com/recuva
快速，轻松地恢复已删除的文件

www.snipaste.com
Snipaste 是一个简单的截图工具

www.diskgenius.cn
DiskGenius，原名DiskMan，是一款免费的硬盘修复、分区工具软件

www.visualstudio.com
Microsoft Visual Studio（简称VS）是微软公司的开发工具包系列产品。VS是一个基本完整的开发工具集，它包括了整个软件生命周期中所需要的大部分工具

www.ghisler.com
Total Commander 是一款应用于 Windows 平台的文件管理器

autohotkey.com
AutoHotkey是面向普通电脑用户的自由开源的自动化软件工具，它让用户能够快捷或自动执行重复性任务

www.mozilla.org/zh-CN/thunderbird
电子邮箱客户端

java.com/en/download/manual.jsp
所有jre(Java Runtime Environment)下载

winpcap.org
为应用程序提供访问网络底层的能力,它用于windows系统下的直接的网络编程

desowin.org/usbpcap
USB数据包检测,捕获工具

nasm.us
汇编编译器

heidisql.com
SQL管理工具

mingw.org
MinGW（Minimalist GNU for Windows），又称mingw32，是将GCC编译器和GNU Binutils移植到Win32平台下的
产物，包括一系列头文件（Win32API）、库和可执行文件。
mingw-w64.org
用于产生32位及64位Windows可执行文件的MinGW-w64项目，是从原本MinGW产生的分支。如今已经独立发展

privoxy.org
Privoxy是一款带过滤功能的代理服务器，针对HTTP、HTTPS协议。通过Privoxy的过滤功能，用户可以保护隐私、对网页内容进行过滤、管理cookies，以及拦阻各种广告等。Privoxy可以用作单机，也可以应用到多用户的网络

github.com/OpenVPN/openvpn-gui
OpenVPN GUI是在Windows XP / Vista / 7/8上运行的OpenVPN的图形前端

github.com/Trinea/android-open-project
Android 开源项目分类汇总

hushmail.com
一般安全电子邮箱

protonmail.com
ProtonMail（在中国被非正式地称为“质子邮箱”）是一个加密的webmail服务，于2013年由欧洲核子研究组织（CERN）成员Jason Stockman、Andy Yen和Wei Sun创建。ProtonMail现由总部设在瑞士日内瓦州的Proton Technologies AG经营。据Andy Yen称，他和公司一半的人都来自麻省理工学院（MIT），ProtonMail的两个服务器分别设在瑞士的Lausanne和Attinghausen

apachefriends.org
XAMPP是一个把Apache网页服务器与PHP、Perl及MariaDB集合在一起的安装包，允许用户可以在自己的电脑上轻易的建立网页服务器

www.piriform.com/CCLEANER
保护隐私,清理不必要的垃圾,找出及修正Windows登录中的问题包括不再使用的扩展名及程序路径的问题等

ipmsg.org/tools/fastcopy.html
FastCopy 是一款快速的文件拷贝计算机软件

www.microsoft.com/zh-cn/software-download/windows10
win10 官方下载链接

sourceforge.net/projects/win32diskimager
将文件写入到磁盘
neosmart.net/EasyBCD
启动菜单管理
www.grc.com/securable.htm
检查是否支持cpu虚拟化

www.cockos.com/licecap
LICEcap 是一款用于屏幕录像制作的开源软件

www.dosbox.com
DOSBox是一种模拟器软件，主要是在IBM PC兼容机下，模拟旧时的操作系统：MS-DOS