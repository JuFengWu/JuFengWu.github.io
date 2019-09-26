---
title: ROS2 gazebo plugin
author: Jufeng Wu
layout: post
---

----------------------
ROS2 的Gazebo plugin 跟 ROS1很像，然而有幾個地方還是不一樣<br/>

#### gazebo_ros_control 不能使用:<br/>
與其說不能使用，到不如說這會使得gazebo卡住，原因是因為ros1的roscore沒有啟動，所以就卡在這裡 <br/>
此外，可以用``<plugin filename="libgazebo_ros_joint_state_publisher.so" name="joint_state">``在ros2中傳送joint_state<br/>
#### 自己寫plugin :<br/>
目前看到大部分的作法還是自己寫plugin,gazebo plugin的寫法大概如下：<br/>
1.在模型檔案中加入
```
<plugin filename="libXXX.so" name="AAA">
      <ros/>
      <axis1>shoulder_1_joint</axis1>
      <axis2>shoulder_2_joint</axis2>
      ....
    </plugin>
```
2.新增一個package ,在Cmakelist.txt加入
```
find_package(gazebo_dev REQUIRED)
find_package(gazebo_ros REQUIRED)
...
add_library(xxx SHARED
  plugin檔案位置
)
...
ament_target_dependencies(XXX
  "gazebo_dev"
  "gazebo_ros"
  "rclcpp"
  "rclcpp_action"
  "ament_index_cpp"
)
...
ament_export_libraries(XXX)
```
然後在package.xml中加入
```
<build_depend>gazebo_dev</build_depend>
  <build_depend>gazebo_ros</build_depend>
...
<exec_depend>gazebo_dev</exec_depend>
  <exec_depend>gazebo_ros</exec_depend>
```
3.gazebo plugin的c++寫法有幾個注意地方<br/>
a. namespace 為gazebo_plugins<br/>
b. 去複寫 XXX::Load<br/>
c. 在load的時候,加入<br/>``this->updateConnection = gazebo::event::Events::ConnectWorldUpdateBegin(std::bind(&XXX::OnUpdate, this));``<br/>
d. 創立OnUpdate，這是每次畫面更新的時候的動作<br/>
e. 記得要加入 GZ_REGISTER_MODEL_PLUGIN(TMGazeboPluginRos)<br/>

大概就是這樣，詳細的話可以看[這裡](https://github.com/JuFengWu/techman_robot_grasp_ros2/tree/master/tm_gazebo_plugin)<br/>
