# 实验指导 4 - 服务管理与系统日常维护

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


## 任务11： 使用监视工具分析系统性能

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

## 任务12： 内核管理

* 升级内核
  * yum update kernel-VER 
  * yum update
* 调整内核参数
  * 使用 `sysctl` 显示内核参数
  * 使用 `sysctl -w` 设置内核参数
    * 如：开启包转发功能 


## 任务13：系统启动过程

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


## 任务14： 禁用一致的网络设备名

* 修改 `/etc/default/grub` 的 `GRUB_CMDLINE_LINUX` 配置行
* 重新生成 grub2 的配置文件
* 重新启动

## 任务15： 基于 tar 的备份

* 完全（Full）备份
  * 将 `/etc /home /srv` 目录下的所有文件打包备份到 `/backup/full-backup.tgz`
  * 将当前完全备份的日期时间戳 (`date +%F`)写入 `/backups/full-backup-date` 文件
* 增量（Incremental）备份
  * 将自昨天 0 点起有变化的数据打包备份到 `/backup/inc-backup-$(date -d yesterday "+%F").txz`
* 差分（Differential）备份
  * 将自完全备份以来有变化的数据打包备份到 `/backup/diff-backup-$(date +%F).tbz`

>* 编写一个每日执行脚本，实现每月一次完全备份，每周一次差分备份，每日一次增量备份，并删除 180 天前创建的备份文件

## 任务16： 基于 rsync 和 tar 的远程备份

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


## 任务17： 基于快照工具（rsnapshot）的备份

**要求：**备份 `/etc/ /home /srv /root` 目录下的所有内容

* 修改 rsnapshot 的配置文件 `/etc/rsnapshot.conf`
  * 假设这些目录都在某个逻辑卷中 
    * 在备份之前，对要备份的目录所在的逻辑卷创建快照
    * 将快照卷挂装到 `/mnt/snap/` 相应的子目录下
    * 备份完毕卸装快照卷并删除快照卷
  * 要求保留 6个小时快照，7 个日快照，4个周快照，6个月快照
* 添加周期性任务配置文件 `/etc/cron.d/rsnapshot` ，配置使之符合备份频率要求


## 任务18： 故障排查

* 找回丢失的 root 口令
* 使用 gpt 分区设备的备份分区表修复其主分区表
* 使用通过 dd 命令备份的 DOS（MBR） 分区表镜像文件修复 DOS 分区表