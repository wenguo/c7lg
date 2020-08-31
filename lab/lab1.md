# 实验指导 1 - CentOS系统安装配置与基本操作

>#### 学习目标
> * 安装和使用 VirtualBox
> * 最小化安装 CentOS 7
> * 学会本地登录和远程登录
> * 学会使用命令帮助
> * 学会获取系统基本信息
> * 学会关闭和重启系统
> * 熟悉 Linux 文件系统的层次结构
> * 熟悉 bash 的 通配符、命令补全、命令历史、命令别名 的使用
> * 掌握 `ls` , `pwd` , `cd` , `mkdir` , `rmdir` , `tree` 等目录操作命令
> * 掌握 `touch` , `cp` , `mv` , `rm` , `ln` , `file` 等文件操作命令
> * 掌握 `cat` , `more` , `less` , `head` , `tail` 等文本文件显示命令
> * 掌握 bash 的 变量定义和引用 `${}`、变量作用域
> * 掌握 bash 的 命令替换 `$()` 的使用
> * 掌握 bash 的 整数运算 `$[]` 和 `$(())` 的使用
> * 掌握 bash 的 重定向和管道 的使用
> * 掌握 bash 的 环境变量和环境配置文件 的使用
> * 熟悉 正则表达式 的使用
> * 进一步熟悉 管道 的使用
> * 掌握整行横向截取：`head` 和 `tail` 命令
> * 掌握按关键字横向截取：`grep` 命令
> * 掌握按列纵向截取：`cut` 命令
> * 掌握纵向和横向合并：`cat` 和 `paste` 命令
> * 掌握文本统计：`wc` 命令
> * 掌握文本比较：`diff` 命令
> * 掌握文本排序：`sort` 命令
> * 掌握随机排列：`sort -R` 和 `shuf` 命令
> * 掌握剔除重复行： `sort -u` 和 `uniq` 命令
> * 掌握文本替换和删除：`tr` 和 `sed` 命令
> * 熟悉文本格式化：`fmt`、`pr`、`column` 和 `fold` 命令
> * 熟悉文本编码转换：`iconv`、`enca`、`convmv` 命令
> * 掌握 `vim` 文件编辑器的使用
> * 掌握 `sed`、`awk` 工具的使用

## 任务1：下载并安装 VirtualBox

1. 从 <https://www.virtualbox.org/wiki/Downloads> 下载
  * VirtualBox platform packages
  * Oracle VM VirtualBox Extension Pack
2. 双击下载文件安装

## 任务2：下载并校验 CentOS 7 ISO

1. 从 <http://mirrors.163.com/centos/7/isos/x86_64/> 下载
  * CentOS-7-x86_64-Minimal-1708.iso
  * sha256sum.txt
2. 从 <https://git-scm.com/download/win> 下载 Git for Win 并双击安装
3. 校验 CentOS 7 ISO
  1. 在文件管理器中进入 ISO文件和 sha256sum.txt 所在目录
  2. 鼠标右键点击空白处，在菜单中运行 “Git Bash Here”
  3. 执行命令进行校验
          $ grep 'Minimal' sha256sum.txt | sha256sum -c
	      或
          $ sha256sum -c sha256sum.txt 2>&1 | grep OK

## 任务3：创建并运行 VirtualBox 虚拟机

1. 运行 VirtualBox，单击 “新建” 按钮
  1. 虚拟电脑名称和类型
    * 名称：cent7h1
    * 类型：Linux
    * 版本：Red Hat （64-bit）
  2. 内存大小 1024M
  3. 虚拟硬盘 
    * 虚拟硬盘文件类型：VMDK
    * 分配类型：动态分配
    * 大小：20G
2. 单击 “设置” 按钮
  1. 存储：为虚拟光驱分配 ISO 文件
  2. 网络：
    * **网卡1：NAT** 
    * **网卡2：Host-only**
3. 单击 “启动” 按钮运行虚拟机

## 任务4：最小化安装 CentOS 7

1. 进入安装引导界面，选择 “Install CentOS 7” 并回车
2. 选择安装过程使用的语言 “中文”
3. 安装配置
   1. 配置键盘布局  —— 英语（美国） 
   2. 安装设备并分区 —— 自动配置分区（使用 LVM 方式）
     * 修改自动分区布局，将 / 文件系统所在的逻辑卷调整为 8GiB
   3. 配置网络和主机名
      1. 对于被 VirtualBox 设置了 NAT 类型的网卡使（由 VirtualBox 自动分配网络参数），并设置开机启用此网卡
      2. 对于被 VirtualBox 设置了 Host-only 类型的网卡设置静态网络参数 192.168.56.71/24，并设置开机启用此网卡
4. 用户设置
   - 设置root用户口令
   - 创建名为 student 的的普通用户、设置其口令，并将其加入 wheel 组
5. 安装完毕，重新启动

## 任务5：本地登录和远程登录

* 在虚拟机中本地登录
  * 以 root 用户登录
  * 用 Alt+F2 切换第二个虚拟控制台，以 student 用户登录 
* 从宿主机远程登录 Linux
  * 使用 Git Bash
         $ ssh root@192.168.56.71
         

         # logout                          # 或 exit 或 Ctrl+d
         
         $ ssh student@192.168.56.71
         
         $ logout                          # 或 exit 或 Ctrl+d
  * 使用 Putty
    * 从 <http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html> 下载 Putty 并安装
    * 创建两个会话，分别用于 root 和 student 登录 192.168.56.71

