---
layout: post
title: "Wowza 的分布"
date: 2013-10-31 16:27:31
---

<img src="/images/wowzajiagou.jpg">

现在应用情况是一个流发不出去，要大量的用户观看，这样的话就需要用到 [liverepeater](http://www.wowza.com/forums/content.php?227-Liverepeater-Quick-Start) 把流分布出去。按照 liverepeater 的配置基本就可以了。需要注意的几点:

* 如果要用 iphone 观看的话，在 liveorigin 的 LiveStreamPacketizers 要配置成

<pre>
  <code class="xml">
    <LiveStreamPacketizers>cupertinostreamingpacketizer, smoothstreamingpacketizer, sanjosestreamingpacketizer</LiveStreamPacketizers>
  </code>
</pre>


