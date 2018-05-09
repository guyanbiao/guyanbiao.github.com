---
layout: post
title: 从百度地图抓去大学的边界
---

最近一个项目需要大学的地理位置信息，在网上并没有发现相关的库，刚好在百度地图上看到搜一个大学可以有一个范围的蓝线表示出来，就萌生了从百度地图抓去数据的想法

知乎上有个文章给我极大信心和方向上的指导，[《如何获得全国大学的区域边界数据》](http://www.zhihu.com/question/27719852)

1. 获取大学的uid
这个时候大学就是一个兴趣点

{% highlight ruby %}
require "cgi"
result = JSON.parse RestClient.get("http://api.map.baidu.com/place/v2/search?ak=483bcc031ef80a1384950a998908e494&output=json&query=#{CGI.escape "北京大学"}&page_size=10&page_num=0&scope=1&region=#{CGI.escape "全国"}")
@uid = result["results"][0]["uid"]
{% endhighlight %}

2. 获取边界
根据上一步拿到的兴趣点去获取边界信息
{% highlight ruby %}
pr = JSON.parse RestClient.get("http://map.baidu.com/?newmap=1&reqflag=pcmap&biz=1&pcevaname=pc2&da_par=direct&from=webmap&qt=ext&uid=#{@uid}&c=131&ext_ver=new&tn=B_NORMAL_MAP&nn=0&ie=utf-8&l=15&b=(12945800.61,4838078.8495000005;12951968.61,4839350.8495000005)&t=1427876027620")
@geo = pr["content"]["geo"]
{% endhighlight %}

3. 解析数据
上一步的 @geo 是一个字符串，用函数解析一下就可以拿到点了
{% highlight javascript %}
var string = "<%= @geo %>"

arr = parseGeo(string).points.split(";")
points = []
for (var i = 0; i < arr.length; i++) {
  var p = arr[i].split(",")
  points.push(projection.pointToLngLat({x: p[0], y: p[1]}));
}
console.log(points)
{% endhighlight %}

