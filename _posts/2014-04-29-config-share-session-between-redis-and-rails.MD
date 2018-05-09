---
layout: post
title: 在 Rails 和 Node 之间共享 session
---

主要参考这篇[文章](http://stevenyue.com/2013/07/04/sharing-sessions-and-authentication-between-rails-and-node-js-using-redis/)

Rails 本身对 session 做了 Marshal.dump 和 Marshal.load，nodejs 做这部分就比较困难，所以就采用一个变通的方法在用户登录的时候给他的 cookie 里面设一个值，然后再用这个值做为一个 hash 的 key 和用户的信息一起放到 redis 里面，用户登出的时候再把这个值删掉就可以了

```
# app/controllers/application_controller.rb
def after_sign_in_path_for(resource_or_scope)
  #store session to redis
  if current_user
    # an unique MD5 key
    cookies["_validation_token_key"] = Digest::MD5.hexdigest("#{session[:session_id]}:#{current_user.id}")
    # store session data or any authentication data you want here, generate to JSON data
    stored_session = JSON.generate({"user_id"=> current_user.id, "username"=>current_user.screen_name, ... ...})
    $redis.hset(
      "mySessionStore",
      cookies["_validation_token_key"],
      stored_session,
     )
   end
end
 
def after_sign_out_path_for(resource_or_scope)
  #expire session in redis
  if cookies["_validation_token_key"].present?
    $redis.hdel("mySessionStore", cookies["_validation_token_key"])
  end
end
```
