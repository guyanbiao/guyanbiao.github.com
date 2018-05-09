---
layout: post
title: 用日志文件恢复 mysql 的误操作 
---

参见这篇[文章](http://blog.csdn.net/yhjsspz/article/details/17360809)

数据库操作真是危险，有备无患啊！！！

mysql 的数据文件默认在 /var/lib/mysql, 里面有类似 mysql-bin.00000X 的文件就是 mysql的日志了，先把日志文件转成 sql，然后导入就大功告成

`mysqlbinlog  --start-date="2013-10-01 00:00:00" --stop-date="2013-12-12 12:00:00" mysql-bin.000067 > da_nan_bu_si_bi_you_hou_fu.sql`
