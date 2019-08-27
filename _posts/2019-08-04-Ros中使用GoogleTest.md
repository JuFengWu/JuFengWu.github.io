---
title: Ros上使用Google test
author: Jufeng Wu
layout: post
---

----------------------
在c++中，google test非常好用，所以當然要把他導入到Ros中！<br /><br/>

1.在package.xml 中加入

```
	 <test_depend>rosunit</test_depend>
  	<test_depend>gtest</test_depend>
```
 2.在CmakeLists.txt中加入
```
 	catkin_add_gtest(<node_name>  <cpp_files> )
	target_link_libraries(<node_name> ${GTEST_LIBRARIES} pthread)
```
 例如
```
 	catkin_add_gtest(test_move_api test/test_move_api.cpp test/ros_move_stub.cpp src/tm_move_api.cpp)
	target_link_libraries(test_move_api ${GTEST_LIBRARIES} pthread) 
```

3.最後，build這一份source code
```
	catkin_make <package> <test_node_name>
```

例如

```
	catkin_make tm_move_api test_move_api
```

注意：
在使用catkin_make的時候，test code不會一起被build！！

最後是使用
```
	rosrun <package> <test_node_name>
```	
例如
```	
	rosrun tm_move_api test_move_api
```	
詳細的code 可以參考我寫的練習[techman robot ros](https://github.com/JuFengWu/techman_robot_ros) 
