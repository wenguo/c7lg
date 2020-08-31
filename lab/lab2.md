# 实验指导 2 - Linux用户管理与进程管理

>#### 学习目标
> * 使用 `useradd`/`usermod`/`userdel` 命令 添加/修改/删除 用户
> * 使用 `groupadd`/`groupmod`/`groupdel` 命令 添加/修改/删除 组
> * 使用 `id` / `group`命令显示 用户/组 信息
> * 使用 `su` / `sg` 或`newgrp` 命令切换 用户/组
> * 使用 `passwd` 命令管理用户口令
> * 使用 `gpasswd` 命令管理组口令和组成员
> * 使用 `chage` 命令设置已存在用户的口令时效策略
> * 修改 `/etc/login.defs` 文件为新建用户设置口令时效策略
> * 使用 `chmod` 命令设置文件/目录的 基本/特殊 权限
> * 使用 `chown`/`chgrp` 更改文件/目录的 属主/同组人
> * 使用 `umask` 设置文件和目录的生成掩码
> * 使用 `getfacl`/`setfacl` 显示/设置 文件/目录的 FACL 权限
> * 进程管理 
> * 进程查看、显示命令的执行时间
> * 杀死、调整运行优先级
> * 在后台执行进程，注销后保证进程继续执行
> * 实施作业控制


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

## 任务7：权限管理

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


## 任务8：进程管理

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




