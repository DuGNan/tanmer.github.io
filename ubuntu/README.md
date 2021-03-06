# Ubuntu

## 修改apt源

```bash
sudo sed -i 's!http://us.archive.ubuntu.com!https://mirrors.tuna.tsinghua.edu.cn!' /etc/apt/sources.list
```

## 配置

### 允许root远程登录

Ubuntu默认是不允许root远程登录的，我们可以修改`PermitRootLogin`的值，改变root登录方式。

```text
vi /etc/ssh/sshd_config
```

```text
PermitRootLogin yes # 允许root用密码和Public Key方式登录
PermitRootLogin prohibit-password # 
PermitRootLogin without-password # 只允许root用public key认证方式登录
PermitRootLogin no # 不允许root登录
```

## 翻墙技巧

### 傻瓜式

[https://github.com/googlehosts/hosts](https://github.com/googlehosts/hosts)

下载hosts文件，内容添加到`/etc/hosts`

### 进阶式

下载上面仓库的[dnsmasq.conf](https://github.com/googlehosts/hosts/raw/master/hosts-files/dnsmasq.conf)文件，安装dnsmasq服务

```bash
sudo apt-get install dnsmasq
sudo mv ~/dnsmasq.conf /etc/dnsmasq.conf
```

## 软件

### arp

> 查看局域网内IP的Mac地址

```bash
$ arp
Address                  HWtype  HWaddress           Flags Mask            Iface
10.10.0.11               ether   00:04:ff:ff:ff:d0   C                     eth0
10.10.0.16               ether   00:04:ff:ff:ff:a6   C                     eth0
raspbmc.local            ether   00:1f:ff:ff:ff:9c   C                     eth0
10.10.0.19               ether   00:04:ff:ff:ff:c9   C                     eth0
10.10.0.12               ether   bc:f5:ff:ff:ff:93   C                     eth0
10.10.0.17               ether   00:04:ff:ff:ff:57   C                     eth0
10.10.0.1                ether   20:4e:ff:ff:ff:30   C                     eth0
HPF2257E.local           ether   a0:b3:ff:ff:ff:7e   C                     eth0
10.10.0.15               ether   00:04:ff:ff:ff:b9   C                     eth0
tim                      ether   00:22:ff:ff:ff:af   C                     eth0
10.10.0.13               ether   60:be:ff:ff:ff:e0   C                     eth0
```

### iperf

> 带宽测试

```bash
apt install iperf
```

测试方法：准备2台 Linux 云主机（例如CentOS 6.5），一台做server，一台做client，云主机推荐配置为 2核4G

server端命令如下：

```bash
iperf -s
```

client端命令如下：

```text
iperf -c 10.x.x.x(server内网IP) -i 2 -t 30 -P 50
```

### ping

> 检查IP知否联网

```bash
apt install iputils-ping -y
```

### ps

> 查看进程

```bash
apt install procps -y
```

### nmap

> 查看局域网内IP和Mac地址信息

```bash
apt install nmap
```

```bash
root@192-168-1-141:~# nmap -sn 192.168.1.0/24

Starting Nmap 7.01 ( https://nmap.org ) at 2018-04-25 16:52 CST
Nmap scan report for 192.168.1.1
Host is up (0.00046s latency).
MAC Address: FA:FF:FF:FF:FF:FF (Unknown)
Nmap scan report for 192.168.1.127
Host is up (-0.099s latency).
MAC Address: 52:54:00:66:42:AF (QEMU virtual NIC)
Nmap scan report for 192.168.1.185
Host is up (-0.100s latency).
MAC Address: 52:54:00:3F:18:04 (QEMU virtual NIC)
Nmap scan report for 192.168.1.128
Host is up.
Nmap scan report for 192-168-1-141 (192.168.1.141)
Host is up.
Nmap done: 256 IP addresses (5 hosts up) scanned in 2.29 seconds
```

### nslookup

> 检查DNS解析

```bash
apt install dnsutils -y
```

```bash
root@192-168-1-141:~# nslookup www.tanmer.com 8.8.8.8
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
Name:	www.tanmer.com
Address: 120.132.67.49
```

### mail

```bash
apt install mailutils -y
```

### sudo

配置sudo用户不需要密码

```bash
sudo sudovi
```

在打开的文件结尾添加如下内容（注意，一定是文件结尾，否则可能不成功）：

```text
ubuntu ALL=(ALL) NOPASSWD:ALL
```

重新登录，测试一下sudo应该不会出现密码提示。

