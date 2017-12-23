# 教材及勘误

![《Linux 基础及应用教程 （基于CentOS7）》](/assets/figs/covor.jpg)

> To buy 
>
>* <http://item.jd.com/11948553.html>
>* <http://product.dangdang.com/24003094.html>

## 勘误表 （第1次印刷）

### 1、表 6-11 （Page 158）

将表格第五行：

|　      |　                             |           |       |                          |
| ------ | ----------------------------- | --------- | ----- | ------------------------ |
| 服务器 | 使用UDP协议接收发到本机的日志 |  rsyslog  | imudp | $ModLoad imtcp           |
| 　     | 　                            |  　       | 　    | $InputTCPServerRun  514  |

改为：

|　      |　                             |           |       |                          |
| ------ | ----------------------------- | --------- | ----- | ------------------------ |
| 服务器 | 使用UDP协议接收发到本机的日志 |  rsyslog  | imudp | **$ModLoad imudp**       |
| 　     | 　                            |  　       | 　    | **$UDPServerRun 514**    |


### 2、表10-11下的提示框（Page 281）

将如下行： 

    大括号表达式能生成逆序序列，而seq不能

改为：

    大括号表达式能生成逆序序列，seq若生成逆序数字序列需指定负数步长

### 3、操作步骤11.1（Page 294）

将最后的如下行：

    // 6. 在使用此DHCP服务的 CentOS 客户端执行 dhclient -r <interface> 获取IP 
    # dhclient -r eno16777736

改为：

    // 6. 在使用此DHCP服务的 CentOS 客户端执行 dhclient 命令释放并获取IP 
    # dhclient -r eno16777736 ; dhclient eno16777736

### 4、表12-11 （Page 336）

将续表第三行：

    no_root_squash	将root用户或其所属组映射成匿名用户或组，这样设置很不安全不建议使用

改为：

    no_root_squash	不将root用户或其所属组映射成匿名用户或组，这样设置很不安全不建议使用

### 5、操作步骤16.11  （Page 466）

将第五行：

// 使用操作步骤 16.9 创建的服务器自签名证书和私钥文件

改为：

// 使用操作步骤 **16.10** 创建的服务器自签名证书和私钥文件