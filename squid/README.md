
# squid的服务配置

<br>

<br>


1) 实现需求, 公司通过防火墙提供上网服务, 但是, 由于员工上网下载网络上一些不必要的文件, 所以, 需要对某些特定网站做一些特殊的处理
2) 让内网用户们通过接入交换机开始进行上网, 需要, 用户不能在上班的时间访问淘宝, 京东, 玩游戏或者视频网站
3) 由于防火墙是核心设备, 所以, 需要考虑防火墙的性能。
4) 示例如下

![squid](pic/squid01.jpg)

<br>

#### 使用方式为squid透明代理模式对内网用户做上网的访问控制策略

1) 挑选squid源码安装包 squid-3.0.STABLE18.tar.gz

2) 创建squid的非登录式用户
```shell
[root@zhangyz ~]# useradd -M -s /sbin/nologin squid   // -M 表示不创建家目录
```

3) 解压源码安装包进入源码编译安装目录
```shell
[root@zhangyz ~]# tar xf squid-3.0.STABLE18.tar.gz -C /usr/src/squid-package
[root@zhangyz ~]# cd /usr/src/squid-package
```

4) 指定编译安装的参数
```shell
[root@zhangyz squid-package]# ./configure --prefix=/usr/local/squid \
--sysconfdir=/etc \
--enable-linux-netfilter \
--enable-linux-tproxy \
--enable-async-io=100 \
--enable-err-language="Simplify_Chinese" \
--enable-underscore \
--enable-poll \ 
--enable-gnuregex \
--enable-arp-acl
```

5) 上述含义如下
```shell
--prefix 
--sysconfdir=/etc		指定配置文件的存放位置 
--enable-linux-netfilter	使用内核进行过滤
--enable-linux-tproxy		支持透明模式
--enable-arp-acl		可以直接在规则中通过客户端MAC地址进行管理
--enable-async-io=100		提升存储性能值一般都为100
--enable-err-language="Simplify_Chinese"	错误信息显示为中文
--enable-underscore    		允许url路径存在下划线
--enable-poll 			使用poll()模式, 提升性能
--enable-gnuregex		使用正则表达式
```

6) 执行完成之后
```shell
[root@zhangyz squid-package]# make && make install	// 如果报错就按照提示排错
```

7) 由于程序用户不需要宿主目录以及不需要登录操作系统, 所以添加 -M -s 选项现在我们来将他的执行程序链接到/usr/local/sbin目录下, 方便命令的执行或者直接修改PATH环境变量这里我选择修改PATH环境变量
```shell
[root@zhangyz squid-package]# echo "PATH=$PATH:/usr/local/squid/sbin/" >> /etc/profile &&
[root@zhangyz squid-package]# . /etc/profile
```

8) 之后我们将squid安装目录下的var子目录的属组和属主改变
```shell
[root@zhangyz ~]# chown -R squid:squid /usr/local/squid/var
```

9) 会输出很多信息只要细看一下有没有出现错误就可以了
```shell
[root@zhangyz ~]# squid -k parse  
```

10) 语法没有任何错误那么首先对缓存目录进行初始化
```shell
[root@zhangyz ~]# squid -z
```

11) 正确执行了之后我们启动squid
```shell
[root@zhangyz ~]# ssquid
```

12) 查看squid是否正常启动
```shell
[root@zhangyz ~]# netstat -tunlp | grep "squid"
tcp 0 0  :::3128 ::: * LISTEN 37749/(squid-1)
```

13) 可以看见squid服务已经启动了为了更好的控制squid的服务现在编写一个脚本来进行控制
```shell
[root@zhangyz ~]# vim /tools/squid
#!/bin/bash
# chkconfig: 2345 90 25
# config: /etc/squid.conf
# pidfile: /usr/local/squid/var/run/squid.pid
# Description: Squid - internet object cache.

PID="/usr/local/squid/var/logs/squid.pid"
CONF="/etc/squid.conf"
CMD="/usr/local/squid/sbin/squid"

case "$1" in
    start)
        netstat -anpt | grep squid &> /dev/null
        if [ $? -eq 0 ]; then
            echo "squid is running"
        else
            echo "正在启动squid..."
            $CMD
        fi
    ;;
    
    stop)
        $CMD -k kill &> /dev/null
        rm -fr $PID &> /dev/null
    ;;
    
    status)
        [ -f $PID ] &> /dev/null
        if [ $? -eq 0 ]; then
            netstat -anpt | grep squid
        else
            echo "squid is not running."
        fi
    ;;
    
    restart)
        $0 stop &> /dev/null
        echo "正在关闭squid..."
        $0 start &> /dev/null
        echo "正在启动squid..."
    ;;
    
    reload)
        $CMD -k reconfigure
    ;;
    
    check)
        $CMD -k parse
    ;;

    *)
        echo "用法:$0 {start | stop | restart | reload | check | status}"
    ;;
esac
```

14) 之后将squid脚本添加为系统服务并设置每次开机自动运行
```shell
[root@zhangyz ~]# cp /tools/squid /etc/init.d/
[root@zhangyz ~]# chmod +x /etc/init.d/squid 
[root@zhangyz ~]# chkconfig --add squid
[root@zhangyz ~]# chkconfig squid on
[root@zhangyz ~]# service squid start
squid is running
[root@zhangyz ~]# service squid restart
正在关闭squid...
正在启动squid...
```

15) 使用curl 命令对网络进行测试
```shell
[root@zhangyz ~]# curl -I -m 10 -o /dev/null -s -w %{http_code}"\n" www.letuknowit.com www.baidu.com
```

linux下的高并发的squid服务器,TCP TIME_WAIT套接字数量经常达到两,三万服务器容易被拖死通过修改linux内核参数可以减少squid服务器的TIME_WAIT套接字数量

```shell
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 1
net.ipv4.ip_local_port_range = 1024    65000
net.ipv4.tcp_max_syn_backlog = 8192
net.ipv4.tcp_max_tw_buckets = 5000
```

说明
```shell
net.ipv4.tcp_syncookies = 1 表示开启SYN Cookies。当出现SYN等待队列溢出时，启用cookies来处理，可防范少量SYN攻击，默认为0，表示关闭；
net.ipv4.tcp_tw_reuse = 1 表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭；
net.ipv4.tcp_tw_recycle = 1 表示开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，表示关闭。
net.ipv4.tcp_fin_timeout = 30 表示如果套接字由本端要求关闭，这个参数决定了它保持在FIN-WAIT-2状态的时间。
net.ipv4.tcp_keepalive_time = 1200 表示当keepalive起用的时候，TCP发送keepalive消息的频度。缺省是2小时，改为20分钟。
net.ipv4.ip_local_port_range = 1024    65000 表示用于向外连接的端口范围。缺省情况下很小：32768到61000，改为1024到65000。
net.ipv4.tcp_max_syn_backlog = 8192 表示SYN队列的长度，默认为1024，加大队列长度为8192，可以容纳更多等待连接的网络连接数。 
net.ipv4.tcp_max_tw_buckets = 5000 表示系统同时保持TIME_WAIT套接字的最大数量，如果超过这个数字，TIME_WAIT套接字将立刻被清除并打印警告信息。默认为180000，改为5000。对于Apache、Nginx等服务器，上几行的参数可以很好地减少TIME_WAIT套接字数量，但是对于Squid，效果却不大。此项参数可以控制TIME_WAIT套接字的最大数量，避免Squid服务器被大量的TIME_WAIT套接字拖死。
```

<br>

执行以下命令使配置生效
```shell
[root@zhangyz ~]# /sbin/sysctl -p
```
