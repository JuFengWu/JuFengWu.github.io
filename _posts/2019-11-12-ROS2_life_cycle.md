---
title: Ros2 life cycle
author: Jufeng Wu
layout: post
---

----------------------
ROS2 跟ROS1最大的不同，大概就是增加了life cycle這一個功能<br/>

雖然Ros1有一些人或是團隊，看到life cycle之後，也幫ros1加了這一個功能，但是一個是系統內建一個是後來加上，還是選系統內建比較實在？<br/>

じゃ、始めましょう！<br/>

要使用life cycle首先要先看一下他的圖<br/>
http://design.ros2.org/img/node_lifecycle/life_cycle_sm.png
基本上，這是一個state machine,有四個狀態<br/>
1.Unconfigured<br/>
2.Inactive<br/>
3.Active<br/>
4.Finalized<br/>
這四個狀態就是圖上藍色的的方塊<br/><br/>
此外，在狀態和狀態之間有6個transition states，分別為<br/>
1.Configuring<br/>
2.CleaningUp<br/>
3.ShuttingDown<br/>
4.Activating<br/>
5.Deactivating<br/>
6.ErrorProcessing<br/>
這六個狀態就是我們可以複寫的狀態啦！<br/>
也就是當這六個transition states出現時，我們要作啥事<br/>
然而這裡要注意的是，ErrorProcessing是系統的error 出現時的handle，所以我們不需要複寫這個<br/><br/>

關於實做的部份，在我寫的[範例](https://github.com/JuFengWu/ros2_basic_test_and_example/tree/master/life_cycle)中，可以看到有三個執行檔<br/>
第一個是很單純的聽<br/>
第二個是打訊號告訴聽的人<br/>
第三個是controler<br/><br/>

第一個很簡單，就是一般的listen node<br/>
第三個controller也不難，基本上就是發送狀態給talker，讓他改變狀態<br/>