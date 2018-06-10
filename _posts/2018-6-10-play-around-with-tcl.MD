---
title: Tcl 超快速入门
layout: post
---

# 概述

Tcl 跟 ruby 一样 是个脚本语言，在最新的 8.6 提供了对象系统，还提供了 coroutine, 跟 ruby 的 fiber 一样。也提供了内建的的 event loop 编程模型，像 eventmachine.  比较特别的是提供了一个自带的 UI 页面，叫 tk，可以快速搭建UI 界面，感觉有点像 visual basic，不过是跨平台，我在 mac 上试了下对 Retina 支持的不是很好。不过有解决方案。

mac 上有安装包，可以安装最新的。如果用 brew 的话 `brew install tcl-tk` 得到的是8.5的版本。

# 语法

## 字符串
Tcl 认为除了关键字就是字符串，所以这里 hello 其实被认为是字符串，或者可以当作 symbol 来理解，如果需要空格的的话就用花括号阔起来，不然会被认为是两个参数。双引号和花括号一样, 但是变量会被替换。单引号没有特别的意义跟其他字符一样的，这跟其他语言的词法解析还是很不同的。

```tcl
puts hello
# => hello

puts hello''world
# => hello''world

puts {hello \n world}
# => hello \n world

puts "hello \n world"
# => hello 
#    world

```

## 变量赋值
这跟上面说的有关系了，因为一切都是字符串如果是, "puts abc" 的话解释器会以为你只是想输出字符串 abc，要是要输出 abc 的值的话就得加一个 $

```tcl
set abc hello
puts $abc
# => hello
```


## 函数
```tcl
proc hello {arg} {
  puts "hello $arg"
}
hello world
# => hello world
```


## 分支语句
```tcl
if {1 == 1 } {
  puts "wow, right!"
} else {
  puts "impossible"
}
```

## regexp
```tcl
regexp {\d} 10 -> foo
puts $foo
# => 1
```

## 运算
下面两个是等价的, 都是给变量啊加1，expr 表示运算可以加减乘除，不知道为什么没有像 lisp 一样把操作符前置，而是引入了新的关键字 expr，后面跟了一个表达式
```tcl
set a 1
set a [expr $a + 1]
incr a 1
```

## list 数据结构和遍历器
```tcl
set cars {Toyota Mazda}
lappend cars Honda
puts $cars
# => Toyota Mazda Honda
foreach car $cars {
  puts $car
}
# => Toyota
#    Mazda
#    Honda
```


## Associative Arrays 和 Dictionary
功能类似 ruby 的 hash。但是 dictionary 更加强大一点，跟 array 比
* 可以直接传递给函数
* 值可以是任何类型

下面的方括号表示对方法的呼叫，相当于 ruby 里的小括号， 因为一切都是字符串，解析器需要明确定信息告诉它是字符串 foo，还是变量 $foo，还是表达式 [foo]

```tcl
array set dic {}
array set dic(foo) bar
puts $dic(foo)
# => bar

dict set clients  name Joe
puts [dict get $clients name]
# -> Joe
```

## 作用域
不像 javascript 或者 ruby 的 block，可以看到函数调用时候 context 时候里面的变量，Tcl 需要显示的声明某个变量我要引用外部的变量，可以用 global 声明他全局变量，或者用 upvar 声明要向上几个调用栈来找这个变量, 这个时候其实传递的就是引用了，在函数外面改变这个值的话，外面的值也会发生变化

```tcl
set item 1
proc test {} {
  puts $item
}
test
# => can't read "item": no such variable

proc test_global {} {
  global item
  puts $item
}
test_global
# => 1

proc test_upvar {} {
  upvar 1 item hah_my_var
  puts $hah_my_var
  set hah_my_var 2
}
test_upvar
puts item
# => 1
#    2
```
## coroutine
用 yield 交出控制权，进入挂起的状态，恢复的手段是在别的 coroutine 中或者事先埋好的 callback 中显始式的调用它的名字, `coroutine name func`，name 就是这个corouting 的标示

vwait 就是表示就如 event loop，等待时间发生 `vwait var` 表示一直等到 var 这个变量被赋值了，那么就继续运行，不然就等着，另外一个是 `vwait forever`，就顾名思义了。
```tcl
package require http

proc get url {
     http::geturl $url -command httpCallback
     #事先埋好 callback
     yield
     #交出控制权
}

proc httpCallback {token} {
  # 唤醒
  query $token
  global sig
  set sig done
}

proc main {} {
     set t [get http://wiki.tcl.tk/4]
     puts [::http::data $t]
     http::cleanup $t
}
coroutine query main
vwait sig
```

## 八卦

看到一篇文章，作者以前应该是 Tcl 死忠，总结了一下为啥 Tcl 没火起来。其实看 Tcl 的文档话感觉作者还挺傲娇的，说 ruby 除了日本基本没啥人用，给鄙视的不行了，也说了现在 node 跟 phtyon 的异步编程老子早就玩的不待完了，感觉有点酸溜溜的，哈哈哈。
作者主要总结就是
* Tcl 社区太屌，没有赶上开源运动的热潮，核心开发人员也不是很想参加社区活动
* 虽然 tk 是大杀器，但是 UI 不行，old school，被 Gnome 和 KDE 抄了老家
* 社区对语言的定义有分歧，有的想 keep small, 也有人想搞成全功能的，这其实挺尴尬的，大家不一心的话确实比较难搞，就跟公司也一样。嵌入式干不过 lua，功能全的就太多了
* 迟迟不引入官方的 OO 实现，开发者无所适从，这个 8.6 已经有了
* 没有 rails 这样的杀手级应用，web 开发又没赶上趟
* Tcl 的一切都是字符串给垃圾回收带来一些问题

总的来说还是有些小惊讶，哦原来可以这么搞啊，刚开始 abc $abc 傻傻分不清楚。总体感觉惊喜不大，不过开发个工作用的桌面应用啥的应该还挺实用的。

看来要给 ruby 贡献点什么才行呀，不然说不定就成了下个 Tcl，当然真成了 Tcl 也没啥不好的。


#### 参考

[tcl tutorial](https://www.tcl.tk/man/tcl8.5/tutorial/tcltutorial.html)

[where tcl and tk went wrong](https://journal.dedasys.com/2010/03/30/where-tcl-and-tk-went-wrong/)
