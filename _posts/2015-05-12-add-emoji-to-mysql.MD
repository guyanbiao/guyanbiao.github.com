---
layout: post
title: 为mysql 添加emoji支持
---

Mysql 从5.5.3 开始支持 utf8mb4 编码，用 4 个 byte 作为一个字符，utf8 编码是3个 bype 的，存emoji就溢出了.

1. 更新mysql(以centos 为例)

{% highlight bash %}
wget file:///Users/neal/Downloads/mysql-community-release-el6-5.noarch.rpm
sudo yum localinstall mysql-community-release-el6-5.noarch.rpm
sudo yum update mysql-server
{% endhighlight %}

2. 重启 mysql 服务

3. 添加编码到 `/usr/share/mysql/charsets/Index.xml`

{% highlight xml %}
<charset name="utf8mb4">
  <family>Unicode</family>
  <description>UTF-8 Unicode</description>
  <alias>utf-8</alias>
  <collation name="utf8mb4_unicode_ci"  id="99">
   <flag>primary</flag>
   <flag>compiled</flag>
  </collation>
  <collation name="utf8_bin"            id="100">
    <flag>binary</flag>
    <flag>compiled</flag>
  </collation>
</charset>
{% endhighlight %}

4. 修改 Rails 的数据库配置

{% highlight yaml %}
  adapter: mysql2
  encoding: utf8mb4
  reconnect: false
  database: feixiang_production
  pool: 5
  username: user
  password: password
{% endhighlight %}

