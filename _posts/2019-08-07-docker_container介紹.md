---
title: Docker Container 介紹
author: Jufeng Wu
layout: post
---

----------------------
docker 的container 是跑在image之上，所以要使用docker必須要先產生container <br/>

此外，container有兩種狀態，up 和exit，up為已經呼叫起來的狀態，exit為沒有被呼叫起來 <br/>
這次就介紹一些常用的container命令吧！<br/>
1. 顯示所有的container
```
docler ps -a
```
2. 產生新的container
```
docker run <options> <image_name> <command> <args>
ex:
docker run -it --rm -p 6080:80 docker_ros_melodic_vnc_techman
docler rin -it docker_ros_melodic ./bin/bash
```
其中，一些常用的options如下：
a. -it : interaction and terminal
b. -rm: remove after exit
c. -p set port
3. 呼叫起現有的container
```
docker start <container_name>
```
4. 執行現有的container
```
docker exec <options> <container_id> <command> <args>
```
這裡的 options 和 arg 以及 command和 run是一樣的
5. 從terminal中離開contianer： 只需要打exit就可以了
6. 關起contianer:
```
docker stop <container_id>
```
7. 顯示container和image不一樣的地方
```
docker diff <container_name_or_container_id>
```
8. 將container 打包成一個image
```
docker commit <option> <container_id> <image_name>:<image_tag>
```
其中有幾點要注意 <br/>
a.用commit 的話會把所有東西，一起打包 <br/>
b.option的指令如下： <br/>
-a: add author <br/>
-c use docker file to commit  <br/>
-m: add memo <br/>
-p: suspend container when commit <br/>

9. 幫container重新命名
```
docker rename old_container_name  new_container_name
```
