---
layout: post
title: ruby重构经常用到的方法
---

##《重构》Ruby 版的笔记

###6.9 使用方法对象替换方法

当一个方法要调用到其他很多方法的时候，参数往往要传来传去，这个时候，加一个类，把这些参数当作实例变量，就可以不用考虑传递参数，
相当于变成了实例上的全局变量

###6.12 利用 block 来传递条件

重构前

```ruby
def number_of_living_descendants
  children.inject(0) do |count, child|
    count +=1 if child.alive?
    count + child.number_of_living_descendatns
  end
end

def number_of_desendants_named(name)
  children.inject(0) do |count, child|
    count +=1 if child.name == name
    count + child.number_of_desendants_named(name)
  end
end
```

重构后

```ruby
def count_desendants_matching(&block)
  children.inject(0) do |count, child|
    count +=1 if yield child
    count + child.count_desendants_matching(&block)
  end
end

def number_of_descendants_named(name)
  count_descendants_matching {|descendant| desendant.name == name}
end

def number_of_living_descendants
    count_descendants_matching {|descendant| desendant.alive?}
end
```
