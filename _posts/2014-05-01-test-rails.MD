---
layout: post
title: Rails 测试入门
---

1. 准备测试数据库 

`rake db:test:prepare`

2. 遇到的坑
以前的版本的 rspec 的 expect 的写法是 expect 不跟参数的，跟一个 block，新的是可以跟参数的
