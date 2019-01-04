
使用md5sum命令对终端当中的字符串进行加密

```shell
[root@zhangyz ~]# echo -n "123456" | md5sum | awk '{print $1}'
e10adc3949ba59abbe56e057f20f883e
```
