DNS (域名解析服务)

SSL/TSS：http-----https      Openssl，CA，Digital Certificater（数字证书）

bind    --------  软件

HTTP: Apache, LAMP, LNMP 

CDN: DNS, varnish

FILE server: NFS, SMB, CIFS, FTP

Netfilter：iptables，tcpwrapper

NSSwitch：framework，platform ， pam（插入式认证模块）
SMTP/POP3/IMAP4：Mail Server

邮件服务器
selinux：Security  Enhanced  Linux
C2-------SElinux-------B1




DNS:Domain Name Service
域名：www.magedu.com（主机名，FQDN，Qualified Domain Name）        
完全限定域名
DNS：名称解析Name Resolving
名称转换，背后有查询的过程，所以被称为名称解析


能够将DNS转换成为IP地址的
libnss_files.so
libnss_dns.so


调用的库文件
/etc/nsswitch.conf               

FQDN----IP          双向转换
172.16.0.1          www.theif.com
172.16.0.2          mail.magedu.com


dns：DNS

stub resolver：名称解析器

ping www.magedu.com



hosts
IPADDR          FQRN           Ailases
172.16.0.1     www.magedu   www

A-------D
hosts

1.周期性任务

2.Server，Server 
1KW：

3.分布式数据库

ICANN：





TLD：
组织域：.com .org .net .cc
国家域：.cn   .tw   .hk  .iq   .ir   .jp
反向域：IP----FQDN----       
反向：IP----FQDN           单独的一种数据库
正向：FQDN-----IP


两段式：递归，迭代

DNS：分布式数据库
上级仅知道与其直接下级
下级只知道根，默认情况下只知道根的位置（需要自己配置）


DNS服务器：
接受本地客户端的查询请求（递归）
来自外部客户端的请求是请求权威答案的
肯定答案：TTL
否定答案：TTL
外部客户端请求：非权威答案


hosts：
IPADDR          FQDN                        Ailases
172.16.0.1     www.puppet.com       www


1.周期性任务

2.Server：


IANA： IP，FQDN，          名称地址分配机构

ICANN： 地址分配机构（民间），用于管理IP地址的分配





根DNS    有13台


DNS服务器类型
主，  从
主DNS服务器；数据修改
辅助DNS服务器，请求数据同步

serial number
refresh
retry
expire
nagative answer TTL
