---
title: 页面内播放以及播放器闪动的解决方案
layout: post
---

在iPhone 版微信的内置浏览器里面，是可以实现在页面内播放视频的，方法又两种，一种是通过 iframe，另外一种直接的方式是通过设置 video tag 的webkit-playsinline 属性


{% highlight html %}
<iframe frameborder=0 src="http://v.qq.com/iframe/player.html?vid=c0015wqcp3v&tiny=0&auto=0" allowfullscreen></iframe>
{% endhighlight %}
或者

{% highlight html %}
<video src="http://mvvideo2.meitudata.com/55123c0eb1af82919.mp4" width="480" height="480" loop="loop" x-webkit-airplay="" webkit-playsinline="" style="position: static; display: block; opacity: 1; left: 0px; top: 0px; height: 320px; width: 320px;"></video>
{% endhighlight %}

之后需要点击视频停止，可能会发现画面会闪动，在开始或者停止的时候，在美拍的app 里面找了大半天，发现是这个css 属性其作用的

{% highlight css %}
body {
    -webkit-tap-highlight-color: rgba(0,0,0,0);
}
{% endhighlight %}

