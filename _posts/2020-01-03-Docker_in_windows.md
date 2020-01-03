---
title: Docker in Windows
author: Jufeng Wu
layout: post
---

----------------------
因為偶爾會使用到windows的電腦，但是linux已經用得很習慣了，所以嘗試在winodws上裝上docker，這一篇就來寫遇到的問題吧!<br/>

Windows上裝docker有兩種，在win10可以直接安裝，但是要經過一些設定<br/>
另外一種是裝docker tool box<br/>
至於win7只能裝docker tool box<br/>
docker tool box跟virtual box有關係，似乎是直接在上面跑docker<br/>

直接安裝的的話目前遇到的坑比較少，但是安裝上似乎常常遇到一些奇怪的問題<br/>
目前唯一的就是vnc，在IE開不起來，要在chrome上開才可以<br/>
這也許也跟我安裝的image有關係吧？<br/>

再來就是tool box了<br/>
目前遇到兩個問題:<br/>
1.vnc 一樣要在chrome上開才開的起來，此外要注意的是,他的ip不是localhost，而是在一開始鯨魚畫面上旁邊的ip<br/>
2.volumn掛不上去<br/>
<br/>

volumn掛不上去這一個問題比較大，就算掛上去沒有顯示錯誤，還是無法和本機同步<br/>
a. 先打開virtual box，會看到一個正在運作的虛擬機<br/>
b. 右鍵按下setting，然後進入分享資料夾，新增一個資料夾，記得要設定成永久有效<br/>
c. 資料夾的名稱和路徑範例如下:<br/>
```
**Name**: d:\ros
**Path**: d/ros
```
d. 打開docker quickstart terminal 重新啟動docker machine，藉由下此指令<br/>
``docker-machine restart``<br/>
e. 確定是否成功，用ssh進入machine內``docker-machine ssh``，到根目錄下，可以找到其位置d或是c<br/>
f. 進入該資料夾，下``ls -all``看看owner是否為docker<br/>
g.當一切成功之後，volumn要掛上的指令為<br/>
``docker run -it -v  //d//ros://root/ros -p 6080:80 <image> bash``<br/>
這裡要注意，因為是linux，所以大小寫有差別<br/><br/>



這樣就可以開心的把volumn掛到windows上了!<br/>

最近，有的時候出去玩的時候，會想帶筆電，如果剛好有靈感可以寫一些程式<br/>
這時後有docker就很方便了，管我用哪一個筆電，image裝好就可以馬上有環境了！<br/>

最後，參考的[stack flow網站](https://stackoverflow.com/questions/33126271/how-to-use-volume-option-with-docker-toolbox-on-windows?fbclid=IwAR3d3vONyiKlNO7u06ucXGp6ffa5a-a3BH7TzpakAosvk9ndTBHg_3i7CZA)

