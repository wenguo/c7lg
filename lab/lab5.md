# 实验指导 5 - 网络配置与防火墙设置

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
> * 防火墙
>   * 使用 `firewall-cmd` 或 `firewall-config` 工具配置基于 firewalld 守护进程的防火墙
>   * 使用 `lokkit` 或`system-config-firewall-tui` 工具配置基于 iptables 服务的防火墙
>   * 编写基于 `iptabls` 命令的防火墙脚本
>   * 编写基于 `iptables-save` 命令输出文件格式的防火墙脚本


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



## 任务6：配置 CentOS 6 的网络连接

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


## 任务7：配置 Debian 9 的网络连接

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


## 任务8：firewalld 防火墙 -- 主机防火墙（单网卡）

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


## 任务9：基于 2222 端口的 SSH 服务的 firewalld  防火墙

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

## 任务10：firewalld 防火墙 -- 网关防火墙（多网卡）

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

## 任务11：iptables 防火墙

1. 关闭 firewalld 并切换使用基于 iptables 服务的防火墙
   * 关闭并禁用 `firewalld`
   * 安装 `iptables-services` 和 `system-config-firewall-tui`
   * 启动并设置开机启动 `iptables` 服务
2. 配置  `iptables` 防火墙
   * 直接修改规则集文件 `/etc/sysconfig/iptables` 配置 
   * 使用  `system-config-firewall-tui` 或 `lokkit` 配置
3. 重新加载防火墙配置

>**提示** 你也可以在  c6-v1 容器上练习配置 iptables 主机防火墙


## 任务12：iptables 脚本

* 基于 `iptables-save` 命令输出文件格式的防火墙脚本
  * **参考** https://github.com/fcaviggia/hardened-centos7-kickstart/blob/master/config/hardening/iptables.sh
* 基于 `iptabls` 命令的防火墙脚本
  * **参考** http://easyfwgen.morizot.net/gen/index.php 
  * **参考** http://www.malibyte.net/iptables/scripts/fwscripts.html




