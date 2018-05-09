---
layout: post
title : Wowza Plugin 获取 query string
---


```
public void onPublish(IMediaStream stream, String streamName,
    boolean isRecord, boolean isAppend) {
  postStreamStart(stream.getName());
  //String uri = stream.getQueryStr();
  //getLogger().info("***********" + uri);
  //IClient client = stream.getClient();
}
```

