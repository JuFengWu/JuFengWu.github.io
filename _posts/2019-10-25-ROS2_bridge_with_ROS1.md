---
title: ros2 and ros1 bridge
author: Jufeng Wu
layout: post
---

----------------------
今天要寫的是ROS2和ROS1之間的bridge<br/>
也就是把ROS2的訊號跟ROS1護相對傳<br/>

作法很簡單，然而要注意一件事情，default的bridge只接受ros2的基礎資料傳送<br/>
詳細可以看[這裡](https://github.com/ros2/common_interfaces)<br/>

這是啥意思？範里裡面的打開talker.cpp我們可以看到他include的是``#include "std_msgs/msg/string.hpp"``<br/>
這一個msg就是在ros2的基礎資料裡面<br/>
如果要送自定義(custom)的msg，必須要在額外build bridge，詳細可以看[這裡](https://github.com/ros2/ros1_bridge/blob/master/doc/index.rst)<br/>

じゃ、始めましょう！<br/>

1. 首先先開啟第一個terminal，然後下<br/>
```
. /opt/ros/melodic/setup.bash
roscore
```
這樣ros1的主要的server就打開了<br/><br/>

2. 接下來，開第二個terminal，然後下<br/>
```
. /opt/ros/melodic/setup.bash
. /opt/ros/dashing/setup.bash
export ROS_MASTER_URI=http://localhost:11311
ros2 run ros1_bridge dynamic_bridge __log_disable_rosout:=true
```
這樣，ros2和ros1的bridge就打開了<br/>
那位啥要加上 ``__log_disable_rosout:=true``，因為目前我測試的docker中的bridge在log紀錄上，好像有一些問題，所以可以加這一個先避開<br/>
但是我在桌機上測試又OK？這真是怪阿？看來又可以去寫問題了<br/>

3. 然後，開啟第三個terminal，然後下<br/>
```
. /opt/ros/melodic/setup.bash
rosrun rospy_tutorials listener
```
這樣，ros1就開始聽ros2的東西了<br/>

4. 最後，開啟第四個terminal，然後下<br/>
```
. /opt/ros/dashing/setup.bash

```
這時候就會看到ros2傳東西給ros1聽<br/>

以上<br/><br/>
有一些國外的網站寫到，建議我們玩ros2<br/>
然而因為ros2的東西還不完整，所以建議當下除了ros2之外要再加上和ros1的bridge跟ros1一起cowork<br/>
所以bridge很重要！ <br/>

