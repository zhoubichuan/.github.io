---
title: linux-8.网络管理
copyright: true
permalink: 1
top: 0
date: 2019-06-07 10:06:23
categories:
- 前端自动化运维
- linux
- 网路管理
tags:
---
## 1.配置ip地址
### 1.1ifconfig临时配置IP
- 查看与配置网路状态
- 临时设置eth0网卡的IP地址与子网掩码
```
[root@localhost cron.daily]# ifconfig
eth0 Link encap:Ethernet HWaddr 00:0C:29:E5:3C:11
    inet addr:127.18.0.252 Bcast:172.18.3.255 Mask:255.255.0
```
```
ifconfig eth0 172.18.0.254 netmask 255.255.0
```
### 1.2setup永久配置IP
```
setup
service network restart
```
## 2.网络配置文件
### 2.1网卡配置
/etc/sysconfig/network-scripts/ifcfg-eth0
```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```
### 2.2主机名
/etc/sysconfig/network
```
NETWORKING=yes 网络功能是否起作用
HOSTNAME=localhost.localdomain 主机名

hostname zhufengjiagou
service network restart
```
### 2.3DNS配置文件
```
#cat /etc/resolv.conf
nameserver 8.8.8.8 DNS服务器
search localhost
nameserver 8.8.8.8
```
## 3.查看网络环境
### 3.1ifconfig
- 查看与配置网络状态命令
- ifconfig看不到网关和DNS `ipconfig/all`
```
物理地址
IPv4 地址
子网掩码
默认网关
DHCP 服务器
DNS 服务器
```
### 3.2关闭和启动网卡
- 禁用该网卡设备 `ifdown 网卡设备名`
- 启用该网卡设备 `ifup 网卡设备名`
### 3.3查询网络状态
- netstat选项
选项 | 含义
-t |列出TCP协议端口
-u | 列出UDP协议端口
-n | 不使用域名与服务名，而使用IP地址和端口号
-l|仅列出在监听状态网络服务
-a|列出所有的网络连接
```
netstat -tlun
netstat -an |more
netstat -unt | grep ESTABLISHED
```
### 3.4netstate -rn
- -r列出路由列表，功能和`route`命令一致
### 3.5域名解析命令
- nslookup [主机名或IP]
- 进行域名与IP地址解析
- 查看本机的DNS服务器
## 4.网络测试命令
### 4.1ping
- ping[选项]ip或域名
- 测试指定IP或域名的网络状况
- 选项
    - -c次数指定ping包的次数
```
ping www.baidu.com -c 3
```
### 4.2traceroute[选项]IP或域名
- 路由跟踪命令
- 选项
    - -n 使用IP,不使用域名，速度更快
### 4.3wget
- 下载命令
```
wget http://www.baidu.com
```
### 4.4tcpdump命令
- tcpdump -i eth0-nnX port 21
- 选项
    - -i 指定网卡接口
    - -nn将数据包中的域名与服务转为IP和端口
    - -X以十六进制ASCII码显示数据包内容
    - port指定监听的端口
## 5.远程登录
### 5.1SSH协议原理
### 5.2对称加密算法
- 采用单密钥系统的加密方法，同一个密钥可以同时用作信息的加密和解密，这种加密被称为对称加密
- 非对称加密算法需要公钥和私钥