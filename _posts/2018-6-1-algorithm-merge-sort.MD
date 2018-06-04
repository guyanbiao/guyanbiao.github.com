---
title: 归并排序
layout: post
tags: [algorithm]
---

# 问题描述
对一个数组进行排序

# 解法

背景知识是，如果让我们两个单调不递减的数组合并起来，然后保证合并后的数据是单调不递减的，这是一件比较容易的事情，可以想象成两个队比身高，身高低的算输(打我呀), 然后两个队都是从低到高派出选手，低的那个出局，站在旁边，我们称之为失败序列,这个时候输的那一队会派出第二个, 输掉的就去失败序列, 依次类推，当一方队伍耗尽的时候，另外一方也依序进入失败队列，最后失败队列就是一个单调不递减的序列，我们就完成了排序, 这个方法我称之为 merge

所以我们就想，如果可以构造两个有序队列就好了。

于是把一个数组分成两两半, 然后分别对他们排序，怎么排序呢，再把他分成两半，一直到就剩下两个元素，直接比较就可了，然后在一层一层的 merge 回来，最后就是结果了

# SMTC(show me the code)


```ruby
def merge_sort(arr)
  return arr if arr.length <= 1
  if arr.length == 2
    if arr[0] > arr[1]
      [arr[1], arr[0]]
    else arr
    end
  else
    midlle = arr.length / 2
    merge merge_sort(arr[0..midlle]), merge_sort(arr[(midlle + 1)..(arr.length)])
  end
end

def merge(first, second)
  result = []
  a, b = first, second
  while true
    if a.length == 0 and b.length == 0
      break
    elsif a.length == 0
      result.push(b.shift())
    elsif b.length == 0
      result.push(a.shift())
    elsif a[0] > b[0]
      result.push(b.shift())
    else
      result.push(a.shift())
    end
  end
  result
end
```
