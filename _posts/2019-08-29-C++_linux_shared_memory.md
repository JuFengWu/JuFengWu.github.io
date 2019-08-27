---
title: C++ linux shared memory
author: Jufeng Wu
layout: post
---

----------------------
兩個執行檔如果要資料交換，除了使用tcp/ip之外，還可以使用shared memory的方式來進行資料交換<br/>

然而使用shared memory進行資料交換要注意關於鎖的機制，以免當其中一個存取的時候，另一個也來存取<br/>