> **参考**
>
> * <http://www.runoob.com/linux/linux-remote-login.html>
> * [《完全用 GNU/Linux 工作》 之 20. 增進 SSH 使用效率 - ssh_config](https://chusiang.gitbooks.io/working-on-gnu-linux/content/20.ssh_config.html)
> * [storm](https://github.com/emre/storm) is a command line tool to manage your ssh connections.

## 任务6：获得命令帮助

* 显示所有可用的 Bash 内置命令
      $ help
* 显示指定的 Bash 内置命令的帮助
      $ help pwd
      $ help type
      $ help exit	
* 使用 `--help` 命令参数获取命令帮助
      $ ls
      $ ls --help
      $ ls /
      $ ls -l /
      $ ll /
      $ which ll                             # ll 是命令别名
      $ ls -F /etc 
      $ ls -a
	
	  $ which clear
	  $ clear                                # 清屏，快捷键为 <Ctrl+l>
	
	  $ basename --help
	  $ basename /usr/bin/sort
      $ dirname --help
	  $ dirname /usr/bin/sort

	  $ date
	  $ date --help
	  $ date +%F
	  $ date +%Y/%m/%d
	  $ date +"%F %T"
	  $ date +%s

      # 显示UNIX纪元时间为 1476436877 的当前时区时间
	  $ date --date='@1476436877'             
	  $ date -d '@1476436877'             
	  $ date --date='@1476436877' +"%F %T"
	  $ date -d '@1476436877' +"%F %T"
	  $ date -d '+45 days' +%F                # 显示 45 天后的日期
	  $ date -d '-45 days' +%w                # 显示 45 天前是周几
      # 显示下周五洛杉矶早九点 的当前时区时间
	  $ date -d 'TZ="America/Los_Angeles" 09:00 next Fri'  
	
	  $ cal
	  $ cal --help
	  $ cal -3
	  $ cal 2016
	  $ cal 9 2016
	  $ cal 9 1752                    # 新历换旧历，有11天被去除
* 使用 man 手册获得命令帮助（1）

      $ man ls
	  $ man date
	  $ man cal
	  $ man which
	  $ man basename
	  $ man dirname
      $ man bash

	  $ manpath         # 显示 man 命令在何处查找手册
	  $ man man         # 显示 man 命令的手册
      
      # 从 man 的索引数据库中查找有关 passwd 的手册（精确匹配）
	  $ whatis passwd   
	  $ man passwd
	  $ man 5 passwd
	
      # 从 man 的索引数据库中查找有关 standards 的手册（模糊匹配）
	  $ apropos standards   
	  $ man 7 standards
	
	  # 比较 whatis 和 apropos
	  $ whatis ls
	  $ apropos ls  或 man -k ls

	  $ man intro
      $ man 4 console
      $ man 4 pts

>**man手册的操作**（与 `less` 命令的操作键兼容）
>
>* 下箭头：向下滚动一行；上箭头：向上滚动一行
>* 空格键 或 PgDn：向下滚动一屏；PgUp：向上滚动一屏
>* /string ： 向下搜索字符串 string
>* n：继续向下搜索；N：继续向上搜索
>* g：跳转到手册首行；G：跳转到手册末行
>* q：退出手册，返回Shell提示符

## 任务7：使用 `su -` 切切换用户身份

* 普通用户切换为 root 用户
	  [student@localhost ~]$ su -
	  Password:           # 输入 root 用户口令
	  [root@localhost ~]# 
	
	  ## 执行只有 root 用户才能执行的管理命令
      # 安装最小化安装未安装的 man-pages
	  [root@localhost ~]# yum -y install man-pages       
	  [root@localhost ~]# mandb --help
      # 创建 man 索引数据库文件
	  [root@localhost ~]# mandb -c
	
	  [root@localhost ~]# exit
	  [student@localhost ~]$ 
* root 用户切换为普通用户
	  [root@localhost ~]# su - student
	  [student@localhost ~]$
	
	  [student@localhost ~]$ exit
	  [root@localhost ~]# 

## 任务8：安装必要的软件并更新系统

* 安装必要的软件
	  # yum -y install  lshw pciutils usbutils 
	  # yum -y install  gdisk system-storage-manager
	  # yum -y install  bash-completion 
	  # yum -y install  zip unzip bzip2 tree tmpwatch pinfo man-pages
	  # yum -y install  nano vim-enhanced tmux screen
	  # yum -y install  net-tools psmisc lsof sysstat
	  # yum -y install  yum-plugin-security yum-utils createrepo
	  # yum -y install  git wget curl elinks lynx lftp mailx mutt rsync bind-utils
* 更新系统
  * 检查是否有可用的软件包更新
        # yum check-update 或 yum list updates
  * 执行更新
        # yum -y update

> **提示**：
>
> * 更新系统时会使用 CentOS 镜像站点中的 update 仓库，因此需要主机连接公网
> * 在安装 Linux 虚拟机时配置了一块自动获得IP的NAT类型的网卡，因此只要安装此虚拟机的宿主机能连接公网则 Linux 虚拟机就能连接公网

## 任务9： 重启系统

* 执行如下命令之一重启系统
      # systemctl reboot
      # reboot
      # shutdown -r now

## 任务10：  获取系统基本信息

* 显示硬件信息
      # lshw                    # 显示所有硬件信息
      # lshw -c processor       # 显示 CPU 的制造厂商及型号
      # lshw -c memory          # 显示物理内存的大小
      # lshw -c disk            # 显示磁盘的实际尺寸
      # lshw -c display         # 显示显卡的制造厂商及型号
      # lshw -c network         # 显示网卡的制造厂商及型号
      
	  # lscpu
	  # lspci
	  # lsusb
* 显示系统信息
      # cat /etc/system-release    # 查看系统发行版本
      # uname -r                   # 查看系统内核版本
      # dmesg                      # 查看系统启动信息
      
      # free -h                    # 查看物理内存和交换空间大小
      # swapon -s                  # 查看所有交换空间
      
      # lsblk                      # 查看系统中的块设备
      # df -Ph                     # 查看磁盘剩余空间
      
      # localectl                  # 查看语言支持与键盘设置
      # timedatectl                # 查看日期和时间（或 date）
      
      # hostnamectl                # 显示主机名（或 hostname）
      # ip addr show               # 显示网络接口参数（或 ifconfig）
      # ip route show              # 显示路由信息（或 route）

## 任务11：  系统基本设置

* 设置语言支持
      # 查看系统支持的语言
	  # localectl list-locales
  
      # 更改为英文，下次登录时生效
      # localectl set-locale LANG="en_US.UTF-8"
	  # 更改为中文，下次登录时生效
	  # localectl set-locale LANG="zh_CN.UTF-8"
* 设置系统时区
      # 查看系统支持的时区
	  # timedatectl list-timezones
	
	  # 更改时区为欧洲巴黎，立即生效
	  # timedatectl set-timezone Europe/Paris
	  # 更改时区为中国上海，立即生效
      # timedatectl set-timezone Asia/Shanghai	
* 设置日期 和/或 时间
      # timedatectl set-time 23:06:00
      # timedatectl set-time 2016-10-15
      # timedatectl set-time '2016-10-15 23:06:00'
        
      ## CentOS6 及早期版本中使用的命令，CentOS7 仍兼容
      # date -s '20161015'
      # date -s '23:06'
      # date -s '20161015 2306'
      
      # date 101523052016.00     # [MMDDhhmm[[CC]YY][.ss]]
* 设置主机名
      # hostnamectl set-hostname cent7h1.olabs.lan
* 配置SELinux
      # 将配置文件 /etc/selinux/config 中的 `SELINUX=enforcing` 行改为 `SELINUX=disabled`
      # sed -i 's/SELINUX=.*/SELINUX=disabled/' /etc/selinux/config

## 任务12： 关闭系统

* 执行如下命令之一关闭系统
      # systemctl poweroff
      # poweroff
      # shutdown -h now


----

> **提示**： 请以普通用户student登录完成下面的操作

## 任务13： 熟悉 Linux 文件系统层次结构

```
$ ls -l /
$ tree -L 1 /
/                                 
├── bin -> usr/bin            # CentOS7中，/bin 链接至 /usr/bin 
├── boot          # 包含系统启动过程所需的文件       
├── dev               # 是一个伪文件系统，只存在内存中。包含设备文件，供用户访问硬件设备
├── etc           # 包含系统配置文件
├── home          # 普通用户主目录的父目录，如普通用户 student 的主目录为 /home/student
├── lib -> usr/lib            # CentOS7中，/lib 链接至 /usr/lib
├── lib64 -> usr/lib64        # CentOS7中，/lib64 链接至 /usr/lib64
├── media         # 包含可移动介质，如CD/DVD光盘 或 USB 的挂载点
├── mnt           # 是临时挂装文件系统的挂载点
├── opt           # 存放第三方应用程序
├── proc              # 是一个伪文件系统，只存在内存中。它以文件系统的方式为用户和程序访问系统内核数据的操作提供接口
├── root          # 超级用户 root 的主目录
├── run               # 是一个伪文件系统，只存在内存中。CentOS7中，整合了旧版本的 /var/run 和 /var/lock
├── sbin -> usr/sbin          # CentOS7中，/sbin 链接至 /usr/sbin
├── srv           # 存放各种服务的数据资料
├── sys               # 是一个伪文件系统，只存在内存中。与 /proc 类似，同时失实现了 Linux 统一设备模型
├── tmp           # 存放临时文件目录，10天内未访问、更改和修改的文件会被删除
├── usr           # 包含系统安装的程序、工具、库文件等，bin子目录下的所有用户皆可执行；sbin 下的仅 root 能执行
└── var           # 包含动态变化的数据和文件，如数据库、缓存数据、日志文件、网站内容等
```


## 任务14： 目录操作命令

```
pwd
cd /
ls -l 
ls -l /etc /var
ls -lR /etc
tree /etc
ls -ld /etc 
cd
mkdir abc bcd cde
mkdir -p def/gh/ij   # mkdir -p 用于创建目录树
tree
rmdir abc
rmdir def            # rmdir 只能删除空目录
rm -d bcd            # rm -d 用于删除空目录
ls
rm -ri cde           # rm -r 可以递归地删除目录中的所有内容，包括其中的子目录、子目录的子目录……
rm -rf def           # rm -r 可以递归地删除目录中的所有内容，包括其中的子目录、子目录的子目录……
ls
```

>**强制执行与交互确认执行**：
>
>* `-f` 参数表示强制执行
>* `-i` 参数表示以交互的确认方式执行
----
>**防止误删除**
>
>* https://linuxtoy.org/archives/rm-protection.html
>* https://github.com/alanzchen/rm-protection#comparison-with-alternative-methods

## 任务15： 绝对路径与相对路径

```
ls /etc/passwd                # 使用绝对路径
cd /etc                       # 使用绝对路径
pwd                           # 显示当前路径
ls passwd                     # 使用相对路径
ls /usr/share/man             # 使用绝对路径

^man^doc                      # 将上一条命令中的 man 替换为 doc
cd <Esc><.>                   # 按下<ESC>键再按下<.>键 用于复制上一条命令的最后一个参数 （也可同时按下 <Alt+.>）

pwd
ls zip<TAB>                   # 使用相对路径
ls ../man/man1                # 使用相对路径
ls ../../src                  # 使用相对路径
cd ../..                      # 使用相对路径
pwd
cd -                          # 进入前次进入的目录
pwd
cd ~                          # 使用绝对路径进入用户主目录，等同于不带任何参数的 cd 命令
                              # 若登录用户为 student，则 ~ 会扩展为 /home/student
```

>**提示**：绝对路径与相对路径的概念适用于所有需要操作目录/文件的命令


## 任务16： 命令补全

```
tz<TAB>                       # 当前以tz开头的命令只有tzselect一个，所以自动补全为 tzselect
pas<TAB><TAB>                 # 两次按下<TAB>键可以列出所有以pas开头的命令
passwd -<TAB><TAB>            # 两次按下<TAB>键可以列出所有当前命令可用的参数
ls /etc/pa<TAB><TAB>          # 两次按下<TAB>键可以列出/etc目录下所有以pa开头的文件或目录
echo $P<TAB><TAB>             # 两次按下<TAB>键可以列出以P开头的所有变量
echo $PA<TAB>                 # 当前以PA开头的变量只有PATH一个，所以自动补全为 PATH
```

> **提示**：<TAB> 可以补全命令、路径和/或文件名、命令参数、变量（以$开头）、用户名（以~开头）


>**命令行编辑**
> 
>* Ctrl-b —— 左移一个字符（左方向键）
>* Ctrl-f —— 右移一个字符（右方向键）
>* Esc-b  —— 左移一个单词 
>* Esc-f  —— 右移一个单词
>* Ctrl-a —— 移动到行首
>* Ctrl-e —— 移动到行尾
>
>* Ctrl-u —— 删除到行首
>* Ctrl-k —— 删除到行尾

## 任务17：命令历史

```
history       # 显示命令历史表
!!            # 执行最近一次执行过的命令，等效于 <Up>键
sudo !!       # 使用 sudo 执行最近一次执行过的命令

!15           # 执行命令历史表中的标号为15的命令
!-15		  # 执行15个命令之前的那个命令

!ls           # 执行最近一次执行过的以 ls 开头的命令
!?abc		  # 执行最近一次执行过的包含abc的命令
```

>**提示**：可以使用 Up、Down 方向键遍历命令历史表

## 任务18：复制、移动、删除文件

```
cp /etc/passwd .        # 将 /etc/passwd 文件复制到当前目录
mkdir myfiles           # 创建空目录 myfiles
cp passwd myfiles       # 将当前目录下的 passwd 文件复制到 myfiles 目录下
mv passwd mypasswd      # 将当前目录下的 passwd 文件改名为 mypasswd
mv mypasswd myfiles     # 将当前目录下的 mypasswd 文件移动到 myfiles 目录下
rm myfiles/passwd       # 删除 myfiles 目录下 的 passwd 文件
mv myfiles files        # 将目录 myfiles 改名为 files
rm files/mypasswd       # 删除目录 files 下的 mypasswd 文件

cp /etc/issue /usr/share/doc/mutt<TAB>/samples/iconv/iconv.glibc-2.1.3.rc \# 续行符 \ 表示下一行是当前行的延续
   files                                                                   # 一次性将多个文件复制到指定目录
mkdir doc
cd files
mv issue iconv.glibc-2.1.3.rc ../doc                      # 一次性将多个文件移动到指定目录
cd
rm doc/issue doc/iconv.glibc-2.1.3.rc                     # 一次性删除多个文件

## 使用 cp 命令复制目录树
#   -R, -r, --recursive     递归地复制目录
#   -a, --archive           等价于 -dR --preserve=all
#   -d                      等价于 -P --preserve=links，即不跟随的符号链接且保留硬链接
#   -P, --no-dereference    不跟随源文件的符号链接
#   -p                      等价于 --preserve=mode,ownership,timestamps
#   --preserve[=ATTR_LIST]  保留文件或目录的属性（默认为:  mode[权限],ownership[属主],timestamps[时间戳]）, 
#                           还可以指定文件或目录的其他属性: context[安全上下文], links[链接], xattr[扩展属性]
#                           all 表示尽可能地保留文件或目录的所有属性

cp -r /etc .                # 将 /etc 整个目录树（-r表示递归）复制到当前目录（仅复制有权读取的文件）
ll etc                      # 已复制的文件时间改为复制时的时间；文件的属主和组改为执行cp命令的用户
cp -a /etc .                # 将 /etc 整个目录树（-a表示归档）复制到当前目录（仅复制有权读取的文件）
ll etc                      # 已复制的文件时间保留源文件的时间、权限；文件的属主和组改为执行cp命令的用户
rm -rf etc                  # 强制删除当前目录下的 etc 目录
```

## 任务19：命令别名

```
l.
ll
which ll
alias                    # 显示当前可用的所有别名
ls
\ls                      # 使用 ls 命令而非 ls 别名
alias ren=mv             # 设置别名 ren
cd /etc
cd..
alias cd..='cd ..'       # 设置别名 cd.. ，等号右侧若包含空格应使用引号括起
cd..
cd
unalias cd..             # 取消别名 cd..
cd..
```

>**参考**：
>
> https://www.linuxtrainingacademy.com/23-handy-bash-shell-aliases-for-unix-linux-and-mac-os-x/

## 任务20： bash 变量定义、引用与显示

```
u=Unix           # 变量赋值
l=Linux          # l是字母不是数字，$1 有特殊含义且不能直接赋值
disto=CentOS
ver=7

ul="$u and $l"   # 等号右侧若包含空格需用引号括起来，双引号会扩展出变量的值
lu='$l and $u'   # 等号右侧若包含空格需用引号括起来，单引号不会扩展出变量的值
echo ${ul} ${lu}
echo $ul $lu

echo $l is a type of $u.
echo $disto$ver a type of $l.

help echo        # 显示 shell 内置命令 echo 的帮助信息

echo -n $l is a type of $u,
echo $disto$ver a type of $l.

echo "$disto$ver a type of $l."     # 双引号内的变量引用会扩展出变量的值
echo -e "$l is a type of $u, \n $disto$ver a type of $l."
echo -e "$ul \t\t $lu"

## 防止变量被扩展
echo '$disto$ver a type of $l.'     # 单引号内的变量引用不会扩展出变量的值
echo \$disto$ver a type of $l.      # \$ 还原 $ 字符本身含义，不会扩展出变量的值
echo "$disto\$ver a type of $l."    # \$ 还原 $ 字符本身含义，不会扩展出变量的值

l=disto                             # 变量可以再次赋值
echo ${!l}                          # 变量的间接引用

echo "I'm a student." 
echo Je t\'aime.

ilu="Je t'aime."
echo $ilu

echo $RANDOM  $PATH                 # 显示环境变量的值
env                                 # 显示所有环境变量
```

## 任务21： 特殊文件名

```
cd
mkdir My Document
ll
rmdir My Document
mkdir 'My Document'        # 命令操作对象包含空格，可使用单引号括起来
ll
rmdir 'My Document'
mkdir My\ Document         # 使用转义符\ 将空格还原为空格字符本意而非Shell命令行中的命令参数间隔符
ll -d My\ Document

touch un gars, une fille
ll
rm  un gars, une fille
touch 'un gars, une fille'
ll
mkdir TVseries
mv 'un gars, une fille' TVseries

touch 'a&b'  b\&a           # & 在 Shell 中有特殊含义，表示后台执行
touch 'a$b'  b\$a           # $ 在 Shell 中有特殊含义，表示引用变量的值
touch 'a*b'  b\*a           # * 在 Shell 中有特殊含义，表示通配任意个任意字符
ls

touch ./-abc                # 以-开头的文件应使用路径前缀（绝对路径、相对路径均可）
mkdir ~/-ABC                # 以-开头的目录应使用路径前缀（绝对路径、相对路径均可）
ls
```

## 任务22： 文件通配符

```
fruits="coconut cherry grape fig lemon pear plum peach papaya cranberry blueberry"
touch $fruits
touch apple banana orange raspberry
touch apple1 banana2 orange3 raspberry4
touch $RANDOM $RANDOM $RANDOM $RANDOM $RANDOM
mkdir apple$RANDOM banana$RANDOM orange$RANDOM raspberry$RANDOM
touch applePI bananaPI orangePI raspberryPI
touch a${RANDOM}x b${RANDOM}y c${RANDOM}x d${RANDOM}x e${RANDOM}z
ls
```

```
ls *                 # 列出所有文件或目录，不包含隐含文件（即以.开头的文件）
ls .*                # 列出所有隐含文件或目录
ls *berry            # 列出所有以berry结尾的文件
ls ?????             # 列出名字为5个字符的文件或目录
ls ?p*               # 列出名字中第二个字符为p的文件或目录
ls ??p*[0-9]         # 列出名字中第三个字符为p且最后一个字符为数字的文件或目录
ls [abc]*            # 列出名字以a或b或c开头的文件或目录
ls [!abc]*           # 列出名字不以a或b或c开头的文件或目录
ls *PI               # 列出名字以PI结尾的文件或目录
ls *e*               # 列出名字中包含e的文件或目录
ls *[123]            # 列出名字以1或2或3结尾的文件或目录
ls *[0-9]*           # 列出名字中包含数字的文件或目录
ls [a-z]*[xyz]       # 列出名字以小写字母开头且以x或y或z结尾的文件或目录
ls *[!0-9]           # 列出名字不以数字结尾的文件或目录
ls [a-z0-9]*[!0-9]   # 列出名字以小写字母和数字开头且不以数字结尾的文件或目录

mv *[0-9] files
cp [A-Za-z0-9]*[a-z] files
rm *[0-9]* 
rm $fruits
rm -rf *\$* *\&* *\** ./-*
```

>**使用字符类型进行匹配**
>
>* [[:lower:]] 等价于 [a-z]
>* [[:upper:]] 等价于 [A-Z]
>* [[:alpha:]] 等价于 [a-zA-Z]
>* [[:digit:]] 等价于 [0-9]
>* [[:alnum:]] 等价于 [a-zA-Z0-9]
>* [[:cntrl:]] 任何一个控制字符
>* [[:blank:]] 任何一个 空格符 或 制表符（呈水平排列的空白字符）
>* [[:space:]] 任何一个 空格符 或 制表符 或 换行符 或 换页符 或 回车符（呈水平或垂直排列的空白字符）
>* [[:print:]] 任何一个可打印字符，包括空格
>* [[:graph:]] 任何一个可打印字符，不包括空格
>* [[:punct:]] 除了空格和字母数字之外的任何一个可打印字符（标点符号）

----

>**参考**
>
>* $ man 7 glob

## 任务23：大括号扩展

```
ll *PI
rm {apple,banana}PI      # 等效于 rm applePI bananaPI
mv raspberry{PI,}        # 等效于 mv raspberryPI raspberry
cp orangePI{,.bak}       # 等效于 cp orangePI orangePI.bak
ll *PI*
rm *PI*

touch {a..e}${RANDOM}        # 等效于 touch a${RANDOM} b${RANDOM} c${RANDOM} d${RANDOM} e${RANDOM}
ls [a-e]*
touch {m,n}{1,2,3}           # 扩展方式类似于离散数学中集合的笛卡尔积运算
ls [mn]*
touch {p,q}{1{a,b,c},2,3}    # {}可以嵌套
ls [pq][123]*
rm [a-e]* [mn]* [pq][123]*

cd
touch FRIENDS-season{1..10}-episode{1..24}.mp4  # 模拟下载了一部10季每季24集的电视剧
ls FRIENDS*
mkdir -p TVseries/FRIENDS/season{01..10}        # 创建并按照季目录进行整理
tree TVseries
mv FRIENDS-season1* TVseries/FRIENDS/season01
cd TVseries
mv ../FRIENDS-season2* FRIENDS/season02
cd FRIENDS/season03 
mv ~/FRIENDS-season3* .
cd ..
mkdir todo
mv ~/FRIENDS* todo
tree
cd
```

## 任务24：文件类型

```
ll -d /bin /var /etc/passwd /usr/bin/passwd /bin/zcat # 注意输出各行的第一个字符
ll /dev/sda{,1}  /dev/{console,tty0,ttyS0,null,zero,urandom}
```

```
file /bin /var /etc/passwd /usr/bin/passwd /bin/zcat 
file /dev/sda{,1}  /dev/{console,tty0,ttyS0,null,zero,urandom}
file *
```

>**参考**
>
>* [文件类型与其特征码](http://www.garykessler.net/library/file_sigs.html)


## 任务25： 链接文件

```
touch hello 
ll hello
ln hello bonjour          # 创建名为 bonjour 的硬链接
ll hello bonjour          # 注意输出第二列（以空格为间隔）数字的变化
ln bonjour ciao           # 创建名为 ciao 的硬链接        
ll hello bonjour ciao     # 注意输出第二列（以空格为间隔）数字的变化

rm hello
ll hello bonjour ciao     # 注意输出第二列（以空格为间隔）数字的变化
rm bonjour
ll hello bonjour ciao     # 注意输出第二列（以空格为间隔）数字的变化
rm ciao
ll hello bonjour ciao

touch hi
ln -s hi salut            # 创建名为 salut 的符号链接 
ln -s hi ciao             # 创建名为 ciao 的符号链接
ll hi salut ciao          # 注意输出中第一个文件类型字符
rm salut                  # 删除符号 salut 链接文件
ll hi salut ciao
rm hi                     # 删除被符号链接的文件 hi
ll hi salut ciao          # 符号链接文件 ciao 将成为死链

ll -d doc
mv doc mydoc
ln -s /usr/share/doc doc  # 对目录创建符号链接
cd doc
ll
cd
```

## 任务26：命令替换

```
echo "This system's name is $(hostname)."
echo "I am `whoami`"

# 防止命令替换被扩展
echo 'This system's name is $(hostname).'
echo "This system's name is \$(hostname)."
echo This system's name is \$(hostname).

touch backup_$(date +"%F")
rpm -ql $(rpm -qf $(which cp))         # $() 形式的命令替换可以嵌套
```

## 任务27： bash 整数运算

```
echo 10*2*5                          # shell 默认将字面常量视为字符串而非数字

echo $[10*2*5]                       # 使用 $[] 进行整数运算
echo $((10*2*5))                     # 使用 $(()) 进行整数运算

length=10
width=2
height=5

volume=$length*$width*$height        # shell 默认将变量视为字符串而非数字
echo $volume

volume=$[$length*$width*$height]
echo $volume
volume=$[length*width*height]        # 在 $[] 中的变量引用可以省略 $前缀
echo $volume
volume=$(($length*$width*$height))
echo $volume
volume=$((length*width*height))      # 在 $(()) 中的变量引用可以省略 $前缀
echo $volume

((volume=length*width*height))       # 可直接在 (()) 中进行赋值【c语言中的表达式都可以放在(())中】
echo $volume
```

## 任务28： 文本显示

```
cat /etc/passwd                      # 滚屏显示文本
nl /etc/passwd                       # 滚屏显示文本并显示行号，等同于 cat -n /etc/passwd

tac /etc/passwd                      # 纵向反转文本并显示
rev /etc/passwd                      # 横向反转文本并显示

more /etc/passwd                     # 分屏显示文本（不可回翻）【回车向下滚动一行，空格向下滚动一屏】
less /etc/passwd                     # 分屏显示文本（可回翻）【回车向下滚动一行，空格向下滚动一屏】

head /etc/passwd                     # 显示前10行
head -n 5 /etc/passwd                # 显示前5行，可简写为 head -5 /etc/passwd
head -c 5 /etc/passwd                # 显示前5个字符

tail /etc/passwd                     # 显示最后10行
tail -n 5 /etc/passwd                # 显示最后5行，可简写为 tail -5 /etc/passwd
tail -n +5 /etc/passwd               # 显示从第5行到文件尾
tail -c 5 /etc/passwd                # 显示最后5个字符
```

## 任务29：管道

```
ls -Rl /etc | less
ls -t | head -5 
tac /etc/passwd | head               # tail /etc/passwd | tac
tail -n +5 /etc/passwd | head        # 显示第5~15行

echo 'abcd 1234'|rev
echo '上海'|rev
echo '上海自来水来自海上'|rev

date +%s
date +%s | sha256sum
date +%s | sha256sum | base64        # base64 --help
date +%s | sha256sum | base64 | head -c 32   # 生成一个32位的随机口令

# 显示当前目录列表的同时将输出结果保存于文件 saved-output
ll | tee /tmp/saved-output
# 显示当前目录列表的同时将输出结果追加保存于文件saved-output并将ll的输出结果以邮件方式发送给 root，邮件标题为ll
ll | tee -a /tmp/saved-output | mail -s ll root
more /tmp/saved-output
```

## 任务30：使用 bc 进行数值运算

```
echo "1.23+4.5/6.7*8.9-10^2" | bc

echo "obase=2; 30" | bc             # obase=2 将输出基数设置为2（默认为10）；计算十进制数 30 的二进制数
echo "ibase=8; 30" | bc             # ibase=8 将输入基数设置为8（默认为10）；计算八进制数 30 的十进制数
echo "ibase=8; obase=16; 30" | bc   # 计算八进制数 30 的十六进制数

pi=3.14
r=1.23
area=$pi*$r*$r
echo "pi=$pi   --   r=$r   --   area=$area"

pi=$(echo "scale=10; 4*a(1)" | bc -l)  # scale=10设置输出的小数点后的保留位数；-l 启用算数函数库；pi=4*atan(1)
echo "$pi*$r*$r" | bc
area=$(echo "$pi*$r*$r" | bc)
echo "pi=$pi   --   r=$r   --   area=$area"
```

## 任务31：输出重定向

```
## 覆盖式
ls -Rl /etc  > /tmp/saved-output        # 标准输出重定向
less /tmp/saved-output
ls -Rl /etc 2> /tmp/saved-outerr        # 标准错误输出重定向
cat /tmp/saved-outerr
ls -Rl /etc  > /tmp/saved-output 2> /dev/null ; less /tmp/saved-output   # 多个命令放在一行上顺序执行用;间隔
ls -Rl /etc &> /tmp/saved-outputanderr ; less /tmp/saved-outputanderr    # 标准输出和错误输出同时重定向

## 追加式
echo "****************" >> /tmp/saved-output
ll >> /tmp/saved-output
echo "****************" >> /tmp/saved-outerr
ls -Rl /etc 2>> /tmp/saved-outerr ; cat /tmp/saved-outerr
echo "****************" >> /tmp/saved-outputanderr
ls -Rl /etc &>> /tmp/saved-outputanderr
ls -Rl /etc >> /tmp/saved-outputanderr 2>&1
```

## 任务32：输入重定向

```
cat < /etc/passwd

echo "This system's name is $(hostname)." > /tmp/mymail
echo "I am `whoami`" >> /tmp/mymail
echo >> /tmp/mymail    # 在 /tmp/mymail 尾部加添空行
ll  >> /tmp/mymail
mail -s test $USER < /tmp/mymail

tr -dc [:print:] < /dev/urandom | head -c 32  # 以系统随机字符设备作为tr命令的输入，生成一个32位的随机口令

cat <<END   # 将 END 之间的内容作为 cat 命令的输入
aimer
love
END
<Ctrl+D>

su -
cat >> /etc/hosts <<_END # 将 _END 之间的内容作为 cat 命令的输入，并以追加方式输出重定向到文件 /etc/hosts
192.168.0.77   web1
192.168.0.88   db1
192.168.0.99   win1
_END
exit

line=$(ls)
cat <<< $line              # 变量值做输入重定向
```

## 任务33： Shell变量作用域

```
## 1-为 var1 赋值
$ var1=UNIX
## 1-为 var2 赋值
$ var2=Linux
## 1-将变量 var2 的作用范围设置为全局
$ export var2
## 1-直接为全局变量 var3、var4 赋值
$ export var3=centos var4=ubuntu
## 1-在当前Shell中显示四个变量的值
$ echo $var1 $var2 $var3 $var4
UNIX Linux centos ubuntu
## 1-进入子Shell
$ bash
## 2-显示 var1 的值
## 2-由于var1在上一级Shell中没有被声明为全局，所以在子Shell里没有值
$ echo $var1
 
## 2-显示 var2、var3、var4 的值
## 2-由于这三个变量在上一级Shell中被声明为全局变量，所以在子Shell里仍有值
$ echo $var2 $var3 $var4
Linux centos ubuntu
## 2-在当前Shell中将 var2 设置为局部变量
$ export -n var2
## 2-在当前Shell中 var2 仍有值
$ echo $var2
Linux
## 2-进入孙子Shell
$ bash
## 3-由于 var2 在当前Shell的父Shell中已经设置为局部变量，所以在孙子Shell里没有值
## 3-当然，var1 在当前Shell的祖父Shell中就是局部变量，所以在当前Shell里没有值
$ echo $var1 $var2
 
## 3-由于var3和var4 在当前Shell的祖父Shell中设置为全局变量，
## 3-在当前Shell的父Shell中又没有变更，所以在当前Shell里仍有值
$ echo $var3 $var4
centos ubuntu
## 3-返回父Shell
$ exit
## 2-显示当前Shell中变量的值
$ echo $var2 $var3 $var4
Linux centos ubuntu
## 2-修改变量 var3 的值
$ var3=centos7.1
## 2-显示变量 var3 的值
$ echo $var3
centos7.1
## 2-返回父Shell
$ exit
## 1-已在父Shell中
$ echo $var1 $var2 $var3 $var4
UNIX Linux centos ubuntu
```

## 任务34：环境变量使用

```
echo $HISTIGNORE
export HISTIGNORE='pwd:ls:ll'         # 不将 pwd和ls和ll 记入命令历史
echo $HISTCONTROL
export HISTCONTROL='ignorespace:ignoredups'  # ignorespace 表示不将以空格开头的行记入命令历史
                                             # ignoredups 表示不将连续的重复行记入命令历史
											 # erasedups  表示剔除整个命令历史中的重复条目
echo $HISTSIZE $HISTFILESIZE
export HISTSIZE=10000              # 设置内存命令历史的行数
export HISTFILESIZE=10000          # 设置命令历史文件（$HOME/.bash_history）的行数
```

```
$ date
2016年 09月 27日 星期二 19:20:25 CST
$ LANG=C date                           # 使用通用语言（英语）显示命令输出结果，或 LANG=en_US.utf8 date
Tue Sep 27 19:20:29 CST 2016
$ TZ=Europe/Paris date                  # 显示巴黎的当前时间
2016年 09月 27日 星期二 13:20:48 CEST
$ LANG=C TZ=Europe/Paris date           # 使用英语显示命令输出结果
Tue Sep 27 13:20:57 CEST 2016	
$ LANG=fr_FR TZ=Europe/Paris date       # 使用法语显示命令输出结果
mar. sept. 27 13:21:06 CEST 2016
```

## 任务35：设置用户自己的工作环境

```
echo "export HISTIGNORE='pwd:ls:ll'" >> ~/.bash_profile
echo "export HISTCONTROL='ignorespace:ignoredups'" >> ~/.bash_profile
echo "export HISTSIZE=10000" >> ~/.bash_profile
echo "export HISTFILESIZE=10000" >> ~/.bash_profile

echo '
alias cp="cp -i"
alias rm="rm -i"
alias mv="mv -i"
alias cd..="cd .."
alias cd...="cd ../.."
' >> ~/.bashrc
```


----

> **提示**： 请以普通用户 student 登录完成下面的操作
>
> **提示**： 所有的文本显示、截取、排序等处理命令默认将处理结果显示在标准输出（屏幕）上，使用输出重定向可将结果保存为文件

----

>**参考**
>
>* 各个命令的手册
>* **正则表达式手册**  man 7 regex
>* <https://www.gnu.org/software/coreutils/manual/coreutils.html> 的 ch3～ch9


## 任务36：按关键字横向截取：`grep` 命令

```
grep root /etc/passwd            ## 显示 /etc/passwd 文件中包含 root 的行
grep root /etc/{passwd,group,crontab,fstab}  ## 显示多个文件中包含 root 的行
grep -n root /etc/passwd         ## 显示 /etc/passwd 文件中包含 root 的行，同时显示 root 出现的行号
grep -c root /etc/passwd         ## 显示 /etc/passwd 文件中包含 root 的行数

grep -i kernel /etc/issue        ## 显示 /etc/issue 文件中包含 kernel 的行，-i 忽略大小写 
grep 'FTP User' /etc/passwd      ## 当匹配字符串包含空格时需用引号括起来
grep -n '(C)' /bin/zcat          ## 当匹配字符串包含括号时需用引号括起来

echo 'echo "scale=10; 4*a(1)" | bc -l' > pi     ## 生成名为 pi 的文件
grep '*'  pi                     ## 在文件 pi 中查找包含字符 * 的行
grep '*' *                       ## 在当前目录的所有文件中查找包含字符 * 的行
grep '\r' /etc/issue             ## 在 grep 中，\ 视为 shell 的转义字符
grep '\r' /etc/issue -o          ## -o 仅显示匹配的字符串而非匹配的行
fgrep '\r' /etc/issue            ## 在 fgrep 中，所有匹配字符串均视为原始字符
fgrep '\r' /etc/issue -o

grep -2 ftp /etc/passwd          ## 显示匹配 ftp 的行，同时显示匹配行的前2行和后2行（-2 等价于 -C 2）
grep -B 2 ftp /etc/passwd        ## 显示匹配 ftp 的行，同时显示匹配行的前（Before）2行
grep -A 2 ftp /etc/passwd        ## 显示匹配 ftp 的行，同时显示匹配行的后（After）2行

grep ^root /etc/passwd           ## 显示 /etc/passwd 文件中以 root 开始的行
grep -c nologin$ /etc/passwd     ## 显示 /etc/passwd 文件中以 nologin 结尾的行数
grep -n '^[a-zA-Z]' /bin/zcat    ## 显示 /bin/zcat 文件中以字母开头的行并显示行号
grep -n ^$ /bin/zcat             ## 显示 /bin/zcat 文件中的所有空行并显示行号
grep -n $ /bin/zcat              ## $是RE，因为每行都有结尾，所以显示了全部文件

grep '\$' /bin/zcat              ## 取消$的RE含义，恢复$字符本身含义
grep \\$ /bin/zcat               ## 取消$的RE含义，恢复$字符本身含义
grep '\[' /bin/zcat              ## 所有RE具有特殊含义的字符均需类似地处理
grep \\[ /bin/zcat
fgrep $ /bin/zcat                ## 在 fgrep 中，所有匹配字符串均视为原始字符
fgrep [ /bin/zcat

grep -v ^# /bin/zcat             ## 显示 /bin/zcat 文件中 不以 # 开始的行
egrep -v '^#|^$' /bin/zcat       ## 显示 /bin/zcat 文件中 不以 # 开始的行且不显示空行
                                 ## egrep 中 可用 ERE（`|`属于ERE）
egrep 'root|student' /etc/passwd    ## 显示 /etc/passwd 文件中包含 root 或 student 的行
egrep '^(root|student)' /etc/passwd ## 显示 /etc/passwd 文件中以 root 或 student 开始的行

strings /bin/ls                  ## 在二进制文件中 查找可打印的字符串
strings /bin/zcat                ## 在文本文件中 查找可打印的字符串
## 使用 grep 过滤出用随机设备生成的字符串中的字母数字，截取20个字符（可作为随机口令）
strings /dev/urandom | grep -o [[:alnum:]] | head -n 20 | tr -d '\n'
## 使用 grep 过滤出用随机设备生成的字符串中的字母数字和除空白之外的特殊字符，截取20个字符（可作为随机口令）
strings /dev/urandom | grep -o [[:graph:]] | head -n 20 | tr -d '\n'

# 避免将查找对象被 bash 视为 grep 的选项
ls --help |grep -l
ls --help |grep '\-l'
ls --help |grep --author
ls --help |grep '\--author'
## 只列当前目录下的子目录 
ls -F | grep /$                  ## 或者 ls -l | grep ^d
## 计算当前目录下的文件数和目录数
ls -l * | grep ^- | wc -l 
ls -l * | grep ^d | wc -l  
## 查看引导信息中关于网卡的信息 
dmesg | grep eth
## 下面的四个目录是伪系统系统，是内存的一部分，每次启动机器会自动构建其数据，且会随系统的运行而变化
$ findmnt |egrep '/(proc|sys|run|dev) '
├─/sys             sysfs         sysfs      rw,nosuid,nodev,noexec,relatime
├─/proc            proc          proc       rw,nosuid,nodev,noexec,relatime
├─/dev             devtmpfs      devtmpfs   rw,nosuid,size=498172k,nr_inodes=124543,mode=755
├─/run             tmpfs         tmpfs      rw,nosuid,nodev,mode=755
```

```
grep -l root /etc/*                       ## 显示 /etc/ 下的哪些文件包含 root
grep -l root /etc/* 2> /dev/null |wc -l
grep -L root /etc/*                       ## 显示 /etc/ 下的哪些文件 不包含 root
grep -lr root /etc/* 2> /dev/null  ## 显示 /etc/ 下的哪些文件包含 root（`-r` 递归参数，即包含所有子目录中的文件）
grep -Lr root /etc/* 2> /dev/null |wc -l  ## （-r 递归参数，即包含所有子目录中的文件）
```


## 任务37：文本替换和删除：`tr` 命令

```
tr 'a-z' 'A-Z' < /etc/passwd            # 将文件中的小写字母转换成与之对应的大写字母
echo "HELLO WORLD" | tr 'A-Z' 'a-z'     # 将通过管道传递来的输入中的大写字母转换成与之对应的小写字母

grep '[()]' /bin/zcat
tr '()' '{}' < /bin/zcat                # 将文件中的 (转换成{； )转换成}
tr '()' '{}' < /bin/zcat |grep [{}]
tr '()' '[]' < /bin/zcat                # 将文件中的 (转换成[； )转换成]
tr '()' '[]' < /bin/zcat |egrep '\[|\]'

grep '[{}]' /etc/rc.local
tr '{}' '()' < /etc/rc.local
tr '{}' '()' < /etc/rc.local |grep '[()]'
tr '{}' '[]' < /etc/rc.local
tr '{}' '[]' < /etc/rc.local |egrep '\[|\]' 
tr '{}' '[]' < /etc/rc.local |grep -v if |egrep '\[|\]'

tr -s ' \n' < /bin/zcat                # 压缩文件中重复字符（空格、换行）为单个字符
echo "Thissss is     a text linnnnnnne." | tr -s ' sn'  # 压缩输入中的重复字符（空格、s、n）为单个字符

## 将文件中的空格和制表符替换成#
## *（星号）可以使 tr 命令重复#足够多次，以使置换字符集的字符个数与被置换字符集的字符个数一致。
tr -s '[:blank:]' '[#*]' < /usr/lib64/python2.7/colorsys.py        # 通过输入重定向为 tr 传递输入
cat /usr/lib64/python2.7/colorsys.py | tr -s '[:blank:]' '[#*]'    # 通过输入管道为 tr 传递输入

## 创建文件的单词列表
## 将文件中 除大小写字母外的字符（-c '[:alpha:]' 表示 '[:alpha:]' 的补集） 都转换成单个换行符。
## *（星号）可以使 tr 命令重复换行符足够多次，以使置换字符集的字符个数与被置换字符集的字符个数一致。
tr -sc '[:alpha:]' '[\n*]' < /bin/zcat
cat /usr/share/doc/nano-2.3.1/faq.html | tr -sc '[:alpha:]' '[\n*]' 
## 将处理结果中的大写字母替换为小写字母，之后再剔除重复的行
tr -sc '[:alpha:]' '[\n*]' < /bin/zcat | tr 'A-Z' 'a-z' |sort -u > wordlist
## 将多个文件纵向合并为一个文件
cat /bin/zcat /usr/share/doc/nano*/*.html > myfile
cat myfile | tr -sc '[:alpha:]' '[\n*]' | tr 'A-Z' 'a-z' |sort -u > wordlist
## 省去中间文件 myfile，通过管道将 cat 命令的输出直接传递给 tr 命令作为其输入
cat /bin/zcat /usr/share/doc/nano*/*.html | tr -sc '[:alpha:]' '[\n*]' | tr 'A-Z' 'a-z' |sort -u > wordlist

## 将 /dev/urandom 作为输入文件生成一个 20*512B （默认1块为512字节） 的输出文件 /tmp/random-file
dd if=/dev/urandom count=20 of=/tmp/random-file
ll /tmp/random-file
## 将输入文件中的所有非打印字符（有效控制字符除外）替换为?（问号），并将结果保存为 /tmp/random-file2
## *（星号）可以使 tr 命令重复?足够多次，以使置换字符集的字符个数与被置换字符集的字符个数一致。
tr -c '[:print:][:cntrl:]' '[?*]' < /tmp/random-file > /tmp/random-file2
## 省去中间临时文件，通过管道将 dd 命令的输出直接传递给 tr 命令作为其输入
dd if=/dev/urandom count=20 2> /dev/null | tr -c '[:print:][:cntrl:]' '[?*]' > /tmp/random-file2
## 将输入文件中的所有非打印字符删除，并将结果保存为 /tmp/random-file3
tr -dc '[:print:]' < /tmp/random-file > /tmp/random-file3
## 省略中间临时文件，通过管道将 dd 命令的输出直接传递给 tr 命令作为其输入
dd if=/dev/urandom count=20 2> /dev/null | tr -dc '[:print:]' > /tmp/random-file3
ll /tmp/random-file*

tr -d [:punct:] < /bin/zcat                  # 输出文件中的除了标点符号（包括一般的标点、所有括号等）之外的内容
echo "hello 1234 world 56" | tr -d '0-9'     # 将通过管道传递来的输入中的数字删除
tr -dc [:alnum:] < /dev/urandom | head -c 20 # 将随机设备生成序列中所有除了字母数值之外的字符删除并截取前20个字符

## 计算1到100之和
echo {1..100} | echo $[ $(tr ' ' '+') ]
echo {1..100} | tr ' ' '\n' | echo $[ $(tr '\n' '+') 0 ] 
echo {1..100} | xargs -n1 | echo $[ $(tr '\n' '+') 0 ]

```


## 任务38：随机排列（“洗牌”）： shuf 

```
cat /etc/passwd
shuf /etc/passwd   ## 以行为单位打乱文件内容的顺序，并在标准输出上显示

shuf -e apple banana cherry lemon orange pear raspberry  ## 将命令参数作为输入
shuf -e apple banana cherry lemon orange pear raspberry -n10 ## `-n` 参数用于指定输出的行数
shuf -e apple banana cherry lemon orange pear raspberry -n10 -r ## `-r` 参数指定输出可以包含重复的行
shuf -i 1-10                ## 将一个数值范围作为输入
shuf -i 1-20 -n10|xargs     ## 此处 xargs 的作用是将以换行为间隔符的输出转换成以空格为间隔符
shuf -i 1-10 -n20 -r|xargs

## 随机生成彩票
shuf -i 0-9 -n7 -r|xargs
echo "七星彩：$(shuf -i 0-9 -n7 -r|xargs)"
echo "排列五：$(shuf -i 0-9 -n5 -r|xargs)"
echo "排列三：$(shuf -i 0-9 -n3 -r|xargs)"

shuf -i 1-33 -n6 |tr '\n' ' ' ; shuf -i 1-16 -n1
shuf -i 1-35 -n5 ; shuf -i 1-12 -n2 |xargs
## 用()将一组命令的输出通过管道同时传递给下个命令(此处为xargs)做其输入
(shuf -i 1-35 -n5 ; shuf -i 1-12 -n2)|xargs 

echo "双色球：$(shuf -i 1-33 -n6|tr '\n' ' ')| $(shuf -i 1-16 -n1)"
echo "大乐透：$(shuf -i 1-35 -n5|tr '\n' ' ')| $(shuf -i 1-12 -n2|xargs)" 

echo "双色球：$(shuf -i 1-33 -n6| sort -n |tr '\n' ' ')| $(shuf -i 1-16 -n1)"      ## 对红球以数字方式排序
echo "大乐透：$(shuf -i 1-35 -n5| sort -n |tr '\n' ' ')| $(shuf -i 1-12 -n2| sort -n|xargs)" 

## 用随机数据填充表格，生成20行表格数据
echo st{01..20} | tr ' ' '\n' > /tmp/tpm-name
shuf -n 20 -e $(echo {A..Z}{a..z}{a..z}) > /tmp/tpm-nick
tr -dc [:alnum:] < /dev/urandom | head -c 200 |fold -w 10 > /tmp/tpm-pass # fold -w 10 表示每行宽度为10个字符
shuf -e male famale -n 20 -r > /tmp/tpm-gender
shuf -i 18-30 -n 20 -r > /tmp/tpm-age
eval echo 13{$(tr -dc 0-9 < /dev/urandom | head -c 180 |fold -w 9|tr '\n' ',')} |xargs -n1 > /tmp/tpm-tel
# tr -dc 0-9 < /dev/urandom | head -c 180 |fold -w 9|tr '\n' ',' 
# 从随机设备删除除数字之外的所有字符|截取前180个字符|以每行9个字符进行换行|将所有换行符替换为逗号
# $(tr -dc 0-9 < /dev/urandom | head -c 180 |fold -w 9|tr '\n' ',')    # 执行命令替换
# echo 13{$(tr -dc 0-9 < /dev/urandom | head -c 180 |fold -w 9|tr '\n' ',')}
# 试图执行大括号扩展，由于｛｝内有命令替换 $()，输出结果仅执行了命令替换，未对大括号进行扩展
# eval echo 13{$(tr -dc 0-9 < /dev/urandom | head -c 180 |fold -w 9|tr '\n' ',')}
# evel 命令首先扫描命令行进行所有的替换（变量替换、命令替换），然后再执行命令，从而扩展了大括号表达式
# |xargs -n1 类似于 |tr ' ' '\n'
# 或 eval echo 13{$(shuf -i 0-9 -n180 -r | tr -d '\n'   |fold -9| tr '\n' ',')} |xargs -n1 > /tmp/tpm-tel
# 或 eval echo 13{$(shuf -i 0-9 -n180 -rz| tr -d '\000' |fold -9| tr '\n' ',')} |xargs -n1 > /tmp/tpm-tel

#echo -e "Name\tNick\tPassword\tGender\tAge\tTel" > usertable
paste /tmp/tpm-{name,nick,pass,gender,age,tel} >> usertable # 将多个文件横向合并（以制表符为列间隔符）
rm /tmp/tpm-{name,nick,pass,gender,age,tel}
cat usertable                  # （因为数据随机产生，所以您的数据与列出的不一致，但数据格式一致）

st01    Syu     LeLJpG9KTU      famale  28      13342675170
st02    Ohn     N30uLpQYh1      famale  19      13332881942
st03    Rmo     fZZHqVMfae      famale  25      13347745308
st04    Mju     CioZLy4Pt0      male    22      13459810354
st05    Biu     aAyFvdSeTu      male    22      13673659969
st06    Psk     EGaJ5OLLzU      male    27      13988407444
st07    Tln     JPNxVoRUmr      male    18      13770716174
st08    Ptu     ve39zRTmiL      male    21      13823448278
st09    Xaw     jW9gIOf1xr      famale  29      13195204321
st10    Ibo     5oskpYfdeY      famale  28      13716142384
st11    Qbx     WKH38Remu8      famale  18      13988542041
st12    Bdj     EhTujFPuaV      famale  28      13364446233
st13    Vel     i5BAGUvFGL      male    21      13557400946
st14    Gdb     xOUc2ArRvd      male    19      13657805803
st15    Nzn     sRcLxaAGMJ      famale  30      13105101475
st16    Jka     RFNSBwG8P8      famale  18      13807347024
st17    Mmt     GbP1yCXdKA      male    20      13253259923
st18    Hli     P4N6MZFhAR      male    18      13282951065
st19    But     GK3Ckrduil      famale  18      13747153535
st20    Nta     laYnn9puAO      famale  24      13935185913


## 用随机数据生成一个简易版的 Apache 访问日志文件
## 随机生成25个IPv4地址
shuf -i 1-254 -n100|sed 'N;N;N;s/\n/./g' > /tmp/tpm-ip
## 随机生成25个日期时间（格式与 Apache 访问日志文件中的日期字段一致）
for d in $(shuf -i $(date +%s -d '-180 days')-$(date +%s -d '+180 days') -n 25) 
 do echo "- - [$(LANG=C date -d "@$d" +'%d/%h/%Y:%T %z')]" ; done > /tmp/tpm-date
paste -d ' ' /tmp/tpm-{ip,date} > logfile # 将多个文件横向合并（-d ' ' 指定以空格为列间隔符）
rm /tmp/tpm-{ip,date} 
cat logfile                   # （因为数据随机产生，所以您的数据与列出的不一致，但数据格式一致）

68.39.177.6 - - [08/Aug/2016:02:00:46 +0800]
225.189.211.29 - - [18/Aug/2016:17:11:31 +0800]
50.155.240.250 - - [30/Jul/2016:01:36:16 +0800]
121.226.58.74 - - [28/Jan/2017:14:47:45 +0800]
107.17.243.35 - - [19/Aug/2016:11:56:29 +0800]
………………

```

## 任务39：sort 命令

```
shuf -e software-v{1,3,10}.{0,1,12} > /tmp/tosort1
# 对文件 /tmp/tosort1 进行排序（依次比较每行的每个字符，默认按ASCII码升序排列）
sort /tmp/tosort1
sort -r /tmp/tosort1   （-r 按降序排列）
# 将包含小数点的数字视为版本号排序，将排序结果使用输出重定向保存为 /tmp/tosort2
sort -V /tmp/tosort1 > /tmp/tosort2
# 若将排序结果重新写回输入文件，可使用 -o 选项指定文件名
sort -V /tmp/tosort1 -o /tmp/tosort1

# 对 usertable 的第5列排序
sort -k5 usertable             # 将从第5列开始到最后一列的内容视为排序对象
sort -k5,5 usertable           # 严格按照第5列排序
sort -k5n usertable            # 对第5列以数字排序（以数字方式排序不会跨列，即 -k5,5n可简作 -k5n）
# 对 usertable 的第5列排序，并剔除重复
sort -k5,5 -u usertable 
# 对 usertable 的第5列排序，若排序结果相同再按第4列排序，并剔除重复
sort -k5,5 -k4,4 -u usertable 
# 对 usertable 的第5列排序，若排序结果相同再按第1列排序
sort -k5,5 -k1,1 usertable
# 对 usertable 的第5列排序，若排序结果相同再按第1列的第3~4字符按数字排序
sort -k5,5 -k1.3,1.5n usertable
# 对 usertable 的第4列以逆序排序，若排序结果相同再按第5列以数字排序
sort -k4,4r -k5n usertable
# 对 /etc/passwd 的第3列以数字方式排序（以:作为列间隔符）
sort -t ':' -n -k3  /etc/passwd
# 对 /etc/passwd 的第5列排序（b用于忽略前导空格或制表符），若排序结果相同再对第3列以数字方式排序
sort -t ':' -k 5b,5 -k 3,3n /etc/passwd
sort -t ':' -n -k 5b,5 -k 3,3 /etc/passwd
sort -t ':' -b -k 5,5 -k 3,3n /etc/passwd

# 对 logfile 按IP地址排序（对以.间隔的四个数字分别以数字方式排序）
sort -t '.' -k 1,1n -k 2,2n -k 3,3n -k 4,4n logfile
# 对 logfile 按日期和时间排序
sort -t ' ' -k 4.9n -k 4.5M -k 4.2n -k 4.14,4.21 logfile
# -k 4.9n 对第4列第9个字符开始（终结于:，以为:不是合法数字字符）的内容以数字方式排序 （年份）
# -k 4.5M 对第4列第5个字符开始（终结于/，以为/不是合法月份字符）的内容以月份方式排序 （月份）
# -k 4.2n 对第4列第2个字符开始（终结于/，以为/不是合法数字字符）的内容以数字方式排序 （日）
# -k 4.14,4.21 对第4列第14个字符开始到第21个字符以ASCII码排序（时间）

# 以降序方式显示使用磁盘空间最多的普通用户的前十名
$ su -
# du -cks /home/*|sort -rn |head -11
# exit
$
# 按内存使用从大到小排列输出进程。
ps -e -o "%C : %p : %z : %a"|sort -k5 -nr
# 按CPU使用从大到小排列输出进程。
ps -e -o "%C : %p : %z : %a"|sort –nr
```

> **参考**： <http://www.cnblogs.com/51linux/archive/2012/05/23/2515299.html>

## 任务40：cut、wc、uniq 命令

```
cat usertable logfile > newfile   # 纵向合并多个文件

cut -f 1,3-5 usertable         # 截取出第1,3,4,5列（默认以tab为列间隔符）
cut -d' ' -f 1,4 logfile       # 以空格为列间隔符截取出第1,4列
cut -d':' -f1,3,6 /etc/passwd  # 以冒号为列间隔符截取出第1,3,6列

cut -d' ' -f4,5 logfile| cut -c14-21 | 截取 logfile 文件中的时间（cut -c14-21截取第14~21个字符）

cut -f5 usertable | sort -n |uniq     # uniq 只剔除连续的重复行，所以使用 uniq 之前应先排序
cut -f5 usertable | sort -n |uniq -c  # uniq -c 计算重复行的个数
cut -f4 usertable | sort -r |uniq -c  # 计算 男女 （第4列为性别）各多少人

wc usertable                   # 显示文件的行数（l）、字数（w）、字符数（c）
wc -l usertable                # 显示文件的行数（l）
wc -L /etc/passwd              # 显示文件最长行的字符数

ifconfig |grep -v ^' '|grep -v ^$| cut -d: -f1  # 显示所有网络接口设备名称
ip a | grep '^[0-9]' | cut -d: -f2              # 显示所有网络接口设备名称
ip a | grep '^[0-9]' | cut -d: -f2 | wc -l      # 显示所有网络接口设备的个数
ip a | grep '^[0-9]' | grep -v lo | cut -d: -f2 | wc -l  # 显示除了 lo 的所有网络接口设备的个数

# 显示 enp0s3 的 IPv4 地址
ifconfig enp0s3 | grep 'inet ' | tr -s ' '| cut -d' ' -f3
ifconfig enp0s3 | grep 'inet ' | tr -dc '[0-9. ]' | tr -s ' ' | cut -d' ' -f2
ifconfig enp0s3 | grep 'inet ' | cut -d: -f2 | tr -s ' ' | cut -d' ' -f1
ifconfig enp0s3 | grep 'inet ' | cut -d: -f2 | awk '{print $1}'

ip addr show enp0s3 | grep 'inet '| cut -d'/' -f1 | tr -s ' ' | cut -d' ' -f3
ip addr show enp0s3 | grep 'inet '| tr -dc '[0-9./]'|cut -d'/' -f1
```

## 任务41：行编辑器：sed

```
# 将每行开始的 st 替换为 ST，并在屏幕显示
sed 's/^st/ST/' usertable
# 将每行开始的 st 替换为 ST，并将替换结果写入文件（-i）
sed -i 's/^st/ST/' usertable
# 将13替换为ac，若一行上有多个13，则全部（g）替换为ac，并在屏幕显示
sed 's/13/ac/g' usertable

# 显示 enp0s3 的 IPv4 地址
ifconfig enp0s3 | grep 'inet ' | sed -e 's/[a-z]//g' -e 's/^[ \t]*//' -e 's/  */ /g' |cut -d' ' -f1
#ifconfig enp0s3|取出包含inet的行|删除所有小写字母、删除所有前导空格或制表符并将多个空格替换成一个|截取第一列

ifconfig enp0s3 | grep 'inet ' | sed 's/[^0-9.:]//g' | cut -d: -f2


## 将文本的奇数行和偶数行合并 
sed '$!N;s/\n/ /'  logfile   # 若不是末行（$!）就将下一行追加读入到缓冲区，并将缓冲区中的换行符替换为空格符
## 生成25个随机IPv4地址 
shuf -i 1-254 -n100 | sed 'N;N;N;s/\n/./g'
## 将文本的第2行和第3行合并 
sed '2,3 N; s/\n/ /' logfile
sed -n '2,3 N; s/\n/ /p' logfile            # 只输出合并的行


## 计算当前用户使用最频繁的10个命令（包括通过管道传递的命令）
## 参考： https://github.com/logotype/useful-unix-stuff
history | sed "s/^[0-9 ]*//" | sed "s/ *| */\n/g" | awk '{print $1}' | sort | uniq -c | sort -rn | head -n 10

```

>#### 参考
> * https://www.gnu.org/software/sed/manual/sed.html
> * [SED 简明教程](http://coolshell.cn/articles/9104.html)
>#### 练习
> 使用 `sed` 命令将 [DevOps实践指南.txt](/assets/exercises/The-DevOps-Handbook.txt) 格式化成 [DevOps实践指南.md](/assets/exercises/The-DevOps-Handbook.md) 。 


## 任务42：awk

```
## 打印所有行（即省略匹配筛选部分）
awk '{print}' usertable                     # 等效于 cat usertable
awk '{print NR,$0}' usertable               # 为每行前添加行号，类似于 cat -n usertable
awk '{print NR "\t" $0}' usertable          # 为每行前添加行号和制表符
awk '{print NR "\t" $1,$3,$6}' usertable 
awk '{print NR "\t" substr($4,2,11),substr($4,11,8)}' logfile   # 使用函数substr取子串
awk -F':' '{print $1,$3}' /etc/passwd       # 指定以冒号作列间隔符，默认的列间隔符为空格或制表符
## 使用正则表达式书写 pattern
awk '/ST[12].*/' usertable                  # 打印正则表达式匹配的行
awk '/ST[12].*/ {print $1,$3,$6}' usertable # 打印正则表达式匹配行的第1,5,6列
awk '/ST[12].*/ {print $1,substr($3,5,4),$6}' usertable # 使用函数substr取子串
awk '/^1.*/ {print NR "\t" $1 "\t" substr($4,2,11),substr($4,11,8)}' logfile
awk -F':' '/^r.*/ {print $1,$4}' /etc/passwd
## 使用关系表达式书写模式
awk 'NR % 2 == 1' usertable                 # 打印所有奇数行
awk 'NR % 2 == 1 {print $1,$3,$6}' usertable
awk 'NR % 2 == 1 {print $1,substr($3,5,4),$6}' usertable 
awk 'NR % 2 == 1 && substr($6,1,3) == '131' {print $1,$3,$6}' usertable
awk 'NR%2==1 && ($5<25 || $4=='male')' usertable
## 使用范围匹配
awk '/ST05/,/[MN].*male/' usertable
awk '/ST05/,NR==10 {print $1,$2,$3,$5}' usertable

# 从 uptime 的输出过滤出时间和三个平均负载字段
uptime |awk -F '[ ,]+' '{print $2" - "$11,$12,$13}'     # 将一个或多个空格或逗号视为列间隔符
# 过滤出 enp0s3 网络接口当前的IPv4地址
ip a s enp0s3|grep 'inet '| awk -F '[ /]+' '{print $3}' # 将一个或多个空格或/视为列间隔符
ifconfig enp0s3|grep 'inet '| awk -F ' +' '{print $3}'  # 将一个或多个空格视为列间隔符
# 显示除了 lo 之外的所有网络接口
ifconfig -a |grep '^\w'|awk '!/lo/{print $1}'           # grep '^\w' 过滤出以字母数字下划线开头的行
ip a|grep '^[0-9]' | awk -F '[ :]' '!/lo/{print $3}'    # 将一个或:视为列间隔符


# 统计 /etc/passwd 的账户数
awk '{count++;print $0;} END{print "user count is ", count}' /etc/passwd  # count未赋初值，默认为0
awk 'BEGIN {c=0;print "[start] count=" c} {c++;print $0;} END{print "[end]user count is", c}' /etc/passwd
# 统计当前目录下的文件占用的字节数
ls -l|grep -v ^d|awk 'BEGIN {size=0} {size+=$5} END{print "[end]size is", size}'
ls -l|grep -v ^d|awk 'BEGIN {size=0} {size+=$5} END{print "[end]size is", size/1024/1024,"M"}' # 以MB为单位
# 统计当前目录下所有12月份文件的字节数
$ LANG=C ls -l /etc | grep -v ^d| awk '$6 == "Dec" { sum += $5 } ; END { print sum }'

## 使用 awk 的数组
# 统计  usertable 文件中各个年龄的人数
# 以年龄为数组下标，对应下标的数组元素值为年龄的个数
awk '{++age[$5]}; END{for (i in age) print i, age[i]}' usertable  
# 查看TCP连接的各种连接状态的连接数
# 以TCP状态为数组下标，对应下标的数组元素值为TCP状态的个数（netstat -nt输出的最后一列$NF为状态）
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
netstat -nt| awk '{++S[$NF]} END {for(a in S) print a, S[a]}'
```

> **参考**
>
>* https://www.gnu.org/software/gawk/manual/gawk.html
>* [AWK 简明教程](http://coolshell.cn/articles/9070.html)


## 任务43：文本编辑器 vim

```
vimtutor             # 通过 vimtutor 学习 vim 的使用
```

> **参考**： 
>
>* [Vim Tutorial](https://linuxconfig.org/vim-tutorial)
>* [Learn Vim Progressively](http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/) -- [简明 Vim 练级攻略](http://coolshell.cn/articles/5426.html)
>* [Vim Adventures](http://vim-adventures.com/) -- [VIM大冒险](http://coolshell.cn/articles/7166.html)
>* https://www.linuxtrainingacademy.com/vim-cheat-sheet/
>* https://www.linuxtrainingacademy.com/linux-text-editors-tutorials/





