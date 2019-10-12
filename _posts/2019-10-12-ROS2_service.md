---
title: Ros 2 service
author: Jufeng Wu
layout: post
---

----------------------
這次我們來討論ros2的service吧<br/>

service跟msg很像，唯一不同的是聽到的人會傳送feedback回來<br/>
剩下的跟service很像，注意事項也跟topic很像 <br/>

創立的方法很簡單<br/>
a. service的server:<br/><br/>
aa. 初始化<br/>
bb. 使用產生node<br/>
cc. 設定handleService<br/>
dd. node->create_service<br/>
要注意,handleService中會有三個變數，分別是request_header和request以及response<br/>
request_header的作用還不確定<br/>
request是接收的資料<br/>
response則是回傳的資料<br/><br/><br/>
b. service的client：<br/><br/>
aa.初始化以及使用產生node<br/>
bb.等待連線OK
cc. 設定資料
dd.當資料傳送回來之後，看結果<br/><br/>
大概就是這樣，這跟msg實在超級像，範例在[這裡](https://github.com/JuFengWu/ros2_basic_test_and_example)，會msg應該沒啥問題XD


