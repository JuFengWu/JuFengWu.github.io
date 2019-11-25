---
title: Ros2 error and solve
author: Jufeng Wu
layout: post
---

----------------------
這一篇基本上是用來紀錄ros2在寫的時候遇到的一些錯誤，以及這一些錯誤要如何解決<br/>


1.<br/>
```
terminate called after throwing an instance of 'std::runtime_error'
what():  Node has already been added to an executor.
```
在ros2中,一個node只能被spin一次，如果spin兩次會出現這一個問題<br/><br/>
2.<br/>
如果listener只聽一次就停下來，檢查listener的listener參數和宣告的是一致<br/>
