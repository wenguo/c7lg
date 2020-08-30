# 实验指导 1.1 - 安装 Linux 虚拟机

>#### 学习目标
> * 安装和使用 VirtualBox
> * 最小化安装 CentOS 7


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


## 参考资源

* https://access.redhat.com/documentation/zh_cn/red-hat-enterprise-linux/
* https://access.redhat.com/documentation/zh-CN/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/
\newpage
# 实验指导 1.2 - 初入 Linux

>#### 学习目标
> * 学会本地登录和远程登录
> * 学会使用命令帮助
> * 学会获取系统基本信息
> * 学会关闭和重启系统

## 任务1：本地登录和远程登录

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

## 任务2：获得命令帮助

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

## 任务3：使用 `su -` 切切换用户身份

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

## 任务4：安装必要的软件并更新系统

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

## 任务5： 重启系统

* 执行如下命令之一重启系统
      # systemctl reboot
      # reboot
      # shutdown -r now

## 任务6：  获取系统基本信息

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

## 任务7：  系统基本设置

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

## 任务8： 关闭系统

* 执行如下命令之一关闭系统
      # systemctl poweroff
      # poweroff
      # shutdown -h now

\newpage
# 实验指导 2.1 - 命令行操作基础

>#### 学习目标
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

----

> **提示**： 请以普通用户student登录完成下面的操作

## 任务1： 熟悉 Linux 文件系统层次结构

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


## 任务2： 目录操作命令

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

## 任务3： 绝对路径与相对路径

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


## 任务4： 命令补全

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

## 任务5：命令历史

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

## 任务6：复制、移动、删除文件

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

## 任务7：命令别名

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

## 任务8： bash 变量定义、引用与显示

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

## 任务9： 特殊文件名

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

## 任务10： 文件通配符

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

## 任务11：大括号扩展

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

## 任务12：文件类型

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


## 任务13： 链接文件

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

## 任务14：命令替换

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

## 任务15： bash 整数运算

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

## 任务16： 文本显示

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

## 任务17：管道

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

## 任务18：使用 bc 进行数值运算

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

## 任务19：输出重定向

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

## 任务20：输入重定向

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

## 任务21： Shell变量作用域

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

## 任务22：环境变量使用

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

## 任务23：设置用户自己的工作环境

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

\newpage
# 实验指导 2.2 - 文本编辑与处理

>#### 学习目标
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


## 任务1：按关键字横向截取：`grep` 命令

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


## 任务2：文本替换和删除：`tr` 命令

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


## 任务3：随机排列（“洗牌”）： shuf 

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

## 任务4：sort 命令

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

## 任务5：cut、wc、uniq 命令

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

## 任务6：行编辑器：sed

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


## 任务7：awk

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


## 任务8：文本编辑器 vim

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






\newpage
# 实验指导 3.1 - 账户管理与口令管理

>#### 学习目标
> * 使用 `useradd`/`usermod`/`userdel` 命令 添加/修改/删除 用户
> * 使用 `groupadd`/`groupmod`/`groupdel` 命令 添加/修改/删除 组
> * 使用 `id` / `group`命令显示 用户/组 信息
> * 使用 `su` / `sg` 或`newgrp` 命令切换 用户/组
> * 使用 `passwd` 命令管理用户口令
> * 使用 `gpasswd` 命令管理组口令和组成员
> * 使用 `chage` 命令设置已存在用户的口令时效策略
> * 修改 `/etc/login.defs` 文件为新建用户设置口令时效策略


## 任务1：显示用户和组管理的相关信息

* 显示当前用户的用户和组信息
* 查看添加用户时读取的默认（范围）值
* 查看使用 useradd 命令添加用户时的默认值
* 列表显示默认系统账户数据库文件及其权限
* 查看 shadows 手册明确每一列的含义

## 任务2：创建用户并为其设置登录口令

* 若当前不是 root 用户，请切换到 root 用户环境
* 查看 useradd 的命令帮助
* 添加名为 toni 的用户并为其设置口令
* 查看 /home/toni 目录自身的权限
* 显示 /home/toni 目录下的所有文件（包括隐含文件）
* 显示指定用户 toni 的用户和组信息
* 显示4个账户文件最后一行的内容，熟悉各个字段的含义

## 任务3：修改用户名和组名

* 将 toni 用户的名字改为 tony，同时将其主目录移动到 /home/tony
* 将 toni 组的名字改为 tony
* 检验更改结果

## 任务4：组成员管理

* 添加名为 leader 的组
* 添加用户 emily（同时会创建同名主组），并指定其附加组为 leader
* 添加用户 jacob，并指定其主组为 leader
* 将用户 tony 的附加组设置为 leader
* 查看 leader 组中的成员
* 添加名为 designer 的组
* 将 tony 用户加入 designer 组，并确保其还是 leader 的组成员 


* 创建 assistant 组
* 创建 5 个用户
* 设置 5 个均为 assistant 组的成员
* 查看 assistant 组中的成员
* 为 assistant 组添加1个新成员
* 从 assistant 组剔除1个成员


>**选择你喜欢的用户名**
>* **!m!** austin victor charles richard alexander jacob michael matthew
>* **!m!** nicholas christopher joshua jason tyler joseph
>* **!f!** alexis amanda tina stella christina hannah alina gloria bella emily 
>* **!f!** brianna samantha hailey ashley kaitlyn madison brandon sarah 


## 任务5： 删除用户和组

* 删除所有你新创建的用户和组（除了 tony）

## 任务6： 口令管理与口令时效

* 以 tony 用户登录并重新设置其口令
* 使用非交互模式将 student 用户的口令设置为 5iCent0S （使用 su - -c）
* 锁定用户 student，显示其口令状态，并尝试以 student 用户身份登录
* 解除对 student 用户的锁定，并尝试以 student 用户身份登录
* 设置新建用户的口令策略，至少每隔90天修改一次自己的口令
* 设置对 student 用户的口令策略，至少每隔60天修改一次自己的口令
* 设置 tony 用户 180 天后到期，并提前7天预警
* 强制用户 student 在首次登录时修改其口令
* 以 student 用户身份登录，并修改其口令


\newpage
# 实验指导 3.2 - 权限管理和进程管理

>#### 学习目标
> * 使用 `chmod` 命令设置文件/目录的 基本/特殊 权限
> * 使用 `chown`/`chgrp` 更改文件/目录的 属主/同组人
> * 使用 `umask` 设置文件和目录的生成掩码
> * 使用 `getfacl`/`setfacl` 显示/设置 文件/目录的 FACL 权限
>* 进程管理 
> * 进程查看、显示命令的执行时间
> * 杀死、调整运行优先级
> * 在后台执行进程，注销后保证进程继续执行
> * 实施作业控制

## 任务1：权限管理

> 请将所有新创建用户的初始口令设置为：5L0ve!iNUx

1. 准备用户、组和目录
  * 添加组 admin，指定其 GID 为 6000
  * 添加用户 tom，其附加组为 admin，且其用户主目录为 /home/tommy
  * 添加用户 tyler，其主组为 admin
  * 添加用户 nicholas，其 UID 为 888，不允许该用户登录系统
  * 添加用户 sarah，该账户第一次登录系统时即被系统要求修改密码
  * 添加用户 amanda，该账户会在 14 天后被禁用，其 umask 值为 0022
  * 创建目录 /admin/{ventes,devel,formation,other,default}
2. 分配对不同目录的访问权限
  * 目录 /admin/ventes 的所有者为 root, 所属组为 admin,隶属于admin 组的所有用户均可在此目录下创建文件，但新建文件和目录的所属组自动是 admin, 属主是创建者自身，其它用户无任何权限
  * 目录 /admin/devel 的属主和属组为 root，所有人均可在此目录下创建文件，但不能删除别的用户创建的文件
  * 目录 /admin/formation 的属主和属组均为 root, 用户 root 拥有所有权限，
root 组只能读和执行，其他人无任何权限，但是用户 tyler 对此目录具有一切权限, 而用户 tom 可以读取该目录下的文件
  * 在目录 /admin/default 下的文件（已经存在的和将来要创建的）对于 sarah 用户都具有一切权限
  * 所有人在目录 /admin/other 下均可创建和删除文件，但以 root 身份创建文件的 /admin/other/notice 文件不能被任何人（包括 root）删除和重命名


## 任务2：进程管理

* 显示进程
  * 显示出当前用户在shell下所运行的进程的信息
  * 只查看用户 student 的进程的信息
  * 列出系统中正在运行的所有进程的详细信息
  * 列出系统中正在运行的所有进程的详细信息，并想看清所运行的进程的完整命令行
  * 列出系统中正在运行的所有进程的详细信息，对 -pcpu 输出列进行排序
  * 显示系统进程树
  * 列出 sshd 进程的详细信息
  * 列出 sshd 进程的 PID （请先远程登录几次，再查看）
  * 列出 root 用户运行的 sshd 进程的 PID
  * 列出 student 组运行的所有进程的PID
  * 列出 root 用户或 daemon 用户运行的所有进程的PID
* 作业/进程控制
  * 在前台运行第1个 sleep 进程（1小时），然后以交互方式挂起之
  * 在后台运行第2个 sleep 进程（2小时）
  * 列出所有正在运行的作业状态
  * 列出所有正在运行的作业状态，同时列出进程PID
  * 将第1个 sleep 进程调度到后台继续执行
  * 列出所有正在运行的作业状态，同时列出进程PID
  * 显示所有名为 sleep 进程的 pid 和 nice 值
  * 尝试调高第1个 sleep 进程的优先级
  * 尝试调低第1个 sleep 进程的优先级
  * 尝试再次调高第1个 sleep 进程的优先级
  * 将第1个 sleep 进程调度到前台
  * 终止第1个 sleep 进程的执行
  * 杀死第2个 sleep 进程
  * 强行杀死名为 bash 的进程，发生了什么？为什么？

>**提示** 请使用 student 和 root 身份分别执行上面的作业控制，找出不同之处

## 参考资源

  * http://people.cs.pitt.edu/~alanjawi/cs449/code/shell/UnixSignals.htm





\newpage
# 实验指导 4 - 本地存储管理

>#### 学习目标
> * 分区工具
>   * 使用 `fdisk`/`cfdisk`  管理 MBR 分区
>   * 使用 `**gdisk**`/`cgdisk`  管理 GPT 分区
>   * 使用 `parted` 管理 MBR/GPT 分区
>   * 使用 `partx`/`kpartx` 通知内核强制重读磁盘分区表
> * 文件系统管理
>   * 使用 `mkfs.{ext4,xfs}` 创建文件系统
>   * 使用 `mount`/`umount` 挂装/卸装 文件系统
>   * 使用 `fuser` 终止所有正在访问某挂载点的进程
>   * 使用 `blkid` 命令显示文件系统的 卷标/UUID
>   * 修改 `/etc/fstab` 在系统启动时挂装文件系统
>   * 使用 `mkswap`、`swapon`/`swapoff` 管理交换空间
>   * 使用 `fsck`/`xfs_repair` 检查和修复文件系统
> * 磁盘/文件系统常用工具
>   * `dd`、`df`、`du`
>   * `find`
> * 磁盘限额
>   * 使用 `setquota` 或 `edquota` 配置 ext3/4 文件系统的磁盘限额
>   * 使用 `xfs_quota` 配置 xfs 文件系统的磁盘限额
> * 逻辑卷管理
>   * 使用 `{pv,vg,lv}create` 创建 物理卷/卷组/逻辑卷
>   * 使用 `{pv,vg,lv}{s,display}` 查看 物理卷/卷组/逻辑卷
>   * 使用 `{vg,lv}{extend,reduce}` 扩展和收缩 卷组/逻辑卷 
>   * 使用 `resize2fs` 扩展和收缩 ext 文件系统
>   * 使用 `xfs_growfs` 扩展 xfs 文件系统


## 任务1：磁盘分区

* 为虚拟机添加一块 20G 大小的硬盘
* 为磁盘设置 GPT 分区表
* 添加 3 个分区，分别为
  * 5G, Linux 分区类型
  * 3G, Linux 分区类型
  * 1G，swap  分区类型
* 在不重启系统的前提下，让 Linux 内核重新读取新硬盘的分区表

>##### 参考
>
>* https://wiki.archlinux.org/index.php/Partitioning
>* https://wiki.archlinux.org/index.php/Fdisk


## 任务2：创建和 挂装/卸装 文件系统

* 在大小为 5G 的分区上创建 xfs 文件系统
* 在大小为 3G 的分区上创建 ext4 文件系统
* 创建两个文件系统挂装点 /mnt/{xfs，ext4}
* 将大小为 5G 的文件系统手动挂装到 /mnt/xfs
* 将大小为 3G 的文件系统手动挂装到 /mnt/ext4
* 检查文件系统的挂装情况
* 复制 /usr 整个目录的内容到 /mnt/xfs
* 进入 /mnt/ext4 目录，将 /etc 整个目录的内容复制到当前目录
* 使用 fuser 命令分别查看两个挂装点目录下有无进行在运行
* 分别手动卸装 /mnt/{xfs，ext4}

## 任务3：交换空间

* 交换分区
  * 在大小为 1G 的分区上创建 swap 空间
  * 查看当前 swap 空间大小
  * 激活大小为 1G 的 swap 空间
  * 查看当前 swap 空间大小
  * 去激活大小为 1G 的 swap 空间
  * 查看当前 swap 空间大小
* 交换文件
  * 创建一个大小为 512M 的交换文件
  * 为此交换文件创建 swap 空间
  * 查看当前 swap 空间大小
  * 激活交换文件上的 swap 空间
  * 查看当前 swap 空间大小
  * 去激活交换文件上的 swap 空间
  * 查看当前 swap 空间大小

## 任务4：配置系统启动时挂装

* 将大小为 5G 的 xfs 文件系统挂装到 /data（使用文件系统UUID）
* 将大小为 3G 的 ext4 文件系统挂装到 /srv
* 激活大小为 1G 的交换分区
* 模拟系统启动，检查文件系统和交换分区的挂装情况

## 任务5：挂装 iso 文件

* 下载一个 iso 文件
      wget -c http://www.tinycorelinux.net/8.x/x86/release/Core-current.iso 
* 创建 /mnt/iso 挂装点
* 将 iso 文件只读挂装到 /mnt/iso
* 显示挂装点目录的内容
* 解除挂装


## 任务6：使用 dd 命令将 iso 文件的内容写入U盘

>**提示** 本任务使用虚拟块设备模拟了一个 16M 大小的 U盘

* 准备 loop 设备
      # dd if=/dev/zero of=/tmp/vdisk1.dd bs=1M count=16
      # losetup -fP /tmp/vdisk1.dd
      # losetup -a
      /dev/loop0: [64768]:4221244 (/tmp/vdisk1.dd)
      # lsblk -io NAME,TYPE,SIZE,MOUNTPOINT,FSTYPE,MODEL| egrep 'loop|^NAME'
* 准备 iso 文件
      # wget http://www.tinycorelinux.net/8.x/x86/release/Core-current.iso
* 将 iso 文件的内容写入虚拟块设备
      # dd if=Core-current.iso of=/dev/loop0
      # lsblk -io NAME,TYPE,SIZE,MOUNTPOINT,FSTYPE,MODEL| egrep 'loop|^NAME'
* 检查写入的虚拟块设备（U盘）内容
  * 将虚拟块设备 挂装到 /mnt/iso
  * 显示挂装点内容
  * 解除挂装

## 任务7：文件系统检查和修复

