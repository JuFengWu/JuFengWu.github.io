---
title: Docker container connect
author: Jufeng Wu
layout: post
---

----------------------
這次在docker中遇到了一個問題，要如何在其他台電腦用tcp ip和內部的containetr溝通<br/>

我們把這一個問題分成幾個階段的問題：<br/><br/>

1. container內為clinet，外部的電腦為server<br/>
2. container內為server，同一台電腦上中本機端為client 要連線進來<br/>
3. container內為server，其他電腦上為client 要連線進來<br/><br/>

關於問題一，其實很簡單，就像一般的連線一樣，輸入外部電腦的ip以及連線的port就可以傳送資料過去了<br/><br/>

關於問題二，首先要先進入doecker，下ifconfig找出ip address<br/>
或是在外面下指令 ``docker inspect container_name | grep "IPAddress"``找出ip address<br/>
例えば：我剛剛下這一個指令就跑出"172.17.0.2"<br/>
這時外面連線的時候就按照這一個ip address就可以了<br/>
例えば：我就要輸入"172.17.0.2"<br/><br/>

關於問題三，問題會有一點複雜<br/>
首先，我們從問題二中會知道，container其實有自己的ip，然而本機也有屬於自己的ip<br/>
這時外面的電腦要連的是本機的ip<br/>
例えば：　本機ip為 192.168.100.27  container的ip為 172.17.0.2，這時外面要輸入的ip為192.168.100.27<br/>
可以這樣做完之後，保證還是連不起來，なぜ？<br/>
問題在於port的設定阿～<br/>
因為要把container的port和本機的port相連，也就是再一開始產生container的時候就要加上-p XX（本機port）:XX（container port）<br/>
例えば：`` docker run -it -v /home/leowu/ros2:/root/ros2  -p 6080:80 -p 6189:6189 ros_env:ver1``<br/>
這時可以看到，我把本機的6080和內部的80相連，本機的6189和內部的6189相連<br/>
所以這時，外部就連 192.168.100.27 port為6189 就可以連起來了！<br/>

最後，還是要murmur一下，為什麼在設計的時候，居然是我們的產品當client去連線其他家的週邊產品（當server）<br/>
真的就不擔心老闆有一天說要一次支援很多家週邊產品嗎？<br/>
還是打算每連線一個週邊產品就開一個client的thread XD<br/>
這到底是那一個pm設計的，給我出來面對（敲碗！）<br/>
