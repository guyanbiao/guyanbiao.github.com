---
layout: post 
title: Bootstrap 中 tab 的写法
---

code:

```
<ul class="nav nav-tabs" >
    <li>
        <a href="#abc" data-toggle="tab">abc</a>
    </li>
    <li>
        <a href="#bcd" data-toggle="tab">bcd</a>
    </li>
</ul>

<div class="tab-content">
  <div class="tab-pane fade" id="abc">
  alalal
  </div>
  <div class="tab-pane fade active in" id="bcd">
  alalal
  </div>
</div>
```