>**对挂装点 /data 和 /srv 分别进行如下操作

* 手动卸装文件系统
* 检查文件系统
* 向挂装点对应的分区里写入连续随机数据，模拟分区损坏
  * 越过 100 块，像设备写入 10 块（每块 512 字节）数据
* 检查文件系统
* 修复文件系统
* 重新挂装文件系统，并查看分区中的原有数据

## 任务8：df 和 du 命令

* 显示 /etc 目录中每个文件的磁盘占用
* 显示 /etc 目录总共占用了多少
* 显示 xfs 和 ext4 文件系统的剩余空间
* 显示除了 tmpfs 之外，所有文件系统的剩余空间，并按照使用率降序输出

## 任务9： find 命令

* 查找 /var 目录下属主为root，且属组不为 root 的所有文件或目录
* 查找 /var 目录下属主为root，且属组不为 root 的所有常规文件
* 查找 /bin /sbin 目录下所有 设置了 SUID 或 SGID 的文件
* 查找 /etc 目录下最近一周内修改过其内容且大小小于 2k 的文件
* 查找当前系统上没有属或属组，且最近一周内曾被访问过的文件或目录
* 查找 /etc 目录下所有用户都没有写权限的文件
* 查找 /etc 目录下至少有一类用户没有执行权限的文件
* 删除 /tmp/test 目录下 30 天前 60 天内修改的文件
* 删除 /tmp/test 目录下 30 天前修改的文件
* 复制 两周之前早 9:00 到两月之前 19:00 点之间修改的文件 到 /backup/1目录下


> **提示** 可以使用如下脚本在 /tmp/test 目录下先随机生成 200 个 100 天以内的文件，再使用 find 命令按时间戳查找和删除
```
#!/bin/bash
[ -d "/tmp/test" ] || mkdir /tmp/test
for d in $(shuf -i $(date +%s -d '-100 days')-$(date +%s) -n 200)
do 
   time=$(date -d "@$d" +%F)
   touch -d "$time" /tmp/test/file_$time
done
```

## 任务10：创建基于新硬盘上的卷组和逻辑卷

1. 为逻辑卷准备新的分区
  * 为虚拟机添加第3块 10G 大小的新硬盘（/dev/sdc）
  * 为磁盘设置 GPT 分区表
  * 使用全部空间添加 1 个分区，类型为 8e00
  * 在不重启系统的前提下，让 Linux 内核重新读取新硬盘的分区表
2. 管理LVM
  * 在新建的分区上创建物理卷
  * 基于此物理卷创建名为 db 的逻辑卷
  * 在 db 卷组中创建名为 mysql 的逻辑卷，大小使用全部FREE
3. 管理基于 LVM 上的文件系统
  * 对名为 mysql 的逻辑卷创建 ext4 文件系统
  * 创建挂装点目录 /var/lib/mysql
  * 修改 /etc/fstab 设置对此逻辑卷的启动时挂载
  * 重新挂在 /etc/fstab 中的文件系统，检查挂装情况    


## 任务11：扩展现有的卷组和逻辑卷

1. 为系统中已经存在的 home 逻辑卷准备新的分区
  * 在第2块硬盘（/dev/sdb）上，使用全部剩余空间添加 1 个分区，类型为 8e00
  * 重启系统，使得 Linux 内核重新读取硬盘的新分区表
2. 管理LVM
  * 在新建的分区上创建物理卷
  * 将此物理卷扩展到 home 逻辑卷所在的卷组中
  * 扩展名为 home 的逻辑卷，大小为 8G
3. 扩展逻辑卷上的 xfs 文件系统
4. 若安装 CentOS 时没有使用硬盘的所有空间，请将剩余的所有空间加到 / 文件系统所在的卷组中

## 任务12：缩减逻辑卷

1. 解除对 mysql 逻辑卷 的挂装
2. 强行检查 mysql 逻辑卷上的 ext4 文件系统
3. 将 mysql 逻辑卷缩减至原始大小的 50%
4. 缩减 mysql 逻辑卷上的 ext4 文件系统使之适应逻辑卷的新大小
5. 重新挂装 mysql 逻辑卷
6. 检查 mysql 逻辑卷上文件系统的大小

## 任务13*：逻辑卷快照

1. 在 mysql 逻辑卷的挂装点目录里创建一个测试文件，如：
        lvs > /var/lib/mysql/test
        cat /var/lib/mysql/test
2. 对 mysql 逻辑卷创建名为 mysql_snap 的只读快照卷，大小为 1G
3. 将快照卷 mysql_snap  挂装到 /lvmsnap/mysql 目录
4. 在 mysql 逻辑卷的挂装点目录里修改测试文件，如：
        (lvs ；vgs) > /var/lib/mysql/test
        cat /var/lib/mysql/test
5. 验正快照卷的功能，如
        cat /lvmsnap/mysql/test
6. 将快照卷里的内容备份到 /backup/mysql
7. 解除对快照卷的挂装
8. 删除快照卷 mysql_snap
9. 删除 mysql 卷上的测试数据


