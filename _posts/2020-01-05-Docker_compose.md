---
title: Docker compose
author: Jufeng Wu
layout: post
---

----------------------
會使用docker之後，RUN的東西越加越多，這時候就是該使用docker compose來管這一些images了<br/>

要注意的是，安裝好docker之後，docker compose必須要額外下載，並且安裝之後才可以使用<br/>

要玩docker compose，首先要寫docker-compose.yaml，在這裡可以設定image以及container等東西<br/>

一個docker-compose.yaml會長這一個樣子<br/>
```
version: '3.0'
services:
  test_dockerfile_base:
    image: dockerfile_base_test
    build: 
      context: ./base
      dockerfile: dockerfile_base
    container_name: dockerfile_base_container

  test_dockerfile_one:
    image: dockerfile_one_test
    build:
      context: ./one
      dockerfile: dockerfile_one
    container_name: dockerfile_one_container
    volumes: 
      - /home/leowu/ros2:/root/ros
    ports:
      - 6080:80
```
此外，我也在github上有放置docker compose,可以參考這個[網站](https://github.com/JuFengWu/techman_robot_ros/blob/master/docker/docker-compose.yaml)<br/><br/>
首先，``version``是指docker compose的版本，這裡就用version3來進行示範<br/><br/>
再來是``service``，這是只要做什麼事情，在這裡有兩個事情要做，一個是test_dockerfile_base，另一個是test_dockerfile_one<br/><br/>
接著是``image``，這裡是指docker compose要產生的image的名稱<br/>
例如，在test_dockerfile_base這一個動作內，做完會產生一個叫做dockerfile_base_test的image<br/><br/>
然後是``build``，裡面有兩的子小指令，一個是context，另一個是dockerfile<br/>
``context``是以現在的位置中，dockerfile在哪裡<br/>
例如這裡是``./base``，代表就是在這一個位置有一個資料夾叫做base，裡面有dockerfile<br/>
``dockerfile``則是名稱，例如在base裡面的dockerfile名稱為dockerfile_base<br/><br/>
再來是``container_name``，代表產生的container的名稱是什麼<br/>
例如這裡就是產生一個叫做dockerfile_base_container的container<br/><br/>
接著是``volumes``，這裡是產生container時候的volumn（在docker run -v XXX的動作）<br/>
例如這裡就是把/home/leowu/ros2和container內的/root/ros連結在一起<br/><br/>
最後是``ports``，相對於產生container時候的 -p的動作<br/>
在這裡是把本機端的6080和裡面的80連結在一起<br/><br/>

接下來，則是怎麼跑docker-compose<br/>
如果只要build一個image，而不要產生container，這樣就下<br/>
``docker-compose build``<br/>
如果要build，而且同時幫每一個images都產生container的話，就下<br/>
``docker-compose up -d``<br/>
如果要build全部，而且只要部份產生container的話，要下<br/>
```
docker-compose build
docker-compose up -d <定義的動作們>
```
例如<br/>
```
docker-compose build
docker-compose up -d test_dockerfile_one
```
這樣就會產生兩個images，dockerfile_base_test以及test_dockerfile_one<br/>
但是只有產生一個container，叫做dockerfile_one_container<br/><br/>

docker compose在管理很多images的時候很好用，不過就是要花一點時間研究一下<br/>
不過說到花時間就要抱怨,公司的電腦還是傳統硬碟，所以在使用docker或是docker compose的時候總是要花一堆時間<br/>
因為要灌好才知道到底狀態怎樣，這時家裡的ssd m2就買對了，時間真的省很多<br/>
以後應該要在考績表的建議欄位上寫，電腦應該要盡可能換ssd，這樣我相信公司的同事們才會更願意來嘗試docker和docker-compose<br/>
而且可以減少開機等待的時間

