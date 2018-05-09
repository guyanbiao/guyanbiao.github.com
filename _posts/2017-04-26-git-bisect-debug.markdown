---
layout: post
title: "Debug 利器 Git Bisect"
excerpt: "当你知道问题是在哪里引入的情况下文件标注可以帮助你查找问题。 如果你不知道哪里出了问题，并且自从上次可以正常运行到现在已经有数十个或者上百个提交，这个时候你可以使用 git bisect 来帮助查找。 bisect 命令会对你的提交历史进行二分查找来帮助你尽快找到是哪一个提交引入了问题。"
tags: [Git]
---

老早就听说过用 **Git 的 bisect, 二分查找问题 commit**, 只是一直都没机会实践运用. (项目都跑的太正常了, 手动斜眼)

其实, 只是绝大部分时候总可以通过各种 **Log** 来追踪到问题所在. 今个儿遇到了个实在没法看日志的问题.

团队的成员提交了各自提交了各种代码并合并到了 master 分支, 正准备发测试环境, 发现部署的时候 **precompile** 崩掉了, 一看报错日志. 一脸懵逼了...


这只说了某个等号附近有语法错误, 具体在哪一个文件哪一行也没个说法, 只给出了这一大坨, 加起来单是当天新增的 commit 少说二三十个, 总不能一个个去 reset 看哪儿改的 JavaScript 引起吧 = . =

```ruby
ExecJS::ProgramError: SyntaxError: Unexpected token operator «=», expected punc «,» (line: 58005, col: 69, pos: 2186981)

Error
    at new JS_Parse_Error (<eval>:3623:11948)
    at js_error (<eval>:3623:12167)
    at croak (<eval>:3623:22038)
    at token_error (<eval>:3623:22175)
    at expect_token (<eval>:3623:22411)
    at expect (<eval>:3623:22562)
    at ctor.argnames (<eval>:3623:27486)
    at function_ (<eval>:3623:27550)
    at expr_atom (<eval>:3623:31068)
    at maybe_unary (<eval>:3624:1752)
    at expr_ops (<eval>:3624:2523)
    at maybe_conditional (<eval>:3624:2615)
    at maybe_assign (<eval>:3624:3058)
new JS_Parse_Error ((execjs):3623:11948)
js_error ((execjs):3623:12167)
croak ((execjs):3623:22038)
token_error ((execjs):3623:22175)
expect_token ((execjs):3623:22411)
expect ((execjs):3623:22562)
ctor.argnames ((execjs):3623:27486)
function_ ((execjs):3623:27550)
expr_atom ((execjs):3623:31068)
maybe_unary ((execjs):3624:1752)
expr_ops ((execjs):3624:2523)
maybe_conditional ((execjs):3624:2615)
maybe_assign ((execjs):3624:3058)
```

还好可以在本地用

```ruby
RAILS_ENV=staging rake assets:precompile
```

还原 precompile 的错误,

--------------

想起 git bisect, 看样子可以用上了.

```ruby
git bisect start
git bisect good be835ca5ce4755ea106701587c308d6add8d4f7
# be835ca5ce4755ea106701587c308d6add8d4f7 是该分支上确定不会出现这个 bug 的 commit,
# 其实, 只是随意在昨天的 commit 里面捞一条出来而已
```

标记好了以后跑一次

```ruby
RAILS_ENV=staging rake assets:precompile
```

发现还是编译不过, 于是

```ruby
# 标记为 bad
git bisect bad

Bisecting: 36 revisions left to master after this (roughly 5 steps)
[76dbb96bc3b5bfb1465f81081aeebb9a34e6f92b]

➜  Cherry git:(76dbb96)
```

**76dbb96** 这个 commit 正好是 先前设定的 **be835ca5ce4755ea106701587c308d6add8d4f7** 跟当前最新 commit 的中间节点.

git 此时已经 **checkout** 到 76dbb96

**说明问题出在最新 commit 到 76dbb96 commit 之间.**

刚刚已经标记好了 bad, 再继续跑

```ruby
RAILS_ENV=staging rake assets:precompile
```

发现没有问题, 能编译过, 所以,

```ruby
git bisect good

Bisecting: 18 revisions left to master after this (roughly 4 steps)
[cdc07fc05a85465797086dc0f9fe5c663b7ffcf6]
➜  Cherry git:(cdc07fc)
```

再一次 checkout

标记为 good 以后可以看到, 问题出在从 cdc07fc 到当前最新的代码之间某个 commit. 继续重复上述的步骤, 其实 git 有给提示最多还要 4 次就可以找出问题 commit 在哪了.

实际可能更少. 更快就找到了问题所在的 commit 了.

最后, 发现是 Javascript 方法默认值这里出的问题 = . =

```javascript
(function(){
  window.DrawCarState = {
    single_car_state: function(car_id, begin_at, end_at, draw_target = '', format = 'html'){
      $.ajax({
      ...
      })
    }
  }
}).call(this);

```

定位好问题分支以后可以通过

```ruby
git bisect reset
```

恢复到 bisect 前的状态
