通过curl的-w参数我们可以自定义curl的输出 %{http_code}代表http状态码
```shell
[root@zhangyz ~]# curl -I -m 10 -o /dev/null -s -w %{http_code} www.111cn.net
```

上面的输出是不含换行的如果需要换行的话加上 \n
```shell
[root@zhangyz ~]# curl -I -m 10 -o /dev/null -s -w %{http_code} www.111cn.net
200[root@zhangyz ~]# 
[root@zhangyz ~]# curl -I -m 10 -o /dev/null -s -w %{http_code}"\n"  www.111cn.net
200
```

curl命令

<br>

1) -a/--append 上传文件时附加到目标文件

2) -A/--user-agent <string> 设置用户代理发送给服务器

3) -anyauth 可以使用 "任何" 身份验证方法

4) -b/--cookie <name=string/file> cookie字符串或文件读取位置

5) -basic 使用HTTP基本验证

6) -B/--use-ascii 使用ASCII /文本传输

7) -c/--cookie-jar <file> 操作结束后把cookie写入到这个文件中

8) -C/--continue-at <offset> 断点续转

9) -d/--data <data> HTTP POST方式传送数据

10) --data-ascii <data> 以ascii的方式post数据
11) --data-binary <data> 以二进制的方式post数据
12) --negotiate 使用HTTP身份验证
13) --digest 使用数字身份验证
14) --disable-eprt 禁止使用EPRT或LPRT
15) --disable-epsv 禁止使用EPSV
16) -D/--dump-header <file> 把header信息写入到该文件中
17) --egd-file <file> 为随机数据(SSL)设置EGD socket路径
18) --tcp-nodelay 使用TCP_NODELAY选项
19) -e/--referer 来源网址
20) -E/--cert <cert[:passwd]> 客户端证书文件和密码 (SSL)
21) --cert-type <type> 证书文件类型 (DER/PEM/ENG) (SSL)
22) --key <key> 私钥文件名 (SSL)
23) --key-type <type> 私钥文件类型 (DER/PEM/ENG) (SSL)
24) --pass <pass> 私钥密码 (SSL)
25) --engine <eng> 加密引擎使用 (SSL). "--engine list" for list
26) --cacert <file> CA证书 (SSL)
27) --capath <directory> CA目录 (made using c_rehash) to verify peer against (SSL)
28) --ciphers <list> SSL密码
29) --compressed 要求返回是压缩的形势 (using deflate or gzip)
30) --connect-timeout <seconds> 设置最大请求时间
31) --create-dirs 建立本地目录的目录层次结构
32) --crlf 上传是把LF转变成CRLF
33) -f/--fail 连接失败时不显示http错误
34) --ftp-create-dirs 如果远程目录不存在，创建远程目录
35) --ftp-method [multicwd/nocwd/singlecwd] 控制CWD的使用
36) --ftp-pasv 使用 PASV/EPSV 代替端口
37) --ftp-skip-pasv-ip 使用PASV的时候,忽略该IP地址
38) --ftp-ssl 尝试用 SSL/TLS 来进行ftp数据传输
39) --ftp-ssl-reqd 要求用 SSL/TLS 来进行ftp数据传输
40) -F/--form <name=content> 模拟http表单提交数据
41) -form-string <name=string> 模拟http表单提交数据
42) -g/--globoff 禁用网址序列和范围使用{}和[]
43) -G/--get 以get的方式来发送数据
44) -h/--help 帮助
45) -H/--header <line>自定义头信息传递给服务器
46) --ignore-content-length 忽略的HTTP头信息的长度
47) -i/--include 输出时包括protocol头信息
48) -I/--head 只显示文档信息
49) 从文件中读取-j/--junk-session-cookies忽略会话Cookie
50) - 界面<interface>指定网络接口/地址使用
51) -krb4 <级别>启用与指定的安全级别krb4
52) -j/--junk-session-cookies 读取文件进忽略session cookie
53) --interface <interface> 使用指定网络接口/地址
54) --krb4 <level> 使用指定安全级别的krb4
55) -k/--insecure 允许不使用证书到SSL站点
56) -K/--config 指定的配置文件读取
57) -l/--list-only 列出ftp目录下的文件名称
58) --limit-rate <rate> 设置传输速度
59) --local-port<NUM> 强制使用本地端口号
60) -m/--max-time <seconds> 设置最大传输时间
61) --max-redirs <num> 设置最大读取的目录数
62) --max-filesize <bytes> 设置最大下载的文件总量
63) -M/--manual 显示全手动
64) -n/--netrc 从netrc文件中读取用户名和密码
65) --netrc-optional 使用 .netrc 或者 URL来覆盖-n
66) --ntlm 使用 HTTP NTLM 身份验证
67) -N/--no-buffer 禁用缓冲输出
68) -o/--output 把输出写到该文件中
69) -O/--remote-name 把输出写到该文件中，保留远程文件的文件名
70) -p/--proxytunnel 使用HTTP代理
71) --proxy-anyauth 选择任一代理身份验证方法
72) --proxy-basic 在代理上使用基本身份验证
73) --proxy-digest 在代理上使用数字身份验证
74) --proxy-ntlm 在代理上使用ntlm身份验证
75) -P/--ftp-port <address> 使用端口地址，而不是使用PASV
76) -Q/--quote <cmd>文件传输前，发送命令到服务器
77) -r/--range <range>检索来自HTTP/1.1或FTP服务器字节范围
78) --range-file 读取（SSL）的随机文件
79) -R/--remote-time 在本地生成文件时，保留远程文件时间
80) --retry <num> 传输出现问题时，重试的次数
81) --retry-delay <seconds> 传输出现问题时，设置重试间隔时间
82) --retry-max-time <seconds> 传输出现问题时，设置最大重试时间
83) -s/--silent静音模式。不输出任何东西
84) -S/--show-error 显示错误
85) --socks4 <host[:port]> 用socks4代理给定主机和端口
86) --socks5 <host[:port]> 用socks5代理给定主机和端口
87) --stderr <file>
88) -t/--telnet-option <OPT=val> Telnet选项设置
89) --trace <file> 对指定文件进行debug
90) --trace-ascii <file> Like --跟踪但没有hex输出
91) --trace-time 跟踪/详细输出时，添加时间戳
92) -T/--upload-file <file> 上传文件
93) --url <URL> Spet URL to work with
94) -u/--user <user[:password]>设置服务器的用户和密码
95) -U/--proxy-user <user[:password]>设置代理用户名和密码
96) -v/--verbose
97) -V/--version 显示版本信息
98) -w/--write-out [format]什么输出完成后
99) -x/--proxy <host[:port]>在给定的端口上使用HTTP代理
100) -X/--request <command>指定什么命令
101) -y/--speed-time 放弃限速所要的时间。默认为30
102) -Y/--speed-limit 停止传输速度的限制，速度时间'秒
103) -z/--time-cond 传送时间设置
104) -0/--http1.0 使用HTTP 1.0
105) -1/--tlsv1 使用TLSv1（SSL）
106) -2/--sslv2 使用SSLv2的（SSL）
107) -3/--sslv3 使用的SSLv3（SSL）
108) --3p-quote like -Q for the source URL for 3rd party transfer
109) --3p-url 使用url，进行第三方传送
110) --3p-user 使用用户名和密码，进行第三方传送
111) -4/--ipv4 使用IP4
112) -6/--ipv6 使用IP6
113) -#/--progress-bar 用进度条显示当前的传送状态
