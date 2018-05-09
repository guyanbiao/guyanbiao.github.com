---
layout: post
title: Rails 配置 mysql的远程链接
---

记录第一次配置 mysql 远程链接的问题，主要参考这篇[文章](http://www.cyberciti.biz/tips/how-do-i-enable-remote-access-to-mysql-database-server.html)

###1. 更改 mysql 配置

`vi /etc/my.cnf`

把文件里面的 `skip-networking` 去掉，这个是把监听端口禁止，当你根本不允许外部链接的时候可以用这个。

然后加入

`bind-address=202.54.10.20`

server-ip 要填的就是 mysql sever 所在的那台机器的 ip

###2. 重启 msyql

###3. 用 root 用户登录设置远程访问权限

```
 mysql -u root -p mysql
 mysql> CREATE DATABASE foo;
 mysql> GRANT ALL ON *.* TO bar@'10.5.1.3' IDENTIFIED BY 'PASSWORD';
```

这里需要说的是 10.5.1.3 是这你最终用来在远端登录 mysql client 的那台机器的 ip

###4. 配置防火墙规则
在这一步的时候要可以先把防火情停掉，

`sudo /etc/init.d/iptables top`

然后看看从远程的机器上的 mysql 客户端能不能连上来，

`mysql --host=202.54.10.20 --user=mysql --port=3306 -p`

不行的话返回头检查，可以的话在继续下面：

`/sbin/iptables -A INPUT -i eth0 -s 10.5.1.3 -p tcp --destination-port 3306 -j ACCEPT`

这句话是说接受 eth0 这个网卡上来自 10.5.1.3 的 对于 3306 这个端口的访问，如果加了 iptables 就连不上的话，就是 iptables 的规则有问题了，还是问题 google 吧


