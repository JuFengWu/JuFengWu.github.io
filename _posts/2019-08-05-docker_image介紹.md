---
title: Docker Image 介紹
author: Jufeng Wu
layout: post
---

----------------------
docker 可以說由兩個東西組成，image 和 container<br/>
這一篇，我們就一起來討論image些常用的指令吧

<br/>1.列出所有images的名稱
```
docker images 
```
注意： 列出的created 是指create 這一個images的時間而非download的時間！！

<br/>2.從網路上(docker hub)尋找新的 image
```
docker search <key_world>
```
<br/>3.下載網路上(docker hub)的image
```
docker pull <image_name>:<tag>
```
<br/>4.把image儲存起來
```
docker save <image> > <file_name>

or

docker save -o <file_name> <image>
```
<br/> 5.把儲存的image匯入
```
docker load <  <file_name>
or
docker import  <file_name>
```
注意： import 和 load 有一些地方不一樣<br/>
a. imoprt 可以重新指定image的名字<br/>
b. load不會失去之前的歷史，import會失去之前的歷史<br/>

<br/>6.從 docker file 產生image 
```
docker build -t <image_name>:<tag> .
```

<br/>目前大概就這樣，有發現新的再繼續加入
