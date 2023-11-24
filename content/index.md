---
title: Welcome to Quartz
---

This is a blank Quartz installation.
See the [documentation](https://quartz.jzhao.xyz) for how to get started.


```dataview
calendar file.ctime
from "content"
```


```dataview
list
from "content"
where contains(tags, "daily")
```



```dataview
table title, file.ctime as "created_time"
from "content"
where contains(tags, "daily")
```


```dataview
task
from "content"
where contains(author, "lgf")
```




