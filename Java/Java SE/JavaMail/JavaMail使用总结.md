# 问题总结

## 1、端口问题

使用JavaMail发送邮件时如果不指定端口的话默认是通过系统`25`端口发送，而阿里云服务器默认禁用`25`端口，如需使用需要提交申请；

建议使用`465`、`587`端口发送邮件，使用这两个端口需要使用SSL协议，而高版本的JDK默认是禁用SSL协议的，Linux环境下需要在文件：jdk安装目录/conf/security/java.security中修改以下内容：

```shell
jdk.tls.disabledAlgorithms=   SSLv3,TLSv1,TLSv1.1,RC4, DES, MD5withRSA, \
DH keySize < 1024, EC keySize < 224, 3DES_EDE_CBC, anon, NULL
# 将  SSLv3,TLSv1,TLSv1.1 这几个协议删除，若还不生效可以将这段字符全部删掉；
```

注意：修改后务必**重启系统**让配置生效！！！！！！！！！！！！



