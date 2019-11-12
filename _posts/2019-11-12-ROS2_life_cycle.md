---
title: Ros2 life cycle
author: Jufeng Wu
layout: post
---

----------------------
ROS2 跟ROS1最大的不同，大概就是增加了life cycle這一個功能<br/>

雖然Ros1有一些人或是團隊，看到life cycle之後，也幫ros1加了這一個功能，但是一個是系統內建一個是後來加上，還是選系統內建比較實在？<br/>

じゃ、始めましょう！<br/>

要使用life cycle首先要先看一下他的[圖](http://design.ros2.org/img/node_lifecycle/life_cycle_sm.png)<br/>
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
6.ErrorProcessing<br/><
這六個狀態就是我們可以複寫的狀態啦！<br/>
也就是當這六個transition states出現時，我們要作啥事<br/>
然而這裡要注意的是，ErrorProcessing是系統的unexception error 出現時的handle，所以我們不需要複寫這個<br/><br/>

此外，要注意的事情是，這幾個狀態必須要按照圖上的流程走，不能亂走<br/>
例如想要從Unconfigured直接跳過Inactive到Active是不行的，會報error!<br/>
但是不會進入ErrorProcessing<br/><br/>

關於實做的部份，在我寫的[範例](https://github.com/JuFengWu/ros2_basic_test_and_example/tree/master/life_cycle)中，可以看到有三個執行檔<br/>
第一個是很單純的聽<br/>
第二個是打訊號告訴聽的人<br/>
第三個是controler<br/><br/>

第一個很簡單，就是一般的listen node<br/>
第三個controller也不難，基本上就是發送狀態給talker，讓他改變狀態<br/>
第二個稍微複雜一點，就是這六個transition states要做的相對應的動作<br/>
例如在Configuring的時候進行create_publisher;on_cleanup的時候重製counter之類的<br/>
然後只有在Active的狀態中,才會發送publish message出去<br/>
其他的狀態就算publish這一個function在作用，也不會發送message出去<br/>

life cycle大概就是這樣<br/>
其實之前在寫android的時候app也有屬於他的life cycle,所以看到life cycle這一個東西有一種熟悉感覺XD<br/>
此外，這一個東西在發展robot系統的時候好用，如果未來我要架設公司的系統的時候，大概也會用life cycle這一個概念吧<br/>
這樣每一個演算法或是功能開發者,主要的演算法放在activity,剩下只需要想好其他的狀態中需要做啥事情<br/>
例如進入到緊急停止的狀態把速度快速降到零<br/>
然後，每一個狀態狀態由主要的程式架構開發者或是由safety function來調控,這樣的話整個程式就會變得很好維護了！