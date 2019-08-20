---
title: C++ memory leak 尋找方法-linux(Valgrind)
author: Jufeng Wu
layout: post
---

----------------------
相對於python,c#,java, 寫 c++和c最頭痛的大概就就是memory leak的問題<br/>
雖然c++有smart pointer（之後會提到）,但是有的時候還是要必須要用傳統的new 以及delete的時候,確定有沒有memory leak就是很重要的一件事情了,特別是這一隻程式需要被執行很久的時候<br/>
所以這次就來教一下memory leak的好工具Valgrind ！（linux專用喔！）<br/>

1. 安裝 Valgrind :
``sudo apt-get install valgrind``
2. 執行Valgrind：
``valgrind --leak-check=full ./your_file.out``
<br/>用這一個[例子](https://github.com/JuFengWu/cpp_examples/tree/master/valgrind_liinux_memory_leak)來說<br/>

要看哪裡有問題，所有執行的步驟為：

1. 下指令``make``，build 專案
2. 下指令``valgrind --leak-check=full ./test_memory_leak.out``
<br/>就可以從它顯示有問題的地方找出蛛絲馬跡<br/><br/><br/>

每次說到memory leak 讓我印象最深刻的處理方法就是:<br/>
讓電腦一直跑一直跑，然後觀察memory 有沒有上升<br/>
雖然這不失一個方法，但是每次看到這樣都覺得有一點土砲，無法顯示自身為程式工程師的專業（誤）<br/>
最大的問題是，無法真的確定memory 是不是真的有free掉，說不定跑幾天沒有，但是到客戶手上有有問題，既然有好用的工具，就來使用一下吧！

