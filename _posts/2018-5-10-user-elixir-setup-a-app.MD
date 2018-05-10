---
layout: post
title: 一步一步用 elixir 搭建一个 obd 的 socket server
---

# 应用场景

obd 设备要通过 socket 把数据发送到服务器, 然后把数据存储到数据库，要求服务器可以通过 socket 下发 指令。

ruby 虽然有一个基于 actor 的框架 celluloid(三六劳艾的), 但是用下来发现性能还是不够，尤其是遭遇移动运行商出现网络问题或者重启服务，导致设备集体重连的时候，会带来比较大的冲击，所以后来决定干脆采用 elixir 本尊来尝试一下

待续...
