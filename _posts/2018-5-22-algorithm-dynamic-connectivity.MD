---
layout: post
title: 动态连通性
tags: [algorithm]
---
# 前言
主要内容出自算法第四版，这里记录一些我的理解

# 问题描述
连通性就是判断两个事物是不是一伙的，是不是连在一起，就好像一个连在一起的铃铛，可不可以一次把全部都提留起来。
构建数据的过程就是把一个一个对象连起来，如果已经连起来了，就不用连了, 最后形成一幅无环连通图

# 应用

* 网络
* 变量相等
* 集合

# 解法

## 1. quick-find
顾名思义，就是 find 起来很快，union 很慢. 实现的方式用输入构建一个哈希表(key-value) table, 对于j 和 k两个如果 table[j] == table[k], 就认为 j 和 k 是联通的

```ruby
# 以下代码均为实例代码
def find(key)
  table[key]
end

def union(first_key, second_key)
  first_value = find(first_key)
  second_value = find(second_key)
  if first_value != second_value
    table.each do |k, v|
      if v == first_value
        table[k] = second_value
      end
    end
  end
end
```

## 2. quick-union

利用链表，把所有的在一起的连起来。如果 对 p 和 q 做 union 的做作，就是把 p 所在树的根节点指向 q 所在树的根节点, 数据结构上也是用一个Hash Table, 根节点的 key value 是相等的，其他节点的 key 代表这个数据本身，value 代表他指向的节点

```ruby
def find(key)
  # 相当于找到这棵树的根节点来代表他, 相当于他们这个团体(set)的名字
  compare_value = key
  while compare_value != table[key]
    compare_value = table[key]
  end
  compare_value
end

def union(first_key, second_key)
  first_key_root = find(first_key)
  second_key_root = find(second_key)
  if first_key_root != second_key_root
    table[first_key] = second_key_root
  end
end
```

## 3. weighted quick-union (加权quick-union)
quick-union 明显的缺点是找的时候，如果树太高的话，找的这个点又是在树底下，要挨个找到根，非常浪费时间. 所以说怎么把书的高度降低就是关键了。这个时候就是刚才连树的时候，可以 a 往 b 连，也可以 b 往 a 连，那怎么选择呢，我们这次不按照传值传进来的顺序连，而是智能一点，总是把短的树往长的树连，这样子就是 weighted quick-union 啦，简单又好用 perfect!

```ruby
class WeightedQuickUnion
  # capacity 的读音是[ ke 'pan si ti] 注意重音位置
  def initialize(capacity)
    @set_count = capacity #不同的集团有几个，也就是所谓的连通分量

    # 保存 x 在他所在集团成员数量
    @single_set_count_table = (1..capacity).to_a.map {|x| [x, 1]}.to_h

    #一开始大家全部都指向自己
    @relation_table = (1..capacity).to_a.map {|x| [x, x]}.to_h
  end

  def addNode(first_node, second_node)
    first_key_root = find(first_key)
    second_key_root = find(second_key)
    return if (first_key_root == second_key_root)

    if @single_set_count_table[first_node] >  @single_set_count_table[second_node]
      @relation_table[second_node] = find(first_node)
      @single_set_count_table[first_node] += single_set_count_table[second_node]
    else
      @relation_table[first_node] = find(sencond_node)
      @single_set_count_table[second_node] += single_set_count_table[second_node]
    end
    @set_count--
  end

  def find(key)
    # 跟 quick-union 是一样的
    compare_value = key
    while compare_value != table[key]
      compare_value = table[key]
    end
    compare_value
  end
end
```

