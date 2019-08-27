---
title: Docker Dockerfile 介紹
author: Jufeng Wu
layout: post
---

----------------------
上一篇有提到，docker 可以說由兩個東西組成，image 和 container 這一篇<br/>

我們就一起來討論從docker file 建立image的指令吧！ <br/>
常用的指令如下：
<br/>
<br/>
1. FROM : 表示這一個image 來自那一個base, base可以是docker hub，也可以是本機<br/>
2. LABEL :給這一個image一些資訊，例如作者<br/>
3. RUN: 下command 到 terminal<br/>
關於run要注意一些事情
a. 只能下命令,沒有辦法去記憶參數,例如
```
RUN source ~/.bashrc
RUN xxx //此時無法使用剛剛的source
RUN source ~/.bashrc && XXX  //有辦法使用環境變數
```
b. 在一個run中執行兩個動作的方法可以用
```
 RUN command_A && command_B
```

c. 在linux中，因為有一些會需要輸入y去同意安裝，這時候可以用
```
 RUN echo "y" | <command>
```
<br/>
4.ENV 設定一些build的環境參數
<br/>
5.COPY 把本機的資料複製到image內
```
CPOY <本機資料> <image的資料>
```
