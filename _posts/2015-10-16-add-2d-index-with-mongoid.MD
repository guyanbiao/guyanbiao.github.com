---
layout: post
title: mongoid 查找附近的记录
---

model 的写法

```ruby
class Meal
  include Mongoid::Document

  field :location, type: Array
end
```
这里要注意的是，location 的类型必须是 Float 组成的 Array，而不是字符串

query 的写法

```ruby
Meal.where(:location => {'$near' => [50,50],'$maxDistance' => 5}).count
end
```