>#####参考
>* [Linux LVM 简明教程](https://linux.cn/article-3218-1.html)


## 任务14： 在 ext4 文件系统上配置用户的磁盘限额

** 要求：**

* 创建用户、组并设置其成员
  * 创建用户 fanny 和 ann
  * 创建组 webs 和 apps
  * 分别将 fanny 和 ann 加入组 webs 和 apps 
* 对挂装在 /srv 目录上的 ext4 文件系统设置用户配额
  * 创建 fanny 用户并设置其为 webs 组的成员
  * 为用户 fanny 设置容量软限制 400M、容量硬限制 500M 的块配额
  * 为用户 fanny 设置文件数软限制 2000、文件数硬限制 2500 的 inode 配额
  * 创建新用户 ann，并以 fanny 用户为参考用户设置其用户的磁盘配额
* 对挂装在 /srv 目录上的文件系统设置组配额
  * 为组 webs 设置容量软限制 1G、容量硬限制 2G 的块配额
  * 为组 webs 设置文件数软限制 20000、文件数硬限制 25000 的 inode 配额
  * 创建新组 apps，并以 webs 组为参考组设置 apps 组的磁盘配额
* 检查配额
  * 查看磁盘限额报告
  * 查看 fanny 的用户配额
  * 查看 apps 的组配额

## 任务15： 在 xfs 文件系统上配置用户的磁盘限额

** 要求：**


* 对挂装在 /data 目录上的 xfs 文件系统设置用户配额
  * 创建 fanny 用户并设置其为 webs 组的成员
  * 为用户 fanny 设置容量软限制 400M、容量硬限制 500M 的块配额
  * 为用户 fanny 设置文件数软限制 2000、文件数硬限制 2500 的 inode 配额
  * 创建新用户 ann，并以 fanny 用户为参考用户设置其用户的磁盘配额
* 对挂装在 /data 目录上的 xfs 文件系统设置组配额
  * 为组 webs 设置容量软限制 1G、容量硬限制 2G 的块配额
  * 为组 webs 设置文件数软限制 20000、文件数硬限制 25000 的 inode 配额
  * 创建新组 apps，并以 webs 组为参考组设置 apps 组的磁盘配额
* 检查配额
  * 查看磁盘限额报告
  * 查看 fanny 的用户配额
  * 查看 apps 的组配额

\newpage
# 实验指导 5 - 网络配置与包管理

>#### 学习目标
> * 网络配置
>   * 使用 `ip`/`ifconfig` 命令查看网络参数
>   * 使用 `nmcli/nmtui` 配置基于 NetworkManager 服务的网络连接
>   * 使用 `system-config-network-tui` 配置基于 network 服务的网络连接
>   * 直接修改网络配置文件配置网络连接 
>   * 禁用 一致的网络设备名
> * 网络工具
>   * 网络测试工具 `ping`、`ss`、`ip route`、`traceroute`、`dig`
>   * 网络客户工具 `wget`、`lftp`、`elinks` / `w3m`、`mail`
> * RPM 包管理
>   * 使用 `rpm` 命令 查询软件包
>   * 使用 `rpm` 命令 安装/卸载/升级/校验软件包
> * YUM 系统
>   * YUM 及其组件
>   * 使用 `yum` 命令 安装/卸载/升级/查询 软件包
>   * 使用 `rpm --import` 命令导入 YUM 仓库的 GPG 密钥签名
>   * 修改仓库配置文件配置远程 YUM 仓库（包括非官方仓库）
>   * 使用安装光盘配置本地 YUM 仓库


## 任务1：使用网络基本命令

* 显示所有设备的 IP 地址
* 显示所有连接的流量信息 
* 显示本地主机名
* 显示与本机连接的公网网关上的IP 
  * curl/elinks --dump
  * http://whatismyip.org
* 检测与 114.114.114.114 的连通性
* 检测与 8.8.8.8 的连通性（只发1次ICMP包，且等待1秒的响应时间）
* 显示本机路由表
* 检测 www.baidu.com 的 DNS 解析
* 指定使用 8.8.8.8 检测对 baidu.com 域 的 DNS 解析
* 显示到 www.baidu.com 的路由跟踪信息
* 显示所有网络套接字的连接
* 显示所有 tcp 类型网络套接字的连接
* 显示本机监听的所有 tcp 类型网络套接字的连接
* 给本机上的 tony 用户发送一个测试邮件，主题为 “test”，内容为 “hello”

>**参考**
>
>* https://www.linuxtrainingacademy.com/linux-ip-command-networking-cheat-sheet/
>* https://www.linuxtrainingacademy.com/determine-public-ip-address-command-line-curl/
>* [Common administrative commands in Red Hat Enterprise Linux 5, 6, and 7](https://access.redhat.com/articles/1189123)

## 任务2：配置网络连接

* 从宿主机（Windows/Mac/Linux）远程登录 CentOS 7 的虚拟机
  * 使用 VirtualBox 中配置的 Host-Only 网卡的配置连接
  * 如： `ssh root@192.168.56.71`
* 修改 VirtualBox 中配置的 NAT 网卡上配置的网络连接  
  * 这相当于你在机房的局域网里配置服务器的能与公网连通的 IP 地址
  * 查看 NAT 网卡对应的网络参数
  * 将当前 IP 地址值加10，其他参数均不变，使用 `nmcli` 为此连接重新设置静态网络参数
  * 重新激活，并检测修改
  * 检测与公网的连通性，并检测域名解析是否正常
  * 重新配置为动态获得网络参数

## 任务3：YUM 仓库管理

* 配置国内镜像（方法一）
  * 为官方仓库配置使用 163 的镜像
  * 参考 http://mirrors.163.com/.help/centos.html
* 安装配置 EPEL 仓库
  * 查询本机是否安装了 epel 仓库发行包
  * 搜索 epel 仓库发行包的包名称
  * 安装 EPEL 仓库
  * 导入 YUM 仓库的 GPG 密钥签名的公钥
        rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
* 配置国内镜像（方法二）
  * 用 `sed` 命令修改 epel 仓库配置文件配置使用清华大学的镜像地址
  * 清华大学开源镜像网址：http://mirrors.tuna.tsinghua.edu.cn
* 显示当前可用的仓库
* 为每个仓库在本地创建仓库元数据缓存

>**提示** 使用 `yum-utils` 包中提供的 `yum-config-manager` 命令可以管理 yum 的配置选项和仓库配置文件（如：启用/禁用指定的仓库、从一个URL配置仓库配置文件等）

## 任务4：安装 LXC 

>* LXC 相关的 RPM 包在 EPEL 仓库中，请先配置 EPEL 仓库

```
yum -y install debootstrap perl libvirt
systemctl is-enabled libvirtd
systemctl is-active libvirtd
systemctl start libvirtd
ip a # 显示 libvirt 创建的网桥设备


yum search lxc
yum -y install lxc lxc-templates
systemctl start lxc.service
lxc-checkconfig

yum -y install lxc-extra
echo "alias lxc-ls='lxc-ls --fancy'" >> ~/.bashrc
. ~/.bashrc
lxc-ls
```

## 任务5：创建和使用 LXC 容器

1、 通过不同发行模版创建容器

```
ll /usr/share/lxc/templates/

lxc-create -n c7-v1 -t centos
lxc-ls

## 在容器启动前，以 chroot 方式修改口令 
chroot /var/lib/lxc/c7-v1/rootfs passwd

## 启动指定的容器，-d 表示后台
lxc-start -d -n c7-v1 
## 在制定容器中直接执行命令
lxc-attach -n c7-v1 -- passwd  # 在容器启动后设置口令
lxc-attach -n c7-v1 -- yum install openssh-server
lxc-attach -n c7-v1 -- systemctl start sshd
lxc-ls
ssh [<User>@]<IP>

## 关闭指定容器
lxc-stop -n c7-v1

## 创建最新版的 debian 容器
#lxc-create -n d9-v1 -t debian
## 删除容器
#lxc-destroy -n d9-v1
```

2、 下载模版并创建容器

```
## 0. 显示可用的镜像模版
/usr/share/lxc/templates/lxc-download -l

## 1. 从下载的镜像文件创建 CentOS6 容器
lxc-create -n c6-v1 -t download -- -d centos -r 6 -a amd64

## 后台启动容器  c6-v1
lxc-start -d -n c6-v1 
## 在容器启动后，以 lxc-attach 方式修改口令
lxc-attach -n c6-v1 -- passwd
## 如果创建容器时配置了 tty，可通过如下命令连接到 tty 
## 还可以加 -t <n> 表示指定使用 ttyn 终端，默认为 tty1
lxc-console -n c6-v1 
## 登录后在容器中执行命令
yum install openssh-server
service sshd start
exit
#### 提示： <Ctrl-a> q 退出，返回到容器的宿主机

lxc-ls
ssh ...

## 2. 从下载的镜像文件创建 Debian 9 容器
lxc-create -n d9-v1 -t download -- -d debian -r stretch -a amd64
#lxc-create -n d8-v1 -t download -- -d debian -r jessie -a amd64

## 为 d9-v1 容器修改 PATH 环境变量
chroot /var/lib/lxc/d9-v1/rootfs
/bin/sed -i '$ i\export PATH=/bin:/sbin:$PATH' /root/.profile
source /root/.profile &> /dev/null
echo $PATH
exit

## 后台启动容器  d9-v1
lxc-start -d -n d9-v1

lxc-attach -n d9-v1 -- passwd
lxc-attach -n d9-v1 -- apt install openssh-server vim
lxc-attach -n d9-v1 -- vi ~/.bashrc
lxc-attach -n d9-v1 -- systemctl is-enabled sshd
lxc-attach -n d9-v1 -- systemctl is-active sshd
lxc-attach -n d9-v1 -- systemctl start sshd
lxc-ls
ssh ...

## 关闭指定容器
lxc-stop -n d9-v1
```

>**参考**
>
>* https://www.tecmint.com/install-create-run-lxc-linux-containers-on-centos/
>* http://wiki.libvirt.org/page/VirtualNetworking

## 任务6：rpm 命令

* 查询系统中安装的所有RPM软件包
* 查询系统中是否安装了名为 lxc 包
* 若安装了名为 `lxc` 包，在屏幕上显示 yes，否则显示 no
* 查找 /etc/shadow 文件是由哪个包安装的
* 查询 `libvirt` 包的发行信息 
* 查询 `libvirt-daemon` 包内置的安装前/后配置脚本
* 查询 `lxc` 包安装的所有文件
* 查询 `lxc` 包的依赖要求
* 校验 `lxc` 软件包


## 任务7：yum 命令

* 安装 zsh 软件包
* 使用 wget 下载最新的  usermin
  * http://www.webmin.com/download/rpm/usermin-current.rpm
* 安装下载到本地的 `usermin-current`.rpm
* 查询 `lxc` 包的发行信息
* 查询 `lxc` 包的依赖要求
* 当 EPEL 仓库未启用时，查询 lxc 包的依赖要求
* 查询什么软件包里提供了 `ifconfig` 和 `pstree`
* 查询所有的包名包含 httpd 的包
* 查询所有包组信息
* 删除 `usermin` 软件包
* 通过 YUM 事务历史删除已安装的 `zsh`

## 任务8：debian 包管理

* 请在 debian 9 的容器中学习
  * 与 rpm 功能对应的 `dpkg` 的使用
  * 与 yum 功能对应的 `apt`/`apt-get`  
  * 与 `rpm --import` 功能对应的 `apt-key add`

> **参考**
>
> * [Using apt-get Commands In Linux [Complete Beginners Guide]](https://itsfoss.com/apt-get-linux-guide/)
> * [Using apt Commands in Linux [Complete Guide]](https://itsfoss.com/apt-command-guide/)
> * [Difference Between apt and apt-get Explained](https://itsfoss.com/apt-vs-apt-get-difference/)


## 任务9：配置 CentOS 6 的网络连接

>### 关于网络服务
>
>* 静态
>  * network - CentOS 系列
>  * networking - Debian 系列
>* 动态
>  * NetworkManager 

---

>**提示** 
>* 网络服务： `network` 
>* 网络接口配置文件 `/etc/sysconfig/network-scripts/ifcfg-eth0` 
>* 主机名设置在 `/etc/sysconfig/network` 文件中
>* DNS 解析配置文件 `/etc/resolv.conf`
>* 配置的静态 IP 地址的网段应与 libvirt 创建的虚拟网桥一致

1、通过 lxc-console 或 ssh 登录 c6-v1 容器

    [root@c6-v1 ~]#

2、修改接口位置文件  `/etc/sysconfig/network-scripts/ifcfg-eth0` 

    DEVICE=eth0
    #BOOTPROTO=dhcp
    BOOTPROTO=none
    ONBOOT=yes
    HOSTNAME=c6-v1
    NM_CONTROLLED=no
    TYPE=Ethernet
    MTU=
    #DHCP_HOSTNAME=`hostname`
    IPADDR=192.168.122.161
    NETMASK=255.255.255.0
    GATEWAY=192.168.122.1

3、修改 DNS 解析文件 `/etc/resolv.conf`

    [root@c6-v1 ~]# echo "nameserver 192.168.122.1" > /etc/resolv.conf

4、确保 network 网络服务在开机时启动

    [root@c6-v1 ~]# chkconfig network on

5、退出容器登录

    [root@c6-v1 ~]# exit

   若使用 lxc-console 登入容器，则按 `<Ctrl-a> <q>` 返回宿主机的 shell

6、重新启动 c6-v1 容器的网络服务

    [root@host ~]# lxc-attach -n c6-v1 -- /sbin/service network restart

7、重新显示容器信息

    [root@host ~]# lxc-ls


>**参考**
>* [Common administrative commands in Red Hat Enterprise Linux 5, 6, and 7](https://access.redhat.com/articles/1189123)


## 任务10：配置 Debian 9 的网络连接

>**提示** 
>* 网络服务：`networking`
>* 网络接口配置文件 `/etc/network/interfaces` 
>* 主机名设置在 `/etc/hostname` 文件中
>* DNS 解析配置文件 `/etc/resolv.conf`
>* 设置的静态 IP 地址的网段应与 libvirt 创建的虚拟网桥一致


1、使用 chroot 命令切换到 d9-v1 容器的 root 文件系统

    [root@host ~]# chroot /var/lib/lxc/d9-v1/rootfs
    [root@host /]#

2、修改接口位置文件  `/etc/network/interfaces` 

```
[root@host /]# /bin/cat > /etc/network/interfaces <<_END
auto lo
  iface lo inet loopback

auto eth0
#iface eth0 inet dhcp
iface eth0 inet static
  address 192.168.122.191
  netmask 255.255.255.0
  gateway 192.168.122.1
_END

```

3、修改 DNS 解析文件 `/etc/resolv.conf`

    [root@host /]# echo "nameserver 192.168.122.1" > /etc/resolv.conf

4、确保 networking 网络服务在开机时启动

    [root@host /]# /bin/systemctl enable networking

5、退出容器 chroot

    [root@host /]# exit

6、启动容器 d9-v1

    [root@host ~]# lxc-start -d -n d9-v1

7、重新显示容器信息

    [root@host ~]# lxc-ls


>**参考**
>* https://www.debian.org/doc/
>  * https://www.debian.org/doc/manuals/debian-reference/ch05.zh-cn.html


\newpage
# 实验指导 6 - 服务管理与基础服务

>#### 学习目标
> * 服务管理
>   * 使用 `systemctl` 显示、启动和停止服务
>   * 使用 `systemctl` 实现服务的持久化管理
>   * 使用 `service` 和 `chkconfig/update-rc.d` 管理服务
> * cron 服务
>   * 修改配置文件安排 cron 任务
>   * 使用 `crontab` 命令安排 cron 任务
>   * 控制使用 cron 的人员
> * rsyslogd 和 systemd-journal 服务
>   * 显示系统日志
>   * 配置中央日志服务器
>   * 查看 `LogWatch` 的日志统计信息
> * sshd 服务
>   * 配置仅支持用户密钥登录的 SSH 服务器
>   * 配置支持 sftp+chroot 的 SFTP 服务器
>   * 使用 `ssh-keyscan` 命令搜集可信任主机公钥
>   * 用户密钥管理
>   * 使用 SSH 隧道（Tunnel）实现端口转发（Port forward）


## 任务1：守护进程（服务）管理

* 显示当前已运行的所有服务
* 查看所有服务是否在启动系统时启用
* 显示 cron 服务是否正在运行
* 显示 cron 服务是否在启动时运行
* 显示 cron 服务的运行状态
* 关闭正在运行的 cron 服务
* 显示 cron 服务的运行状态
* 启动 cron 服务
* 禁止 cron 服务在启动时运行
* 启用 cron 服务在启动时运行

## 任务2：修改配置文件安排 cron 任务

>* 请在 /etc/cron.d/test 文件中完成下列任务
>* **提问** 此文件修改后需要重新 restart/reload cron 服务吗？

* 每小时第1分钟将系统平均负载信息以追加方式写入 /root/load 文件
* 每阁3小时的30分时将磁盘剩余信息以追加方式写入 /root/du 文件
* 每周1-5晚10点将 /data/work 目录中的内容向 /backup/data/work 同步一次
* 每月1日和15日凌晨1:30重新启动一次 httpd 服务
* 每天凌晨 0:30 找出 /backup 目录下 90 天以前修改过的文件并将其删除

## 任务3：使用 `crontab` 命令安排 cron 任务

* 切换 student 用户身份，完成如下任务
  * 每天 7：00 找出 ~/student 目录下尺寸最大的 10 个文件列表写入 ~/student/maxsize 文件  
  * 每隔 5 天凌晨的 2 点删除 ~/student/temp 目录下的所有文件
  * 每周日凌晨的 2 点将 ~/student/data 同步到 ~/student/bak/data
* 配置仅允许 student 和 tony 用户安排 cron 任务

## 任务4：显示日志信息

* 浏览 /var/log
  * 使用文本工具查看 /var/log/{secure,message} 文件
  * 检查用户上次登录的时间
* 使用 logwatch
  * 在屏幕上打印昨天的日志信息简要，如用户登录失败信息、SSH 登录信息、磁盘空间使用等。
  * 在屏幕上打印今天的 SSH 登录信息。
* 使用 journalctl
  * 显示journal记录的所有日志
  * 显示自昨天以来记录的日志（类似地可以使用today表示今天）
  * 动态跟踪显示最新日志信息
  * 显示日志级别为err的日志
  * 显示内核日志
  * 显示最近一次的启动日志
  * 显示上次启动时的错误日志
  * 显示systemd指定单元的日志
  * 显示进程名为sshd的相关日志
  * 显示指定时间之内的进程名sudo的日志

>**提问** 如何配置 systemd-journal 服务实现持久化存储（即将其存入磁盘而非 tmpfs） 

## 任务5：配置日志服务器

  * 配置将容器 c7-v1 上的所有 rsyslog 日志记入 宿主服务器 
  * 配置将容器 c6-v1 上的所有 rsyslog 日志记入 宿主服务器
  * 配置将容器 d9-v1 上的所有 rsyslog 日志记入 宿主服务器

## 任务6：SSH 密钥管理

* 在容器的宿主服务器上完成如下操作
  * 收集 c7-v1、c6-v1、d9-v1 的主机公钥
  * 以 root 身份创建密钥对（无私钥口令）
  * 将此公钥分发给容器 c7-v1、c6-v1、d9-v1
  * 分别 ssh 登录容器 c7-v1、c6-v1、d9-v1 进行连接测试
  * 为私钥设置保护口令
  * 使用 ssh-agent、ssh-add 导入私钥
  * 分别 ssh 登录 c7-v1、c6-v1、d9-v1 进行连接测试
* 在安装 VirualBox 的 Windows 宿主机上使用 Putty 携带的工具完成
  * 使用 PuTTYgen 生成密钥对（设置私钥口令）
  * 将私钥保存到 \User\YourName\ 的任意子目录下
  * 打开 Putty 的会话窗口，载入连接 CentOS 7 VirtualBox 虚拟机的回话，设置私钥文件位置
  * 运行 Pageant 导入以创建的私钥
  * 使用 Putty 通过密钥登录 CentOS 7 VirtualBox

>**提示** 若你偏爱命令行工具，不喜欢图形工具（如：Putty），安装 Git for Windows 即可，她集成了 openssh-client。

## 任务7：跨主机的文件复制

* 使用 scp/sftp
  * 使用 scp/sftp 在 CentOS 7 VirtualBox 虚拟机和容器 c7-v1、c6-v1、d9-v1 之间互传文件 
* 使用 Windows 下的 GUI 工具在 WIndows 和 CentOS 7 VirtualBox 虚拟机之间互传文件
  * 使用 [WinSCP](https://winscp.net/) 实现 sftp
  * 使用 [FreeFileSync](https://www.freefilesync.org/) 实现基于 sftp 的目录同步

> **参考**
>
> * [使用 rz 和 sz 命令在 Windows 和 Linux 之间传输小文件](https://blog.csdn.net/jack85986370/article/details/51321475)

## 任务8：配置 chroot 的 SFTP 服务

* 配置 sftp-grp 组的成员登录 SFTP 服务器后被监禁在 /home 目录下

>**参考**
>
>* [SFTP only + Chroot](https://www.server-world.info/en/note?os=CentOS_7&p=ssh&f=5) 

## 任务9：配置 SSH 服务

* 设置所有用户必须以密钥方式登录
* 执行 .ssh 目录及文件的严格权限检查
* 设置 ssh 空闲回话时间为5分钟，超过时自动注销
* 关闭 DNS 反向解析功能
* 除了 root 只有 student 和 tony 才能使用 ssh/scp/sftp
* 重新启动 sshd 服务

## 任务10：配置 SSH 隧道

>**基于 WUI 的系统管理工具**
>* Webmin -- http://www.webmin.com
>* Cockpit -- http://cockpit-project.org
>* Ajenti -- http://ajenti.org/

* 在 Linux 上安装、配置并启动 webmin 
  * 安装：参考 http://www.webmin.com/rpm.html 
  * 启动：`/etc/init.d/webmin start`
* 在 Windows 是上配置 SSH 隧道
  * 对本机上的 10000 端口和 CentOS 7 VirtualBox 虚拟机上的 10000 端口建立 ssh 隧道 
    * 安装并使用 **[MyEntunnel](https://myentunnel.informer.com/)** -- `plink` 命令行工具的 GUI 前端
    * 若您已配置了基于 Putty 的密钥登录，可在 Putty 的回话配置界面中设置，也可以使用 `plink` 命令行工具实现
    * 若您已配置了基于 openssh-client 的密钥登录，请使用 `ssh` 命令行工具实现
* 在 Windows 上的浏览器中使用 URL https://localhost:10000 访问 Webmin

>**参考**
>* [三种不同类型的 SSH 隧道](http://hetaoo.iteye.com/blog/2299123) 
>* [通过 SSH 隧道连接远程 MySQL 服务](http://blog.csdn.net/fgf00/article/details/51284335)
>* [如何在VPS上设置SSH隧道](https://www.howtoing.com/how-to-set-up-ssh-tunneling-on-a-vps)


## 任务11：服务管理 （续）

* 在 c6-v1 容器上使用 `service` 和 `chkconfig` 管理服务

\newpage
# 实验指导 7 - 系统维护

>#### 学习目标
> * 使用系统监视工具
>   * `uptime`/`tload`
>   * `top`/`htop`/`dstat`
>   * `free`/`vmstat`
>   * `mpstat`/`iostat`/`sar`
>   * `nload`/`nethogs`/`iftop`
> * 内核管理
>   * 安全升级内核  
>   * 使用 `sysctl` 调整内核参数
> * 系统启动过程
>   * Systemd 的目标管理与运行级别
>   * 使用 `systemd-analyze` 分析启动过程性能
>   * 自定义 Systemd 服务单元的配置文件
> * 备份与恢复
>   * 使用 `cp`、`tar`、`dd`、`rsync` 等命令实施备份
>   * 使用 `rsnapshot` 工具实现快照型备份
>   * 使用 `lsyncd` 实现实时同步    
> * 故障排查
>  * 切换进入 Systemd 的 `rescue`/`emergency` 目标
>  * 使用 安装光盘的援救环境/LiveCD 修复系统启动故障和其他故障
>  * 使用 `grub2-mkconfig` 工具重新生成 GRUB2 的配置文件
>  * 为内核传递 `rd.break` 参数中断 systemd 的执行并修复故障
>  * 修复 root 口令丢失、分区/LVM、文件系统、软件包、网络等故障


## 任务1： 使用监视工具分析系统性能

>**提示** 请先确保常用的系统监视工具已经被安装
> `yum -y install sysstat htop dstat nload nethogs iftop`

* 使用如下工具监视系统
  * `uptime`/`tload` 
  * `top`/`htop`/`dstat`
  * `free`/`vmstat`
  * `mpstat`/`iostat`/`sar`
  * `nload`/`nethogs`/`iftop`
* 系统性能分析
  * 系统平均负载为何时表示负载过重？
  * 如何找出 CPU 瓶颈？
  * 如何找出 内存 瓶颈？
  * 如何找出 I/O  瓶颈？
  * 如何找出 带宽 瓶颈？

## 任务2： 内核管理

* 升级内核
  * yum update kernel-VER 
  * yum update
* 调整内核参数
  * 使用 `sysctl` 显示内核参数
  * 使用 `sysctl -w` 设置内核参数
    * 如：开启包转发功能 


## 任务3：系统启动过程

>### BIOS/UEFI -> GRUB2 -> 内核初始化 -> Systemd 初始化

* 内核初始化
  * 参考 `man 7 dracut.bootup` 
* Systemd 初始化
  * 参考 `man 7 bootup`
* systemd 的运行目标管理
  *  显示当前已激活的目标
  *  显示当前已加载的所有目标
  *  显示当前已安装的所有目标
* systemd 的默认目标管理
  * 类似于在 CentOS 6 中配置或切换系统运行级别 
  * 显示当前的默认目标
  * 显示默认目标的依赖关系
  * 设置默认目标（下次启动时生效）为 `graphical.target`
  * 更改当前的目标（立即生效）到 `emergency.target`


## 任务4： 禁用一致的网络设备名

* 修改 `/etc/default/grub` 的 `GRUB_CMDLINE_LINUX` 配置行
* 重新生成 grub2 的配置文件
* 重新启动

## 任务5： 基于 tar 的备份

* 完全（Full）备份
  * 将 `/etc /home /srv` 目录下的所有文件打包备份到 `/backup/full-backup.tgz`
  * 将当前完全备份的日期时间戳 (`date +%F`)写入 `/backups/full-backup-date` 文件
* 增量（Incremental）备份
  * 将自昨天 0 点起有变化的数据打包备份到 `/backup/inc-backup-$(date -d yesterday "+%F").txz`
* 差分（Differential）备份
  * 将自完全备份以来有变化的数据打包备份到 `/backup/diff-backup-$(date +%F).tbz`

>* 编写一个每日执行脚本，实现每月一次完全备份，每周一次差分备份，每日一次增量备份，并删除 180 天前创建的备份文件

## 任务6： 基于 rsync 和 tar 的远程备份

**要求** 
1. 备份生产服务器（容器 c6-v1）的 /etc /home 目录到备份服务器（假如使用在第一章安装的 CentOS 7 VirtualBox 虚拟机）
2. 保留历史归档 （每月一次完全备份，每周一次差分备份，每日一次增量备份）
3. 删除 180 天前创建的备份文件

**方法**
1. 在容器 c6-v1 上执行任务5的操作，然后使用 rsync 命令同步到备份服务器
2. 优选方法
   * 在备份服务器上创建目录 /backup/c6-v1/{current,archive} 
   * 在备份服务器上每天将 c6-v1 上的 /etc /home 同步到自己的 /backup/c6-v1/current/{etc,home}
   * 在备份服务器上针对 /backup/c6-v1/current/{etc,home} 里的数据实施如 任务5 的操作，并将生成的 tar 文件存到 /backup/c6-v1/archive 目录里
   * 这样可以减轻生产服务器的负载并极大地减少了网络传输

试编写一个脚本完成上述任务。


## 任务7： 基于快照工具（rsnapshot）的备份

**要求：**备份 `/etc/ /home /srv /root` 目录下的所有内容

* 修改 rsnapshot 的配置文件 `/etc/rsnapshot.conf`
  * 假设这些目录都在某个逻辑卷中 
    * 在备份之前，对要备份的目录所在的逻辑卷创建快照
    * 将快照卷挂装到 `/mnt/snap/` 相应的子目录下
    * 备份完毕卸装快照卷并删除快照卷
  * 要求保留 6个小时快照，7 个日快照，4个周快照，6个月快照
* 添加周期性任务配置文件 `/etc/cron.d/rsnapshot` ，配置使之符合备份频率要求

## 任务8： 实时同步

* 安装并配置 `lsyncd` 
* 使 CentOS 7 VirtualBox 虚拟机 的 `/var/www` 目录与 容器 c6-v1 中的 `/var/www` 目录保持同步

## 任务9： 故障排查

* 找回丢失的 root 口令
* 使用 gpt 分区设备的备份分区表修复其主分区表
* 使用通过 dd 命令备份的 DOS（MBR） 分区表镜像文件修复 DOS 分区表
\newpage
# 实验指导 8 - 服务器安全

>#### 学习目标
> * 系统安全
>   * 设置 GRUB 的修改口令
>   * 禁用 `<Ctrl>`+`<Alt>`+`<Delete>` 重启
>   * 设置超时自动注销
>   * 限制 root 用户 bash 命令历史的记录条目数量
>   * 配置 `sudo` 并禁止 root 用户登录
>   * 使用 `rkhunter` 工具实现 rootkit 检查
>   * 使用 `aide` 工具实现重要文件的完整性检查
>   * 配置 root 用户仅能使用用户密钥而非用户口令登录 SSH 服务
>   * 将 SSH 服务运行于非标准端口 
> * 软件安全
>   * `yum update` 与 `yum upgrade` 的区别 
>   * `yum-cron` 获取每日安全更新通知或直接进行安全更新
> * 口令安全
>   * 使用 `pam_unix.so` 模块限制用户使用旧口令
>   * 使用 `pam_pwquality.so` 模块实现口令复杂性检查
>   * 使用 `pam_tally2.so` 模块实现登录失败的账户锁定  
>   * 口令时效
> * 访问控制
>   * 设置用户或组使用系统资源（文件打开数量） 
>   * 配置仅 wheel 组成员可以使用 su 和 sudo 命令
>   * 使用 `TCP Wrappers` 实现基于 主机/网络 的访问控制
>   * 使用 `fail2ban`/`denyhost` 工具保护 SSH 服务
> * 信息与数据安全
>   * 使用对称加密算法加解密文件
>   * 校验文件的 GPG 数字签名
>   * 创建 SSL/TLS 自签名证书
> * 漏洞检测
>   * 使用 `lynis` 安全审计工具检测系统漏洞
>   * 根据 `lynis` 报告提示加固系统

## 任务1：物理安全

* 设置 GRUB 的修改口令
* 禁用 `<Ctrl>`+`<Alt>`+`<Delete>` 重启

## 任务2：登录安全

* 设置 bash 超时自动注销
* 限制 root 用户 bash 命令历史的记录条目数量为 10
* 配置仅 tony 用户可以使用 sudo 实现完全功能访问
* 禁止 root 用户以口令方式登录（口令锁定）
* 用户 root 的 SSH 登录策略 
  * 禁止 root 用户登录 SSH 服务
  * 配置 root 仅能使用用户密钥而非用户口令登录 SSH 服务

## 任务3：软件安全

* 安装 `yum-cron` 
* 配置 `yum-cron`
  * 获取每日安全更新通知
  * 发送到你自己的邮箱

## 任务4： 服务安全

* 关闭无必要 `vfstpd` 服务 
* 关闭无必要 `nfs` 服务 
* 关闭 `NetworkManager` 服务
* 关闭无必要 `ntpd` 和 `chronyd` 服务
  * 使用 `ntpdate` 命令安排每日周期任务进行时间同步

>**将不同的 服务/应用 分散部署在不同的 服务器/虚拟机/容器**

## 任务5：口令安全

* 记住最近使用的5个口令，在设置新口令时不能使用
* 口令质量检查 （/etc/security/pwquality.conf）
  * 口令必须大于12个字符
  * 必须至少包含四类字符中的一个
* 口令时效
  * 对新创建用户设置 90 天后必须重新修改口令
  * 对现存的 tony 用户设置 90 天后必须重新修改口令
* 账户锁定
  * 5 次登录失败进行登录锁定
  * 20 分钟后自动解锁  

## 任务6：访问控制

* 为 apache 用户设置打开文件数的限制为 65535
* 配置仅 wheel 组成员可以使用 su 和 sudo 命令
* 安装 `denyhosts` 工具基于 `TCP Wrappers` 实现对 SSH 服务的主机访问控住保护

## 任务7：文件加密与解密

* 显示 openssl 支持的所有兑成加密算法
* 使用 aes128 加密算法对 /root/anaconda-ks.cfg 文件加密
  * 加密后的文件为  /root/anaconda-ks.cfg.mi
* 对 /root/anaconda-ks.cfg.mi 文件进行解密 
  * 加密后的文件为  /root/anaconda-ks.cfg.new
* 使用 `diff` 命令比对解密文件与源文件

## 任务8：校验 RPM 包的数字签名

* 查询已导入 RPM 数据库的所有 GPG 公钥信息
  * 要求显示 summary，version 和 release 信息 
* 使用 wget 下载最新的  usermin RPM包，以及其 GPG 签名的公钥
  * http://www.webmin.com/download/rpm/usermin-current.rpm
  * http://www.webmin.com/jcameron-key.asc
* 将此 GPG 签名的公钥文件 `jcameron-key.asc` 导入 RPM 数据库
* 再次查询已导入 RPM 数据库的所有 GPG 公钥信息
* 验证 usermin-current.rpm 文件的 GPG 签名


## 任务9：校验文件的数字签名

* 校验 sha256sum.txt.asc 文件的 GPG 签名从而确认此文件确实是 CentOS 发布的
      wget http://mirrors.163.com/centos/7/isos/x86_64/sha256sum.txt.asc
* 下载 CentOS 的 GPG 公钥文件
      wget http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-7
* 显示 GPG 公钥文件的密钥指纹
      gpg --quiet --with-fingerprint ./RPM-GPG-KEY-CentOS-7
* 将 GPG 公钥导入本机的 GPG 公钥链
      gpg --import ./RPM-GPG-KEY-CentOS-7 
* 校验 sha256sum.txt.asc 的 GPG 签名 
      gpg --verify ./sha256sum.txt.asc

* 校验你下载的 CentOS/LinuxMint 发行版的 ISO 文件及其 GPG 数字签名
>   * CentOS - 参考 https://wiki.centos.org/Download/Verify
>   * LinuxMint - 参考 ttp://linuxmint-installation-guide.readthedocs.io/en/latest/verify.html

> **提示** 若 ISO 文件下载在 Windows 上，可以使用 git-for-windows 中集成的 gpg 工具完成

## 任务10：自签名 SSL/TLS 证书

* 创建单域名自签名证书文件及其私钥
  * 证书文件 ftp.olabs.lan.crt
  * 私钥文件 ftp.olabs.lan.key
* 创建多域名自签名证书文件及其私钥文件
  * 证书文件 myservers.crt
  * 私钥文件 myservers.key
  * 多域名 
    * olabs.lan www.olabs.lan wiki.olabs.lan 
    * olabs.net www.olabs.net wiki.olabs.net 
    * olabs.org www.olabs.org wiki.olabs.org 

## 任务11*：由本地 CA 签署证书模拟证书签署过程

* 在 Windows 上创建本地 CA
  * 安装基于 OpenSSL 的前端开源 GUI 工具 [xca](https://sourceforge.net/projects/xca/) 
  * 创建 CA 的证书和私钥
* 在 Linux 上生成证书签名请求文件
  * ftp.olabs.lan.csr
  * myservers.csr 
* 在 Windows 上使用本地 CA 签署证书
  * 将 Linux 上生成的 CSR 文件共享给 Windows （如 scp） 
  * 使用本地 CA 签署 CSR 文件并生成 CRT 证书文件
  * 将生成的 CRT 证书文件共享给 Linux （如 scp） 


## 任务12*：RKHunter ： Check Rootkit

* 安装 RKHunter
* 配置文件：`/etc/rkhunter.conf`
* 检查系统上的 Rootkit

>**参考** [RKHunter : Detect Rootkit](https://www.server-world.info/en/note?os=CentOS_7&p=rkhunter)

## 任务13*： AIDE ： Host based IDS

* 安装 AIDE
* 配置文件：`/etc/aide.conf`
* 生成检测数据库
* 检查系统上的重要文件变化，实现基于主机的入侵检测
* 确认更改安全后重新生成检测数据库

>**参考** [AIDE: Host based IDS](https://www.server-world.info/en/note?os=CentOS_7&p=aide)

## 任务14： 漏洞检测

* 安装 `lynis` 
* 首次执行 `lynis audit system`
* 使用 `lynis -c` 扫描整个系统
* 根据 `lynis` 报告提示加固系统

>**参考**
>* https://cisofy.com/documentation/lynis/get-started/
>* `man lynis`
\newpage
# 实验指导 9.1 - Linux 防火墙

>#### 学习目标
> * 使用 `firewall-cmd` 或 `firewall-config` 工具配置基于 firewalld 守护进程的防火墙
> * 使用 `lokkit` 或`system-config-firewall-tui` 工具配置基于 iptables 服务的防火墙
> * 编写基于 `iptabls` 命令的防火墙脚本
> * 编写基于 `iptables-save` 命令输出文件格式的防火墙脚本


## 任务1：firewalld 防火墙 -- 主机防火墙（单网卡）

* 安装并启动 firewalld
* 使用 `firewall-cmd` 命令完成如下操作 
  * 显示当前的默认区域
  * 显示当前已经激活的区域
  * 显示默认区域的所有规则
  * 显示默认区域内允许访问的所有服务
  * 显示默认区域内允许访问的所有端口
  * 显示 firewalld  预定义的服务（名称）
  * 允许访问 http、https 、syslog 服务
  * 允许访问 81 （TCP）端口 
  * 禁止访问 syslog 服务
  * 仅允许 192.168.56.30 的主机访问 syslog 服务
  * 禁止 192.168.56.30 的主机访问 http 服务
  * 使上述配置持久化（下次启动 firewalld 时生效）
* 在适当的客户端进行测试

>**提问** 下面两条命令的作用
>* `firewall-cmd --get-services | grep -wo http`
>* `firewall-cmd --get-services | tr ' ' '\n' | grep  http`


## 任务2：基于 2222 端口的 SSH 服务的 firewalld  防火墙

1. 配置 SSH 服务
  * 设置端口为 2222
  * 重新加载配置
2. 自定义 firewalld 的规则配置文件
  * 复制默认的 SSH 规则文件到 `/etc/firewalld/services/`
  * 修改复制后的 `/etc/firewalld/services/ssh.xml`， 将 22 端口改为 2222
  * 重新加载防火墙配置  
3. 配置 fail2ban
  * 安装 fail2ban
  * 创建本地配置文件 `/etc/fail2ban/jail.local`
          [DEFAULT]
          findtime = 900
          [sshd]
          enabled = true
          bantime = 3600
          maxretry = 5
          port=2222
  * 设置 fail2ban 立即/开机 启动


## 任务3：SSH X11 Forwarding

* 服务器端
  * 安装 `xauth`
  * 配置 SSH 服务器开启 `X11Forwarding`
  * 重新加载 sshd 配置文件
  * 安装 `firewall-config`
* 客户端
  * 安装 Xming
  * 配置 Putty 中的 ssh 会话启用 `X11Forwarding`
  * 连接已配置的 ssh 会话
  * 执行 GUI 工具 `firewall-config` 配置防火墙

>* [Xming](https://xming.en.softonic.com/)
>* [PuTTY+Xming 实现 X11 的 ssh 转发](http://blog.csdn.net/smstong/article/details/46328247)

## 任务4：firewalld 防火墙 -- 网关防火墙（多网卡）

>**配置环境**
>* 防火墙主机（CentOS 7 VirtualBox 虚拟机）有 2 块物理网卡
>  * NAT 类型网卡 （自动获得网络参数）
>  * host-only 网卡 （192.168.56.71/24） 
>  * libVirtd 虚拟网桥 （192.168.122.1/24）
>    * c7-v1 容器 （192.168.122.71/24）
>* 服务器主机（CentOS 7 VirtualBox 虚拟机）有 1 块网卡
>  * host-only 网卡 （192.168.56.30/24） 
>* Window 宿主机 
>  * host-only 虚拟网桥 （192.168.56.1/24） 
>  * 能连接公网 （有线/无线）

* 配置防火墙主机 
  * 显示 firewalld 预定义的区域
  * 显示 firewalld 预定义的区域的所有规则
  * 仅显示 `work` 区域的所有规则
  * 显示当前已经激活的区域
  * 显示默认区域的所有规则
  * 将默认区域设置为 `work`
  * 将 VirtualBox 虚拟机中设置为 NAT 的网卡对应的设备设置为 `external` 区域
  * 绑定源地址 192.168.122.0/24 到 `public` 区域
  * 显示 `public` 区域的所有规则
  * 重新显示当前已经激活的区域
  * 允许访问  `external` 区域的 http、https 服务
  * 允许访问  `external` 区域的81 （TCP）端口 
  * 仅允许 192.168.56.30 的主机访问 `work` 区域（默认）的 syslog 服务
  * 删除 `external` 区域上默认设置的 IP 伪装功能
  * 显示 `external` 区域的所有规则
  * 添加 `work` 区域上对 `192.168.56.0/24` 的 IP 伪装功能
  * 在 `work` 区域（默认）上，将对 192.168.56.71:22/tcp 的访问重定向到新克隆的系统 192.168.56.30 的 22 端口
  * 在 `work` 区域（默认）上，将对 192.168.56.71:2022/tcp 的访问重定向到 c7-v1 容器的 22 端口
  * 显示 `work` 区域的所有规则
  * 显示 `public` 区域的所有规则
  * ......
  * 使上述配置持久化（下次启动 firewalld 时生效）
* 测试
  * 在防火墙主机上测试
        ping 192.168.56.1 
        ping 192.168.56.30
        ping 192.168.122.71

        ssh  192.168.122.71
        ssh -p 2222 192.168.122.71

        ssh 192.168.56.30
  * 在 Windows （192.168.56.1）上测试
        ssh -p 22     root@192.168.56.71
        ssh -p 2222   root@192.168.56.71
        ssh -p 2022   root@192.168.56.71
  * 在服务器主机（新克隆的虚拟机）上，测试 IP 伪装 
        elinks --dump http://whatismyip.org

>**提问**  下面命令的帮助输出中只有 **[Z]** 而没有 **[P]** 说明了什么？如何持久化地将一个网络接口绑定到一个 firewlld 的区域？
>```
># firewall-cmd --help |grep -A1 '\--change-interface='
>  --change-interface=<interface>
>                       Change zone the <interface> is bound to [Z]
>```
>**参考**
>* https://fedoraproject.org/wiki/Firewalld?rd=FirewallD
>* http://firewalld.org/

## 任务5：iptables 防火墙

1. 关闭 firewalld 并切换使用基于 iptables 服务的防火墙
   * 关闭并禁用 `firewalld`
   * 安装 `iptables-services` 和 `system-config-firewall-tui`
   * 启动并设置开机启动 `iptables` 服务
2. 配置  `iptables` 防火墙
   * 直接修改规则集文件 `/etc/sysconfig/iptables` 配置 
   * 使用  `system-config-firewall-tui` 或 `lokkit` 配置
3. 重新加载防火墙配置

>**提示** 你也可以在  c6-v1 容器上练习配置 iptables 主机防火墙


## 任务6：iptables 脚本

* 基于 `iptables-save` 命令输出文件格式的防火墙脚本
  * **参考** https://github.com/fcaviggia/hardened-centos7-kickstart/blob/master/config/hardening/iptables.sh
* 基于 `iptabls` 命令的防火墙脚本
  * **参考** http://easyfwgen.morizot.net/gen/index.php 
  * **参考** http://www.malibyte.net/iptables/scripts/fwscripts.html


## 任务7：Debian 防火墙

* 配置 Debian 系列服务器的防火墙
  * [Debian Firewall](https://wiki.debian.org/DebianFirewall)

>**参考**
>* [Debian VPS 系统 iptables 防火墙使用教程](https://yq.aliyun.com/ziliao/70072)
>* [Debian/Ubuntu系统中安装和配置 UFW](http://blog.csdn.net/jb19900111/article/details/18552913)
>* [ubuntu server guide -- firewall](https://help.ubuntu.com/lts/serverguide/firewall.html)
>* [Ufw 使用指南](http://wiki.ubuntu.org.cn/Ufw使用指南)





\newpage
# 实验指导 9.2 - 代理服务 与 VPN（选做）

>#### 学习目标
> * 使用 Squid 实现 HTTP/FTP 缓存代理
> * 使用 Shadowsocks 实现 Socks 代理
> * 使用 pptpd 实现基于 PPTP 协议的 VPN
> * 使用 libreswan 实现基于 IPSec 协议的 VPN
> * 使用 openvpn 实现基于 SSL 协议的 VPN


## 任务1：缓存代理

>参考
>* Squid - http://www.squid-cache.org
>* Varnish - https://www.varnish-cache.org

## 任务2：Socks 代理

>参考
>* https://shadowsocks.org （需科学上网）
>* https://github.com/xuxiaodong/selfhosted-server （基于 Ansible 的部署）

## 任务3：PPTP 协议的 VPN

>参考
>* https://github.com/drewsymo/VPN

## 任务4：IPSec 协议的 VPN

> 参考 
>* https://github.com/BoizZ/PPTP-L2TP-IPSec-VPN-auto-installation-script-for-CentOS-7
>* https://github.com/thefangbear/oneclickVPN

## 任务5：SSL 协议的 VPN

>参考
>* https://github.com/Nyr/openvpn-install
>* https://github.com/Angristan/OpenVPN-install

\newpage
# 实验指导 10 - Shell 编程

>#### 学习目标
> * 掌握脚本的编辑、运行和调试方法
> * 掌握 bash 变量的定义、引用、替换扩展
> * 掌握 bash 的各种流程控制语句
> * 掌握 bash 函数的定义和调用方法
> * 掌握命令行参数的使用方法
> * 掌握避免错误的处理方法使得脚本更健壮

## 任务1：编写一个最小化安装 CentOS7 后的配置脚本

**要求：**

1. 配置 YUM 并更新系统
  * 安装 EPEL 仓库
  * 导入已安装所有仓库的 GPG 签名文件 `/etc/pki/rpm-gpg/RPM-GPG-KEY-*`
  * 配置所有仓库的国内镜像URL
  * 更新系统
2. 安装常用的必要的软件
  * bash-completion vim-enhanced
  * net-tools psmisc yum-cron yum-utils yum-security
  * wget curl elinks w3m lftp mailx mutt rsync ntp git bind-utils
  * sysstat htop dstat nload nethogs iftop
  * rkhunter aide denyhost fail2ban lynis
3. 系统基本配置
  * 设置语言、时区
  * 设置主机名
  * 创建一个普通用户并为其设置口令
  * 禁用 chrony 和 ntp 服务
  * 使用 ntpdate 配置每日时间同步
  * 限制每个用户能打开的最大文件数
  * 限制仅 wheel 组的人才能使用 sudo 和 su 命令
  * 关闭 SELINUX
  * 确保网络参数配置正确后关闭 NetworkManager 服务
4. 安全配置
  * SSH
    * 允许使用口令登录但 root 用户只能使用密钥登录
    * 预先写入 root 远程 ssh 登录的公钥并为 .ssh 目录及其文件设置安全权限
    * 关闭 DNS 反向解析检查
    * ssh 回话闲置 5 分钟后自动注销登录
    * 安装配置 fail2ban
  * 口令
    * 口令必须大于12个字符
    * 必须至少包含四类字符中的一个
  * 网络 
    * 开启防火墙 22，80，443 端口

>* 扩展：使 `postinstall.sh` 脚本同时适用于 CentOS 6/7 
>* Vagrant 自动部署 -- https://www.vagrantup.com/docs/provisioning/shell.html

## 任务2：根据 shell 类型统计用户数

**要求：**

1. shell 类型通过命令行参数 `-s <SHELL>` 的形式指定
  * 显示 shell 类型为 SHELL 的用户数并列出所有用户名
  * SHELL 必须是 `/etc/shells` 文件中存在的类型，否则不执行脚本且退出状态码为1
2. 当没有给出命令行参数或命令行参数为 `-h | --help` 时，显示帮助信息并退出
3. 当命令行参数为其他值时，显示提示信息且退出状态码为2

若脚本名为 usersbysh，执行效果如下：

```
$ usersbysh -s bash
bash - 3 users , they are: root,student,tony
$ echo $?
0

$ usersbysh -s zash
Invalid shell.
$ echo $?
1

$ usersbysh
Usage: usersbysh -s <SHELL>|-h|--help
根据 shell 类型统计用户数并显示用户列表
$ echo $?
0

$ usersbysh --help
Usage: usersbysh -s <SHELL>|-h|--help
根据 shell 类型统计用户数并显示用户列表
$ echo $?
0

$ usersbysh -a
usersbysh：无效选项 -- a
Try 'usersbysh --help|-h' for more information.
$ echo $?
2
```

## 任务3：根据用户选择显示不同类型的系统信息

**要求：**

1. 向用户展示功能菜单，具有如下功能：
  * 显示所有登录用户
  * 显示系统平均负载
  * 显示物理内存的使用
  * 显示交换空间的使用
  * 显示磁盘空间的使用
  * quit或q
2. 读取用户从键盘上的选择
  * 根据不同的选择执行相应的命令
  * 选择 quit 或 q 时退出脚本
  * 输入错误的选择时退出脚本，且退出状态吗返回1

## 任务4：任务3扩展

**要求：**

1. 当用户选择显示相应信息后不退出，而让用户继续选择，继续显示相应内容，用户选择 quit 菜单项才退出。
2. 使用 `while` 语句 和 `select` 语句分别完成。

## 任务5：检测与服务器处于同一子网的每台主机的连通性

**要求：**

1. 通过 `ip|ifconfig` 命令获取本机 IP 地址
  * 使用变量替换扩展删除最后一位十进制数  
2. 通过 ping 命令测试当前主机与 1..254 所有主机的连通性
  * 若能 ping 通就显示 "`<IP>` is up."，其中的IP要换为真正的IP地址，且以绿色显示
  * 若不能 ping 通就显示"`<IP>` is down."，其中的IP要换为真正的IP地址，且以红色显示
3. 请分别使用 `while`，`until` 和 `for` (两种形式)循环实现

> **提示** 为避免突发性的线路故障发生，在使用 ping 时，每次等待 ping 的响应时间可以设置稍长一些（如：`-W 2`）

## 任务6：任务5 的函数实现方式

**要求：**

1. 使用函数实现获取本机 IP 地址
2. 使用函数实现一台主机的连通性判定
3. 在主程序中调用函数判定指定范围内的所有主机的在线情况

> **扩展** 根据子网掩码的值确定要 ping 的主机的 IP 地址范围

## 任务7：找出使用率大于 90% 的所有分区

**要求：**

1. 使用 read 配合 while 语句实现
2. 将 df 命令的输出结果通过管道传递给 while
  * 不要对虚拟文件系统进行操作 
3. 将结果通过邮件发送给 root
  *  只有当有使用率大于 90% 的分区时才发送邮件
  *  邮件主题为：Warn: Disk Usage on <主机的FQDN>
  *  邮件体包含：找出的分区设备名（挂装点）和使用率 

>1. **思考1**：循环控制语句 
>  * 若 df 的输出结果以使用率**降序**排序后再通过管道传递给 while 时，在循环体内可以使用哪个循环控制语句配合条件判断来优化？
>  * 若 df 的输出结果以使用率**升序**排序后再通过管道传递给 while 时，在循环体内可以使用哪个循环控制语句配合条件判断来优化？  
>2. **思考2**：下面的命令行能完全实现要求的功能吗？为什么？ 
>  * `df -PT |egrep -v '^Filesystem|tmpfs'|sort -nr -k6 | awk '{print $6 , $7}'|mail -s "Warn: Disk Usage on $(hostname -f)" root`
>  * `df -PT |egrep -v '^Filesystem|tmpfs' | sed 's/%//' |awk '$6>90 {printf "%d%% used on %s (%s)\n", $6, $7, $1}'|mail -s "Warn: Disk Usage on $(hostname -f)" root`


## 任务8：找出使用率大于 N% 的所有分区

任务7 扩展

**要求：**分别实现如下两种情况

1. N 通过命令行参数传递给脚本
   * 若 N 不是值为 0~99 的数字，给出出错提示并退出脚本且退出状态码为1 
2. N 通过 read 从键盘读取
  * 使用循环由键盘读取 N，直至读取的值为 0~99 的数字为止 


## 任务9：打印 1~N 之间的所有 奇数|偶数|素数

**要求：**

1. 编写一个函数用于判断一个字符串是否为纯数字的值 
2. 编写三个函数分别用于打印 1~N 之间的所有 奇数|偶数|素数
3. 编写一个函数显示脚本的格式和功能
3. 脚本的命令行参数
  * 第一个参数必须为一个数字，否则显示使用帮助信息，并设置退出状态码为1
  * -o|--odd ：打印奇数
  * -e|--even：打印偶数
  * -p|--prime：打印素数
  * -a|--all：打印奇数、偶数和素数
  * -h|--help：显示使用帮助信息
  * 对于其他的参数应该报错，并设置退出状态码为2
  * 可以同时指定多个参数，例如 -o -p 表示既打印奇数又打印素数

>**提示** 命令行参数处理：`getopts` 和 `getopt`
>* 参考 [Shell 参数(2) - 解析命令行参数工具：getopts/getopt](https://www.cnblogs.com/yxzfscg/p/5338775.html) 

## 参考资源

* [Shell 编程之语法基础](https://linuxtoy.org/archives/shell-programming-basic.html)
* [Linux Shell Scripting Tutorial (LSST) v2.0 ](https://bash.cyberciti.biz/guide/Main_Page)


\newpage
# 实验指导 11 - DHCP 和 DNS 服务

>#### 学习目标
>* 配置 DHCP 服务器
>* 配置唯高速缓存 DNS 服务器和转发器
>* 配置主 DNS 服务器
>* 配置辅助 DNS 服务器
>* 使用 `rndc` 命令管理 BIND
>* 使用 `dig`/`nslookup`/`host` 命令测试 DNS


## 任务1：DHCP 服务器

**要求**
* DHCP 服务只监听在 Host-Only 网卡
* 默认租约时间为18000秒；最大租约时间为36000秒
* 局域网内所有主机的域名为 `olabs.lan`
* 客户机使用的 DNS 服务器的 IP 地址是 DHCP 服务器的 Host-Only 网卡 IP 地址
* 动态分配的 IP 地址范围从 192.168.56.100/24~192.168.56.199/24，默认网关地址是 DHCP 服务器的 Host-Only 网卡 IP 地址
* 子网中有名为 cent7h2 的主机，需要固定分配 IP 地址 192.168.56.72，其他配置内容使用所在子网的配置
* 配置防火墙允许 192.168.56.0/24 访问 DHCP 服务器
* 配置 cent7h2 主机使用 DHCP 分配网络参数

## 任务2：唯高速缓存 DNS 服务器和转发器

**要求**
* DNS 服务只监听在 Host-Only 网卡
* 配置惟高速缓存 DNS 服务
* 设置仅允许 192.168.56.0/24 的主机查询
* 设置转发地址为 114.114.114.114   8.8.8.8
* 配置防火墙允许 192.168.56.0/24 查询 DNS 服务器
* 使用 DNS 查询工具检测配置
* 修改使用本 DNS 服务器的客户机的 DNS 配置

## 任务3：主 DNS 服务器


**要求**
* 配置 `olabs.lan` 的正向区域文件
* 配置 `56.168.192.in-addr.arpa` 的反向区域文件
* 配置正向区数据库文件和相应的 SOA/NS/MX/A/CNAME RR
* 配置反向区数据库文件和相应的 SOA/NS/PTR RR


## 任务4：辅助 DNS 服务器


**要求**
* 主 DNS 服务器
  * 配置仅允许辅助 DNS 进行区域传输
  * 配置防火墙仅允许辅助 DNS 服务器进行区域传输  
* 辅助 DNS 服务器
  * 配置 `olabs.lan` 的正向区域文件指定主 DNS 服务器
  * 配置 `56.168.192.in-addr.arpa` 的反向区域文件指定主 DNS 服务器
  * 配置防火墙允许 DNS 查询
* 测试辅助 DNS 服务器查询

## 任务5*：配置 Dnsmasq 服务器

**要求**
* 服务只监听在 Host-Only 网卡
* 本地域 DNS 服务器
* 非本地域的 DNS 转发器
* DHCP 服务器
\newpage
# 实验指导 12 - FTP 服务和 NFS 服务

>#### 学习目标
>* 配置匿名 FTP 服务
>* 配置本地用户 FTP 服务
>* 配置虚拟用户 FTP 服务
>* 配置基于 SSL/TLS 的 FTP 服务
>* 配置 NFS 服务和远程挂载


## 任务1：配置匿名 FTP 服务

**要求**
* 关闭本地用户访问
* 设置用户登录后提示为 “Welcome to our anonymous FTP service.”
* 匿名用户不能离开匿名服务器目录 /var/ftp，且只能下载不能上传
* 允许匿名用户上传到目录 /var/ftp/upload
* 允许匿名用户在 /var/ftp/upload 目录中创建目录、文件更名、删除文件
* 设置所有客户机的最大连接数为 100
* 设置每个客户机的最大连接数为 2
* 设置最大传输速率为 500KB/s
* 设置数据被动连接端口范围：10100~10200
* 开启防火墙允许访问 ftp 服务
* 自定义 firewalld 的配置文件 ftp.xml 添加被动连接端口范围

## 任务2：配置本地用户 FTP 服务

**要求**
* 关闭匿名用户访问
* 设置用户登录后提示为 “Welcome to our FTP service.”
* 本地用户可以上传、下载、创建目录、文件更名、删除文件
* 将本地用户限制在其自家目录中
* 禁止 fany 用户访问 FTP 服务
* 设置所有客户机的最大连接数为 200
* 设置每个客户机的最大连接数为 2
* 设置最大传输速率为 500KB/s
* 设置 tony 用户的最大传输速率为 1MB/s
* 设置数据被动连接端口范围：10100~10300
* 禁止 192.168.122.0/24 访问 FTP 服务
* 开启防火墙允许访问 ftp 服务
* 自定义 firewalld 的配置文件 ftp.xml 添加被动连接端口范围


## 任务3：配置基于 SSL/TLS 的 FTP 服务

**要求**
* 匿名用户无需SSL支持
* 本地用户的登录和数据传输均启用SSL/TLS
* 使用 第8章 任务10 创建的证书和私钥文件
  * ftp.olabs.lan.{crt,key}


## 任务4：配置虚拟用户 FTP 服务

**要求**
* 关闭匿名和本地用户访问
* 设置用户登录后提示为 “Welcome to our FTP service.”
* 虚拟用户可以上传、下载、创建目录、文件更名、删除文件
* 将虚拟用户限制在其登录目录中
* 开启防火墙允许访问 ftp 服务

```
# cat /etc/vsftp/vusers.txt
virtual-username1
password1
virtual-username2
password2
virtual-username3
password3
# useradd -d /srv/vftp -s /sbin/nologin vftp
# for u in `sed -n 1~2p /etc/vsftp/vusers.txt`；do
   mkdir -p /srv/vftp/$u
   chown vftp: /srv/vftp/$u
done
# db_load -T -t hash -f /etc/vsftp/vusers.txt /etc/vsftp/vusers.db
# chmod 600 /etc/vsftp/vusers.*
```

## 任务5：配置 NFS 服务和远程挂载

**要求**
* 在服务器端设置共享
  * 所有网络客户可只读访问 /var/ftp/pub  
  * 为 192.168.56.0/24 和 192.168.122.0/24 可只读访问 /var/ftp/yum ，但是 192.168.56.100/24 可读写
  * 为 192.168.56.30 设置对 /backup 的读写共享，且 root 可以以 root 权限访问而不是默认的 nfsnobody 用户权限
  * 为 192.168.56.0/24 设置对 /var/ftp/update 的读写共享，且全部访问者均映射为服务器本地的ftp用户和ftp组 
  * 开启防火墙对 NFS 服务的访问
  * 使用 `exportfs` 重新导出全部共享
* 在客户端挂载共享
  * 使用 `showmount` 查看 NFS 服务器的共享目录
  * 编辑 `/etc/fstab` 设置远程挂载





\newpage
# 实验指导 13 - Samba 服务

>#### 学习目标
>* 配置任何人均可读写的共享
>* 配置认证用户或组可读写的共享
>* 使用 `smbpasswd` 命令管理 Samba 账户数据库
>* 使用 `testparm` 命令检查配置文件的正确性
>* 在 Linux 系统上使用 `smbclient` 命令访问 Samba 共享
>* 在 Linux 系统上挂装 Samba 共享 

## 任务1：配置任何人均可读写的共享

**要求**
* 全局配置
  * 无需用户认证（将不存在用户访问视为Guest账号访问）
  * 将工作组设置为 Windows 默认的 `WORKGROUP`
  * 仅允许本地回环网络和 192.168.56.0/24 访问
  * 指定客户端支持的最高协议版本为 `SMB3` 
* 共享名 `[share]`
  * 共享目录 `/srv/samba/share`
  * 任何人均可读写
* 共享名 `[ftp]`
  * 共享目录 `/var/ftp/upload`
  * 任何人均可读写
  * 对共享目录写入的文件具有 ftp 的属主及组
* 使用 `testparm` 命令检查配置文件的正确性
* 开启防火墙对 samba 服务的访问
* 测试
  * 在 Windows 系统上测试
  * 在 Linux 系统上使用 `smbclient` 命令测试 
  * 在 Linux 系统上挂装 Samba 共享进行测试 

>**提示** 注意 `map to guest = Bad User` 和 `map to guest = Never` （默认值）的区别。

## 任务2：配置认证 用户/组 可读/写 的共享

**要求**
* 全局配置
  * 需用户认证（用户必须输入正确的口令方可访问）
  * 将工作组设置为 Windows 默认的 `WORKGROUP`
  * 仅允许本地回环网络和 192.168.56.0/24 访问
  * 指定客户端支持的最高协议版本为 `SMB3` 
* 共享名 `[homes]`
  * 共享用户自家目录
  * 仅用户自己可读写
  * 仅用户自己可以看到 
* 共享名 `[cdrom]`
  * 共享目录 `/media/cdrom`
  * 任何认证用户均可读
* 共享名 `[tmp]`
  * 共享目录 `/tmp`
  * 任何认证用户均可读写
* 共享名 `[staff]`
  * 共享目录 `/srv/samba/staff`
  * `staff` 组成员均可读写
  * 新建的文件或目录的组为 `staff`
  * 其他人没有任何权限
* 共享名 `[staff2]`
  * 共享目录 `/srv/samba/staff2`
  * `staff` 组成员均可读
  * `staff` 组成员 tony 可读写
  * 新建的文件或目录的组为 `staff`
  * 用户 osmond（不是 `staff` 组成员）可读写 
  * 其他人没有任何权限
* 使用 `smbpasswd` 命令创建 Samba 用户
* 使用 `testparm` 命令检查配置文件的正确性
* 开启防火墙对 samba 服务的访问
* 测试
  * 在 Windows 系统上测试
  * 在 Linux 系统上使用 `smbclient` 命令测试
  * 在 Linux 系统上挂装 Samba 共享进行测试 


>**参考**
>* http://www.techrepublic.com/article/how-to-manage-user-security-in-samba/

\newpage
# 实验指导 14 - Apache 静态站点

>#### 学习目标
>* Apache 的基本配置与安全设置
>* 配置每个系统用户的 Web 站点
>* 使用别名机制配置“虚拟目录”
>* 配置主机访问控制和用户访问控制
>* 配置基于 IP 和 Port 的虚拟主机
>* 配置基于 域名 的虚拟主机
>* 配置基于 SSL/TLS 的虚拟主机
>* 配置 URL/URI 重定向

## 任务1：基本配置与安全设置

### 要求

* 设置服务器名和管理员Email
* 开启 HTTP 的 `KeepAlive` 功能
* 禁用 `/etc/httpd/conf.d/welcome.conf` 配置文件
* 避免服务器信息外泄，控制服务器回应给客户端的应答头
* 控制服务器不显示生成页面的页脚的信息
* 安装配置 mod_evasive 模块防止 DoS 攻击

### 步骤

* 安装 Apache2.4
* 设置服务器名和管理员Email（请根据实际修改）
  * 假定服务器名为 `www.olabs.lan`
* 开启 HTTP 的 `KeepAlive` 功能
* 避免显示测试页面而暴露系统信息
  * 禁用 `/etc/httpd/conf.d/welcome.conf` 配置
  * 生成默认站点主页文件，其内容为 `<H1>It Works!</H1>`
* 避免服务器信息外泄，创建 `/etc/httpd/conf.d/security.conf`
  * 控制服务器回应给客户端的 `Server:` 应答头仅显示 `Apache`
  * 控制服务器不显示生成页面的页脚的信息
* 安装配置 mod_evasive 模块防止 DoS 攻击
  * 安装 EPEL 仓库提供的 mod_evasive
  * 配置 `/etc/httpd/conf.d/mod_evasive.conf`   
* 配置防火墙开启对 http 服务的访问
* 检查配置文件语法的正确性
* 配置 httpd 的开机启动和立即启动
* 使用支持 HTTP 协议的客户端进行测试
  * 本机：`curl --head http://localhost` 
  * 本机：`curl -vsI http://localhost | egrep '^(>|<)'`
  * 本机：`elinks http://localhost`
  * 本机：`elinks http://192.168.56.71`
  * 本机：`elinks http://www.olabs.lan`
  * Windows : http://IPorFQDN
* 查看 Apche 默认的错误日志和访问日志文件
* 使用 `ab` 命令进行压力测试
      ab -n 10000 -c 100 http://localhost/

>**参考**
>* curl 的常用选项
>  * `curl --help|egrep ' \-(I|L|s|S|v)'`
>* [Configure mod_evasive](https://www.server-world.info/en/note?os=CentOS_7&p=httpd2&f=4)

## 任务2：配置每个系统用户的 Web 站点

### 要求

配置每个系统用户的 Web 站点，并以 tony 用户为例进行测试

### 步骤

* 修改 `/etc/httpd/conf.d/userdir.conf` 文件
  * 为每个系统用户指定 Web 站点的文档根目录 `public_html`
* 检查语法正确性并重新启动Apache 
* 为 tony 用户准备 Web 站点 
      # useradd tony
      # mkdir -m 700 ~tony/public_html
      # echo "Test for tony." > ~tony/public_html/index.html
      # chown -R tony.tony ~tony/public_html
      # ls -ld  ~tony  ~tony/public_html
      # setfacl -m u:apache:x ~tony
      # setfacl -m u:apache:x ~tony/public_html
* 在客户端进行浏览测试
  *  http://IPorFQDN/~tony

## 任务3：主机访问控制、别名机制和目录列表

### 要求

* 假设 Apache 主机的 IP 为 192.168.56.71
* 配置 仅允许本地回环网络或 192.168.56.0/24 访问别名 /yum
* 对别名 /yum 所映射的文件系统目录开启目录列表功能

### 步骤

* 创建 `/etc/httpd/conf.d/pxe.conf` 文件
 * 将对 `/yum` 的别名访问映射到文件系统 `/var/ftp/yum`
 * 对目录 `/var/ftp/yum` 设置访问控制
   * 开启目录列表功能
   * 使用 CentOS 中 Apache 默认的目录列表选项配置 （`conf.d/autoindex.conf`）
   * 仅允许本地回环网络或 192.168.56.0/24 访问
* 检查语法正确性并重新启动Apache 
* 在客户端进行浏览测试
  * 本地：http://localhost/yum
  * 远程：http://192.168.56.71/yum
  * 远程：http://www.olabs.lan/yum

>**提示**
>* 在 192.168.56.0/24 之外的主机上测试，例如在 c6-v1 容器里测试 

## 任务4：主机访问控制与用户访问控制

### 要求

* 假设 Apache 主机的 IP 为 192.168.56.71
* 配置仅在 localhost 上可以直接访问 http://127.0.0.1/server-status
* 配置在 localhost 之外对 http://192.168.56.71/server-status 的访问启用 HTTP 基本认证
* 配置仅在 192.168.56.0/24 的网络上可以使用 HTTP 摘要用户认证访问 http://192.168.56.71/sec/

### 步骤
* 为 mod_status 模块设置基本认证的用户访问控制
  * 创建 `/etc/httpd/conf.d/server-status.conf` 文件
    * 配置 Location 容器 /server-status 设置访问控制
    * 开启 mod_status 模块生成服务器状态信息
    * 在 127.0.0.1 上可以直接访问
    * 在 127.0.0.1 之外的网络上使用用户认证访问
      * 使用 HTTP 的基本认证
      * jason 用户可以使用口令 JaP455 访问
  * 使用 `htpasswd` 命令设置基本认证口令文件 `/etc/httpd/.bpasswd` 
* 为 `/var/www/html/sec` 目录设置摘要认证的用户访问控制
  * 创建 `/etc/httpd/conf.d/sec-digest.conf` 文件
    * 配置 `/var/www/html/sec` 目录设置访问控制
    * 仅在 192.168.56.0/24 的网络上可以使用用户认证访问
      * 使用 HTTP 的摘要认证
      * jason 用户可以使用口令 `JaP455` 访问
  * 使用 `htdigest` 命令设置摘要认证口令文件 `/etc/httpd/.dpasswd` 
* 检查语法正确性并重新启动 Apache 
* 在客户端进行浏览测试
  * 本地：`apachectl fullstatus`
  * 本地：`elinks http://127.0.0.1/server-status`
  * 本地：`elinks http://127.0.0.1/sec/`
  * 本地：`elinks http://192.168.56.71/sec/`
  * 远程：http://IPorFQDN/server-status
  * 远程：http://IPorFQDN/sec/


>**提示**
>* 在 192.168.56.0/24 之外的主机上测试，例如在 c6-v1 容器里测试 


## 任务5：基于 IP 和 Port 的虚拟主机

### 要求

* 假设 Apache 主机的 IP 为 192.168.56.71
* 配置基于端口号的 虚拟主机 http://192.168.56.71:8888
* 配置基于 IP 的 虚拟主机 http://192.168.56.111

### 准备

* 准备虚拟站点目录和 index.html 文件
      # mkdir -p /srv/www/192.168.56.{111_80,71_8888}/{htdocs,logs}
      # for i in 192.168.56.{111_80,71_8888} ;\ 
          do echo "$i" > /srv/www/$i/htdocs/index.html ; done 
      # tree /srv/www
```
/srv/www/
├── 192.168.56.111_80
│     ├── htdocs
│     │   └── index.html
│     └── logs
└── 192.168.56.71_8888
        ├── htdocs
        │   └── index.html
        └── logs
```
* 准备被 Apache 主配置文件包含的用于存放虚拟主机配置文件的目录
      # mkdir /etc/httpd/vhosts.d
      # echo 'IncludeOptional vhosts.d/*.conf' >> /etc/httpd/conf/httpd.conf

### 步骤

* 配置基于端口号的虚拟主机
  * 创建  `/etc/httpd/vhosts.d/192.168.56.71_8888.conf`
    * 配置虚拟主机的文档根目录为  `/srv/www/192.168.56.71_8888/htdocs`
* 配置基于 IP 的虚拟主机
  * 为本机的 host-Only 网卡绑定第2个IP地址 192.168.56.111/24
  * 创建  `/etc/httpd/vhosts.d/192.168.56.111.conf`
    * 配置虚拟主机的文档根目录为  `/srv/www/192.168.56.111_80/htdocs`
* 检查语法正确性并重新启动 Apache 
* 配置防火墙允许访问 8888 端口
* 配置域名解析 （bind 或 /etc/hosts）
  * 将 `h111.olabs.lan` 的 IP 设置为地址 192.168.56.111
* 在客户端进行浏览测试
  * `elinks http://192.168.56.71:8888`
  * `elinks http://www.olabs.lan:8888`
  * `elinks http://192.168.56.111`
  * `elinks http://h111.olabs.lan`

## 任务6：基于 域名 的虚拟主机

### 要求

* 创建由 root 管理的 www.olabs.net 和 wiki.olabs.net 的虚拟主机
* 创建由 olabsorg 管理的 www.olabs.org 和 wiki.olabs.org 的虚拟主机

### 准备

* 为 root 用户准备虚拟站点目录和 index.html 文件
      # mkdir -p /srv/www/olabs.net/{www,wiki}/{htdocs,logs,conf}
      # echo "www.olabs.net" > /srv/www/olabs.net/www/htdocs/index.html 
      # echo "wiki.olabs.net" > /srv/www/olabs.net/wiki/htdocs/index.html
* 为 olabsorg 用户准备虚拟站点目录和 index.html 文件
      # useradd -d /srv/www/olabs.org  olabsorg
      # su - olabsorg -c "mkdir -p ~olabsorg/{www,wiki}/{htdocs,logs,conf}"
      # su - olabsorg
      $ echo "www.olabs.org" > www/htdocs/index.html
      $ echo "wiki.olabs.org" > wiki/htdocs/index.html
      $ exit
* 显示 /srv/www 目录结构
      # tree /srv/www
```
/srv/www
├── olabs.net
│   ├── wiki
│   │   ├── conf
│   │   ├── htdocs
│   │   │   └── index.html
│   │   └── logs
│   └── www
│       ├── conf
│       ├── htdocs
│       │   └── index.html
│       └── logs
└── olabs.org
      ├── wiki
      │   ├── conf
      │   ├── htdocs
      │   │   └── index.html
      │   └── logs
      └── www
          ├── conf
          ├── htdocs
          │   └── index.html
          └── logs
```
* 准备被 Apache 主配置文件包含的用于存放虚拟主机配置文件的目录
```
grep 'vhosts.d' /etc/httpd/conf/httpd.conf &> /dev/null \
  || echo 'IncludeOptional vhosts.d/*.conf' >> /etc/httpd/conf/httpd.conf
```

### 步骤

* 创建  `/etc/httpd/vhosts.d/olabs.org.conf`
  * 配置 www.olabs.org 的虚拟主机
    * 配置虚拟主机的文档根目录为  `/srv/www/olabs.org/www/htdocs` 
    * 配置虚拟主机的错误日志为 `/srv/www/olabs.org/www/logs/error_log`
    * 配置虚拟主机的访问日志为 `/srv/www/olabs.org/www/logs/access_log`
  * 配置 wiki.olabs.org 的虚拟主机
    * 配置虚拟主机的文档根目录为  `/srv/www/olabs.org/wiki/htdocs` 
    * 配置虚拟主机的错误日志为 `/srv/www/olabs.org/wiki/logs/error_log`
    * 配置虚拟主机的访问日志为 `/srv/www/olabs.org/wiki/logs/access_log`
* 创建  `/etc/httpd/vhosts.d/olabs.net.conf`
  * 配置 www.olabs.net 的虚拟主机 
    * 配置虚拟主机的文档根目录为  `/srv/www/olabs.net/www/htdocs` 
    * 配置虚拟主机的错误日志为 `/srv/www/olabs.net/www/logs/error_log`
    * 配置虚拟主机的访问日志为 `/srv/www/olabs.net/www/logs/access_log`
  * 配置 wiki.olabs.net 的虚拟主机
    * 配置虚拟主机的文档根目录为  `/srv/www/olabs.net/wiki/htdocs` 
    * 配置虚拟主机的错误日志为 `/srv/www/olabs.net/wiki/logs/error_log`
    * 配置虚拟主机的访问日志为 `/srv/www/olabs.net/wiki/logs/access_log`
* 创建  `/etc/httpd/vhosts.d/olabs.lan.conf`
  * 为主服务器配置默认虚拟主机，域名为 `www.olabs.lan`
    * `<VirtualHost _default_:80>` 
* 为所有虚拟主机配置日志滚动
```
if [ -e /etc/logrotate.d/httpd_vhosts ] ; then :
else
   cp /etc/logrotate.d/httpd{,_vhosts}
   sed -i 's#/var/log/httpd#/srv/www/*/*/logs#' /etc/logrotate.d/httpd_vhosts
fi
```
* 配置域名解析 （bind 或 /etc/hosts）
  * 将 {www,wiki}.olabs.{org,net} 的 IP 设置为本机地址 192.168.56.71
* 检查语法正确性并重新启动 Apache 
* 检查 Apache 基于域名的虚拟主机配置
* 在客户端进行浏览测试
  * `elinks http://www.olabs.net`
  * `elinks http://wiki.olabs.net`
  * `elinks http://www.olabs.org`
  * `elinks http://wiki.olabs.org`
  * `elinks http://www.olabs.lan`


## 任务7：基于 SSL/TLS 的 虚拟主机

### 要求

* 配置对 www.olabs.lan 的 HTTPS 访问
* 配置对 wiki.olabs.net 的 HTTPS 访问

### 步骤

* 安装 mod_ssl
* 为默认虚拟主机配置 SSL/TLS
  * 修改 `/etc/httpd/conf.d/ssl.conf` 
    * 使用 第8章 任务10 创建的证书和私钥文件 `myservers.{crt,key}` 
* 为 wiki.olabs.net 虚拟主机配置 SSL/TLS
  * 修改 `/etc/httpd/vhosts.d/olabs.net.conf`
    * 创建 wiki.olabs.net:443 虚拟主机
    * 使用 第8章 任务10 创建的证书和私钥文件 `myservers.{crt,key}` 
* 检查语法正确性并重新启动 Apache 
* 检查 Apache 基于域名的虚拟主机配置
* 配置防火墙允许访问 https 服务
* 在客户端进行浏览测试
  * `https://www.olabs.lan`
  * `https://wiki.olabs.net`

## 任务8：重定向

### 要求

* 将 http://olabs.org 永久重定向到 http://www.olabs.org
* 将 http://dl.olabs.org 永久重定向到 http://www.olabs.org/download
* 将 http://wiki.olabs.net 永久重定向到 https://wiki.olabs.net
* 将 http://help.olabs.net 永久重定向到 https://wiki.olabs.net

### 步骤

1. 将 http://olabs.org 永久重定向到 http://www.olabs.org
  * 修改 `/etc/httpd/vhosts.d/olabs.org.conf` 
    * 创建基于域名 olabs.org 的虚拟主机
      * 永久重定向到 http://www.olabs.org 
2. 将 http://dl.olabs.org 永久重定向到 http://www.olabs.org/download
  * `mkdir /srv/www/olabs.org/www/htdocs/download`
  * 修改 `/etc/httpd/vhosts.d/olabs.org.conf` 
    * 创建基于域名 dl.olabs.org 的虚拟主机
      * 永久重定向到 http://www.olabs.org/download 
3. 将 http://wiki.olabs.net 永久重定向到 https://wiki.olabs.net
  * 修改 `/etc/httpd/vhosts.d/olabs.net.conf` 
    * 修改基于域名 wiki.olabs.net:80 的虚拟主机
      * 永久重定向到 https://wiki.olabs.net 
4. 将 http://help.olabs.net 永久重定向到 https://wiki.olabs.net
  * 修改 `/etc/httpd/vhosts.d/olabs.net.conf` 
    * 创建基于域名 help.olabs.net 的虚拟主机
      * 永久重定向到 https://wiki.olabs.net 
5. 重新加载 Apache 配置
  * 检查 Apache 基于域名的虚拟主机配置
  * 检查语法正确性并重新启动 Apache 
6. 配置域名解析 （bind 或 /etc/hosts）并测试
  * 将 {,dl.}olabs.org、{help,wiki}.olabs.net 的 IP 设置为本机地址 192.168.56.71
  * 在客户端进行浏览测试
         curl -I http://olabs.org
         curl -IL http://olabs.org
         curl -I http://download.olabs.org
         curl -IL http://download.olabs.org
         curl -I http://wiki.olabs.net
         curl -IL http://wiki.olabs.net
         curl -I http://help.olabs.net
         curl -IL http://help.olabs.net

## 任务9*：限制每 IP 的最大连接数和限速

### 要求

* 为 http://www.olabs.org/download 的访问限制带宽为 500 KB/sec
* 为 http://www.olabs.org/download 的访问限制每个 IP 的最大连接数为 5

### 步骤

* 限速
  * 修改 `/etc/httpd/vhosts.d/olabs.org.conf`
  * 为 www.olabs.org 的 /download 限制带宽为 500 KB/sec
* 限制每个 IP 的连接数
  * 安装 mod_limitipconn
  * 修改 `/etc/httpd/conf.modules.d/10-limitipconn.conf`
    * 在 `IfModule` 容器中添加 `MaxConnPerIP 0` 表示默认无限制
  * 修改 `/etc/httpd/vhosts.d/olabs.org.conf`
    * 为 www.olabs.org 的 /download 限制每个 IP 的最大连接数为 5
* 检查 Apache 基于域名的虚拟主机配置
* 在客户端进行浏览测试


>**参考**
>* [Configure mod_ratelimit](https://www.server-world.info/en/note?os=CentOS_7&p=httpd2&f=3)
>* [Use mod_limitipconn](https://www.server-world.info/en/note?os=CentOS_7&p=httpd2&f=9)


## 任务10*：CentOS 6 和 Debian 9 上的 Apache

### 要求
* 在 CentOS 6 上配置 Apache 2.2
  * 在容器 c6-v1 上完成 任务1~任务9 的功能 
* 在 Debian 9 上配置 Apache 2.4
  * 在容器 d9-v1 上完成 任务1~任务9 的功能 

>**参考**
>* [Apache on CentOS 6](https://www.server-world.info/en/note?os=CentOS_6&p=httpd)
>* [Apache on Debian 9](https://www.server-world.info/en/note?os=Debian_9&p=httpd)
\newpage
# 实验指导 15.1 - LAMP

>#### 学习目标
>* 配置基于 php5_module 模块的 LAMP 环境
>* 配置基于 php-fpm 和 proxy_fcgi_module 模块的 LAMP 环境
>* 安装 SCL 仓库中的 PHP 7.0
>* 安装配置 LAMP 应用
>* 配置 AWStats 实现虚拟主机访问日志分析统计

## 任务1：安装配置 LAMP 环境(1)

### 要求

基于 CentOS7 官方仓库和 EPEL 仓库配置 LAMP 环境
* Aache2.4 + mpm_prefork_module + php5.4 + php5_module 
* MariaDB5.5

### 步骤

1. 安装配置 MariaDB/MySQL 服务
  * 安装 mariadb 和 mariadb-server 
  * 配置 MariaDB
    * 编辑主配置文件 `/etc/my.cnf`，在 `[mysqld]` 段里设置
      * character-set-server=utf8
      * collation-server=utf8_general_ci 
      * max_connections = 50 （默认最大并发连接数为100）
    * 编辑主配置文件 `/etc/my.cnf.d/client.cnf`，在 `[client]` 段里设置
      * character-set-server=utf8
  * 启动 MariaDB 服务，并设置开机启动
  * 配置 MariaDB 服务的 root 用户口令
    *  `mysqladmin -u root password 'Med7ahBuu7ru2Wooyohg'`
    *  或 `mysql_secure_installation`
  * 安装 EPEL 仓库中的并获取更多的配置建议
2. 安装配置 PHP
  * 安装 PHP 及相关模块
         yum install php \
          php-{pdo,mcrypt,mbstring,intl,gd,pecl-{imagick,memcached,redis}
  * 检测安装的 PHP （cli） 版本
         php -v
  * 配置 PHP
    * 编辑主配置文件 `/etc/php.ini`，在 `[PHP]` 段里设置
      * 使用 zlib 库压缩输出并设置压缩级别为 1
      * 限制一个 PHP 脚本可能消耗的最大内存量为 256M
      * 为 POST 方法指定可接受的最大尺寸为 256M
      * 设置可上传文件的最大尺寸为 20M
    * 编辑主配置文件 `/etc/php.ini`，在 `[Date]` 段里设置
      * 为日期函数定义默认时区为 `Asia/Shanghai`
3. 通过 Aapche 的 PHP 模块执行 PHP 脚本
  * 查看 Apche 的 PHP 模块 加载/配置 文件
         grep -v "^#" /etc/httpd/conf.modules.d/10-php.conf
         grep -v "^#" /etc/httpd/conf.d/php.conf
  * 检测 Apache 配置正确性并重新加载 Apache 配置
4. 测试
  * 编写 PHP 测试脚本
         echo '<?php phpinfo()?>' > /var/www/html/info.php
         echo '<?php phpinfo()?>' > /srv/www/olabs.net/www/htdocs/info.php 
  * 在客户端上测试
         elinks http://www.olabs.lan/info.php
         elinks http://www.olabs.net/info.php
  * 压力测试
         ab -n 2000 -c 100 http://www.olabs.net/info.php
5. 使用 `lynis -c` 扫描整个系统，根据报告提示加固系统

>**参考**
>* [How To Install Linux, Apache, MySQL, PHP (LAMP) stack On CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-centos-7)
>* [超实用压力测试工具－ab工具](https://www.jianshu.com/p/43d04d8baaf7)
>* [PHP 缓存](http://laravel-china.github.io/php-the-right-way/#caching)
>* [Easy Deployment of PHP Applications with Deployer](https://www.sitepoint.com/deploying-php-applications-with-deployer/)
>* [LAMPer 技能树](https://laravel-china.org/topics/384/lamper-skill-tree)


## 任务2：安装配置 LAMP 应用（1）

### 要求

安装配置
* phpMyAdmin
* dokuwiki (`.htaccess`)
* h5ai

### 步骤

1. 安装配置 phpMyAdmin
  * 安装 phpMyAdmin 
  * 配置 phpMyAdmin
    * 修改 phpMyAdmin 的配置文件 `/etc/phpMyAdmin/config.inc.php`
      * 为 Cookies 认证重新设置加密口令
    * 修改 Apache 的配置文件 `/etc/httpd/conf.d/phpMyAdmin.conf`
      * 配置允许 192.168.56.0/24 访问 
  * 检测 Apache 配置正确性并重新加载 Apache 服务的配置
  * 测试 phpMyAdmin 
    *  https://www.olabs.lan/phpmyadmin/
    *  https://www.olabs.net/phpmyadmin/ 
2. 安装配置 dokuwiki
  * 下载并部署 dokuwiki 站点内容
```
mkdir /srv/www/olabs.net/wiki/src
cd /srv/www/olabs.net/wiki/src
wget https://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz
tar xzf dokuwiki-stable.tgz
cd ..
rm -rf htdocs
mv src/dokuwiki-2017-02-19e htdocs
```
  * 在浏览器里执行安装脚本进行安装
    * http://wiki.olabs.net/install.php
    * 根据提示信息合理设置文件系统权限
    * 安装后删除 install.php 文件
  * 编辑 Wiki 页面
  * 配置伪静态访问 
    * 编辑  `/etc/httpd/vhosts.d/olabs.net.conf` ，修改 wiki.olabs.net:443 的虚拟主机
      * 配置允许读取 `/srv/www/olabs.net/wiki/htdocs/` 目录下的配置文件 `.htaccess` 
    * 编辑 `/srv/www/olabs.net/wiki/htdocs/conf/local.php` 启用伪静态访问
      * `echo "\$conf['userewrite'] = 1;" >>  /srv/www/olabs.net/wiki/htdocs/conf/local.php`
    * 修改
      * 复制默认发布的 .htaccess.dist 为 .htaccess
        * `cp /srv/www/olabs.net/wiki/htdocs/.htaccess{.dist,}`
      * 编辑 `/srv/www/olabs.net/wiki/htdocs/.htaccess` 使之适应本站
    * 测试伪静态访问 
      * http://wiki.olabs.net/wiki/syntax  
3. 安装配置 h5ai 
  * 为 `/srv/www/olabs.org/www/htdocs/download` 目录安装配置 `h5ai`
  * 使访问 http://www.olabs.org/download 时的显示效果像 https://release.larsjung.de/

>**参考**
>* https://www.dokuwiki.org/install
>* https://www.dokuwiki.org/rewrite
>* https://larsjung.de/h5ai/

## 任务3：安装配置 LAMP 环境(2)

### 要求

基于 CentOS7 官方仓库和 EPEL 仓库配置 LAMP 环境
* Aache2.4 + **mpm_envent_module** + php5.4 + **php-fpm** + **proxy_fcgi_module**
* MariaDB5.5

### 步骤

1. 安装并启动 `php-fpm`
  * 删除用于 Apache 模块的 `php` 包
  * 安装 `php-fpm`
  * 开机/立即 启动 `php-fpm` 守护进程
2. 查看并配置 `php-fpm`
  * 查看 `php-fpm` 版本
         php-fpm -v
  * 查看 `php-fpm` 进程
         ps -HO user -p `pidof php-fpm`
         lsof -i :9000
  * 查看 `php-fpm` 的主配置文件 `/etc/php-fpm.conf`
  * 查看 `php-fpm` 进程池配置文件 `/etc/php-fpm.d/www.conf`
3. 配置 Apache
  * 配置 Apache 通过 **proxy_fcgi_module** 访问 `php-fpm`
    * 创建或编辑 `/etc/httpd/conf.d/php.conf`：
           # Allow php to handle Multiviews
           AddType text/html .php
                   
           # Add index.php to the list of files 
           # that will be served as directory indexes.
           DirectoryIndex index.php
           
           # Enable the http authorization headers.
           SetEnvIfNoCase ^Authorization$ "(.+)" HTTP_AUTHORIZATION=$1
           
           # Redirect the PHP scripts execution to the FPM backend.
           <FilesMatch \.php$>
               SetHandler "proxy:fcgi://127.0.0.1:9000"
           </FilesMatch>
           
           # Prevent .user.ini files from being viewed by Web clients.
           <Files ".user.ini">
               Require all denied
           </Files>
  * 配置 Apache 启用 **mpm_envent_module**
    * 编辑 `/etc/httpd/conf.modules.d/10-php.conf`
           # LoadModule mpm_prefork_module modules/mod_mpm_prefork.so
           # LoadModule mpm_worker_module modules/mod_mpm_worker.so
           LoadModule mpm_event_module modules/mod_mpm_event.so
  * 检测 Apache 配置正确性并重新加载 Apache 服务的配置
4. 测试 Apache 是否启用了 `FPM/FastCGI`
       elinks --dump http://localhost/info.php |grep 'Server API'
       elinks --dump  http://www.olabs.lan/info.php |grep 'Server API'
       elinks --dump  http://www.olabs.net/info.php |grep 'Server API'
5. 压力测试
       ab -n 2000 -c 100 http://www.olabs.net/info.php 

>**参考**
>* [PHP + PHP-FPM](https://www.server-world.info/en/note?os=CentOS_7&p=httpd&f=19)
>* [PHP Configuration Tips](https://developers.redhat.com/blog/2017/10/25/php-configuration-tips/)


## 任务4：安装配置 LAMP 环境(3)

### 要求

基于 SCL 仓库配置 LAMP 环境
* Aache2.4 + **mpm_envent_module** + **php70** + **php-fpm** + **proxy_fcgi_module**
* MariaDB5.5

### 步骤

1. 安装配置 SCL 仓库
       yum -y install centos-release-scl scl-utils
       rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-SCLo
       yum repolist |grep sclo
       yum makecache
2. 安装 php70 及相关模块
       yum -y install rh-php70 \
          rh-php70-php-{pdo,mcrypt,mbstring,intl,gd,pecl-{imagick,memcached,redis},fpm}
       scl -l
3. 切换 php-fpm 到新版本
       systemctl stop php-fpm
       systemctl disable php-fpm
       systemctl start rh-php70-php-fpm
       systemctl enable rh-php70-php-fpm
4. 测试
       elinks --dump http://localhost/info.php |egrep 'Server API|PHP Version'


>### 在命令行上切换使用 SCL 仓库的 PHP
>* 检测原来安装的 PHP 版本
>       which php ; php -v 
>       which php-fpm ; php-fpm -v
>* 切换新 Shell 使用 SCL 仓库的 PHP
>       scl -l
>       scl enable rh-php70 bash
>* 检测新安装的 PHP 版本
>       which php ; php -v
>       which php-fpm ; php-fpm -v
>* 退出新 shell，重新查看 PHP 版本
>       exit
>       which php ; php -v
>       which php-fpm ; php-fpm -v

>### 在命令行上默认使用 SCL 仓库的 PHP
>```
>cat >/etc/profile.d/rh-php70.sh <<_END
>#!/bin/bash
>
>source /opt/rh/rh-php70/enable
>export X_SCLS="\$(scl enable rh-php70 'echo \$X_SCLS')"
>_END
>```

>### 配置 SCL 仓库的 PHP/PHP-FPM
>* 配置文件基于 `/etc/opt/rh/rh-php70/` 目录
>  * php 配置文件: `php.ini` , `php.d/*.ini`
>  * php-fpm 配置文件: `php-fpm.conf` , `php-fpm.d/*.conf`


>### 参考
>* SCL 仓库
>  * https://wiki.centos.org/zh/AdditionalResources/Repositories/SCL
>  * https://wiki.centos.org/SpecialInterestGroup/SCLo/CollectionsList
>  * [Release Notes for Red Hat Software Collections 3.0](https://access.redhat.com/documentation/en-us/red_hat_software_collections/3/html-single/3.0_release_notes/)
>  * [Software Collections](https://www.softwarecollections.org)
>* [PHP Configuration Tips](https://developers.redhat.com/blog/2017/10/25/php-configuration-tips/)
>* http://phpversions.info/operating-systems/


>### 练习
>
>* 安装并启用 SCL 仓库中的 Mariadb v10.2
>       yum search rh-mariadb102


## 任务5：使用 AWStats 实现访问日志分析统计

### 要求

为 www.olabs.{net,org} 虚拟主机配置 AWStats 实现访问日志分析统计

### 步骤

1. 安装 awstats
2. 配置 awstats，为每个虚拟主机创建各自的配置文件
  * `/etc/awstats/awstats.www.olabs.net.conf`
  * `/etc/awstats/awstats.www.olabs.org.conf`  
3. 立即更新指定配置文件的 AWStats 的统计数据库
  * `/usr/share/awstats/wwwroot/cgi-bin/awstats.pl -config=www.olabs.net`
  * `/usr/share/awstats/wwwroot/cgi-bin/awstats.pl -config=www.olabs.org`  
4. 为了通过 CGI 访问查看 AWStats 的 WUI 配置 Apache
  * 编辑被 Apache 主配置文件包含的 `/etc/httpd/conf.d/awstats.conf`
  * 在适当位置添加 HTTP 摘要认证配置，可以使用 第14章任务4 创建的摘要认证口令文件 `/etc/httpd/.dpasswd`  
5. 检测 Apache 配置正确性并重新加载 Apache 配置
6. 通过浏览器访问 AWStats，并测试摘要认证
   * http://www.olabs.net/awstats/awstats.pl
   * http://www.olabs.net/awstats/awstats.pl?config=www.olabs.net
   * http://www.olabs.net/awstats/awstats.pl?config=www.olabs.org


>**参考**
>* [Access Log Analyzer : AWstats](https://www.server-world.info/en/note?os=CentOS_7&p=httpd&f=15)


## 任务6*：安装配置 LAMP 应用（2）

### 要求

* 为 http://blog.olabs.org 安装配置一个基于 Laravel 框架的应用

### 步骤

1. 安装配置 composer
  * 安装 composer

         # curl -sS https://getcomposer.org/installer | php

         # mv composer.phar /usr/local/bin/composer
  * 配置 composer 全局使用的镜像站点

         # composer config -g repo.packagist composer \

              https://packagist.phpcomposer.com
         # tree -F -L 2 ~olabsorg
2. 安装 laravel
       # su - olabsorg
       
       $ scl enable rh-php70 bash
       $ composer create-project  laravel/laravel blog --prefer-dist "5.5.*"
3. 配置并测试 blog.olabs.org 虚拟主机
  * 修改 `/etc/httpd/vhosts.d/olabs.org.conf` 
```
<VirtualHost *:80>
    ServerAdmin root@localhost
    ServerName blog.olabs.org:80
    DocumentRoot /srv/www/olabs.org/blog/public/
    ErrorLog  /srv/www/olabs.org/blog/logs/error_log
    CustomLog /srv/www/olabs.org/blog/logs/access_log combined
    <Directory /srv/www/olabs.org/blog/public/>
        Options +FollowSymLinks
        Require all granted

        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^ index.php [L]
    </Directory>
</VirtualHost>
```
  * 检测 Apache 配置正确性并重新加载 Apache 服务的配置
  * 配置 blog.olabs.org 的域名解析 （bind 或 /etc/hosts）
  * 测试 http://blog.olabs.org
4. 安装配置基于 laravel 的后台生成工具 [Voyager](https://laravelvoyager.com/)
  * 使用 composer 安装 Voyager 所需的 PHP 软件包
  * 创建 BLOG 所需的 mysql 数据库
  * 配置项目下的环境文件 `.env` 
  * 使用 artisan 执行 voyager 的安装并填充测试数据
  * 重新设置管理员 admin 的口令和 Email
  * 在浏览器中访问 BLOG 的后台管理界面  http://blog.olabs.org/admin
5. 基于 Voyager 后台编写代码，创建 BLOG 前台 http://blog.olabs.org

>**参考**
>* https://d.laravel-china.org/docs/5.5/installation
>* https://github.com/the-control-group/voyager
>* https://startbootstrap.com/template-categories/blogs/
>
>** 练习**
>* 安装  [Grav](https://getgrav.org/) 或 [BookStack](https://www.bookstackapp.com/)

## 任务7*：Memcached 和 Redis

### 要求
* 安装、配置并启动 Memcached
* 安装、配置并启动 Redis

>**参考**
>* [Memcached : Install](https://www.server-world.info/en/note?os=CentOS_7&p=memcached)
>  * [Memcached : Use it on PHP](https://www.server-world.info/en/note?os=CentOS_7&p=memcached&f=4)
>* [Redis : Install](https://www.server-world.info/en/note?os=CentOS_7&p=redis&f=1)
>  * [Redis : Use it on PHP](https://www.server-world.info/en/note?os=CentOS_7&p=redis&f=9)

## 任务8*：使用 mylvmbackup 实现 MySQL 数据库的物理备份

### 要求
* 安装 EPEL 仓库中的 mylvmbackup
* 配置 mylvmbackup
  * `/etc/mylvmbackup.conf`
* 安排每日 1点 执行的 cron 任务
  * `/etc/cron.d/mylvmbackup`

>**参考**
>* http://www.lenzg.net/mylvmbackup/
>* ` man mylvmbackup`

## 任务9*：CentOS 6 和 Debian 9 的 LAMP 环境配置

### 要求
* 在 c6-v1 容器上配置 **Apache2.4+php7.0+php-fpm+mpm_envent_module**
* 在 d9-v1 容器上配置 **Apache2.4+php7.0+php-fpm+mpm_envent_module**
\newpage
# 实验指导 15.2 - Tomcat & Apache

>#### 学习目标
* 安装配置 OpenJDK 或 Java SE Development Kit(Oracle) 
* 安装配置独立运行的 Tomcat
* 配置 Tomcat 第二实例
* 配置 Apache 反向代理 Tomcat 
* 配置 Apache 反向代理 usermin 

## 任务1：安装 OpenJDK 和 Tomcat

**要求**
* 安装 java-1.8.0-openjdk 和 tomcat
* 配置独立运行的 Tomcat
* 测试独立运行的 Tomcat
* 安装 Tomcat 的默认 Web应用、管理应用和文档
* 配置 Tomcat 的管理应用
  * 用户 fanny 只能访问 manager 界面
  * 用户 tonny 既可以访问 manager 也可以访问 admin 界面
* 分别以不同用户测试管理界面访问

>**参考**
>* [Install JDK 9](https://www.server-world.info/en/note?os=CentOS_7&p=jdk9&f=1)
>* [Install JDK 8](https://www.server-world.info/en/note?os=CentOS_7&p=jdk8&f=1)
>* [Install OpenJDK 8](https://www.server-world.info/en/note?os=CentOS_7&p=jdk8&f=2)


## 任务2：配置 Tomcat 第二个实例

**要求**
* 配置可由 systemd 管理的第二个 Tomcat 实例


## 任务3：Apache 反向代理

**要求**
* 配置 Apache 反向代理 Tomcat 默认实例
  * 不对 /docs 的URI访问进行反向代理
  * 使用 AJP 协议反向代理 Tomcat 的默认实例
* 配置 Apache 反向代理 Tomcat 第二个实例
* 配置 Apache 反向代理 usermin （https://localhost:20000）
\newpage
# 实验指导 16 - 邮件服务

>#### 学习目标
>* 配置基于系统用户的邮件服务器
>* 使用 swaks/mail 进行邮件测试
>* 安装配置 WebMail


## 任务1：安装配置邮件服务

* 基于 Linux 系统用户实现邮件服务
* 允许 Postfix 为本地服务器及本地域投递邮件
* 通过 Postfix 的映射表配置将发往 root 的邮件同时发给 tonny 用户
* 基于 SASL 配置 SMTP 身份认证
* 配置 Postfix 支持 ESMTPS （25/tcp）
* 配置 Dovecot 支持 POP3S （995/tcp）、IMAPS （993/tcp）
* 安装一款 WebMail 

>* **常见的 Webmail**
>  * RoundCube - http://roundcube.net
>  * SquirrelMail - http://squirrelmail.org
>  * RainLoop - http://www.rainloop.net
>* [Top 9 Best Email Clients for Linux](https://itsfoss.com/best-email-clients-linux/)

## 任务2*：安装 iRedMail

* 在新建的 虚拟机/容器 里安装 [iRedMail](http://www.iredmail.org)


\newpage
