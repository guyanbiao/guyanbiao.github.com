---
layout: page
title:  RESUME
permalink: /resume/
---

## 工作经历
###  【北京】 北京云拓瑞联科技有限公司 2013.2 - 2015.5
公司主要产品是一个直播产品，面向用户提供服务，直播的终端可以是专业的设备或者电脑，手机也可以，通过 rtmp 协议推送到流媒体服务器上，主要的工作内容是 <span class="emphasize">Rails</span> 搭建网站，提供 api 给手机。

同时承担了运维的角色，当时利用 azure 提供的动态伸缩能力，动态调整虚拟机实例个数，来应对突然的高并发同时节省成本。

有部分并发比较高的页面用了静态页面加 <span class="emphasize">Node.js</span> 提供 api 的方式来应对

另外一个项目给中国人寿车险使用，是一套呼叫系统，用到了 <span class="emphasize">xmpp</span>，实现了一个类似视频呼叫中心的系统，定损员发起视频，后台的坐席应答，进行单向视频和双向语音的交流。由于要保证长连接，使用了 SPA，前端用 <span class="emphasize">Angular</span>

当时的 ruby 工程师是 2 人
###  【广州】 深圳车事易汽车科技有限公司 2015.6 - 今
公司有线下业务，主要是给企业客户停供车辆维修养护保险服务，主要客户是瑞卡租车、首汽租车这样的租车公司，系统提供一套客户下单，技师维修的整个的流程的支持，包括财务、仓储，我参与构建网站，api，实现了第一个版本的技师端，用的框架是 <span class="emphasize">React</span>。

期间也用 <span class="emphasize">React Native</span> 做了一个 app，适配 Android 和 iOS。也用微信<span class="emphasize">小程序</span>实现了一个监控功能的 app。

比较重要的一个模块是 obd( 车载诊断系统 )，合作硬件厂商提供硬件，制定二进制的私有通讯协议，通过 socket 的方式把车辆数据发送到 服务器，第一个版本是用 celluoid 加 Active Record 实现，存储过程也是 ruby 脚本，用 <span class="emphasize">Redis</span> 的 list 做队列，数据库用 Postgres 第二个版本的 socket 服务器使用 <span class="emphasize">elixir</span> 实现的，存储过程用了 <span class="emphasize">go</span> 和 ruby，数据库使用 <span class="emphasize">Postgres</span> 加上阿里云提供的<span class="emphasize">表格存储</span>

17年下半年开始承担管理职责，用 tower 管理项目的进度，协调工作，ruby 团队 5 - 8 人，前端工程师一个。


## 教育
2008-2012 广州中医药大学  制药工程 4年制


## 技能
<pre>
Ruby                    * * * * 
Rails                   * * * *
Git                     * * * *
Postgres                * * *
Elixir                  * *
Go                      * *
javascript              * * *
React                   * * *
React Native            * * *
wechat applet           * * 
MiniTest                * * 
Linux                   * * *
</pre>


## 个人介绍
喜欢打 dota，模拟赛车，卡丁车，滑雪，游泳。

跟同事沟通比较顺畅，学习能力比较强，能快速应用到实践中。很能站在用户的角度看产品，善于从用户角度出发把控产品质量。

希望公司有简单高效的管理方式，同事比较 nice，可以让我长期工作下去，有所作为。

接受 ruby go elixir 和 前端的工作，我经验主要以后端为主，前端也可以胜任

## 链接

[github pages](https://guyanbiao.github.io)

[简书 ](https://www.jianshu.com/u/635469050a0c)

[github](https://github.com/guyanbiao)

[ruby-china](https://ruby-china.org/guyanbiao)
