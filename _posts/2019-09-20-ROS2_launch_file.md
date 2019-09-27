---
title: Ros 2 launch file
author: Jufeng Wu
layout: post
---

----------------------
ROS2 的使用上跟ROS1最大的不同大概就是launch file<br/>

這一篇就來介紹一下ROS2的launch file吧<br/>

1.不像ROS1,ROS2的launch file是使用python的語法來寫，最大的優點大概就是可以塞一些print在程式之間，以方便debug<br/>

因為是python，所以再用launch的時候，可以用一些合成string的方法來產生路徑，當然也可以使用<br/>

```
arg = sys.argv[1:]
```
2.來得到使用者輸入的參數<br/>
再來是可以使用``os.path.join`` 這一隻function來設定其路徑<br/><br/>

3.其中可以看到設定路徑的時候使用<br/>
```
get_package_share_directory('package_name')
```
這是因為ros在編譯的時候，會產生install 檔案，一些相關的東西會被放置在install的share裡面<br/>
所以有一些東西就要從install裡面package的share裡面產生路徑，這一個function就是在做這一件事情<br/><br/>

4.剛剛有提到有一些需要使用者呼叫的東西會被放在share裡面，例如URDF或是launch file之類的<br/>
因此在CMakeList.txt中要記得加上
```
install(DIRECTORY launch
  DESTINATION share/${PROJECT_NAME}
)
```
這樣的話就可以把一些給使用者的資訊，例如launch file或是URDF之類的放到install的share裡面<br/>
但是相對的C++或是python的檔案這一些比較敏感的東西，在package5中就不用加上這一行<br/><br/>
5.最後是使用LaunchDescription，這是呼叫node起來的function<br/>
以[這一個](https://github.com/JuFengWu/techman_robot_grasp_ros2/blob/master/tm_launch/launch/tm_rviz_joint_state.launch.py)為例子<br/>
可以看到呼叫了3個node，這一個function傳送的方式是用list的方式，產生並加入名稱為Node的object<br/><br/>
6.Node這一個class的使用方法，可以傳遞:<br/><br/>
a. package: package的名稱<br/>
b. node_executable: node執行檔的名稱<br/>
c. arguments :傳遞參數<br/>
剩下還有幾個不知道的<br/>
d. node_name: 應該是給予這一個node名稱，也許在重複呼叫相同的node執行檔的時候可以用？<br/>
e. output: 這裡每次都看到要加上'screen'，之後有看到不同的在研究看看吧<br/><br/>
7.在run launch file的時候，可以看到螢幕上的前面有
```
[some_node_name-number]
```
這一個表示這``some_node_name``這一個node顯示的資訊，至於``-number``則是表示這是這一個luanch file叫起來的第幾個node<br/>
所以可以從這裡進行debug，看看哪一個node發生了啥事<br/><br/>
ROS2在launch file上從標籤語言改成了python，對時常寫python的我來說真的是一大福音，而且對於debug來說真的簡單很多，在這一點表示開心！
