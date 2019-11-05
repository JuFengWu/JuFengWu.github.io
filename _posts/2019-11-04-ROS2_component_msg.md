---
title: Ros2 component msg
author: Jufeng Wu
layout: post
---

----------------------
Ros2 在他的說明文件裡面，有提到component是Ros2改善的一個東西，所以就來玩玩看吧<br/>

Ros2的component跟Ros1的notelet概念很像，讓之間的message或是service用更快速的方式進行交換<br/>
不一樣的地方在於Ros2的component的api跟一般的node的api是相同的，Ros1則是不同<br/>
再來，Ros1的notelet在跑得時候，要決定mapping到那一個node，Ros2則是不用<br/>
當然上述兩點是Ros2的文件寫的，小的只有玩過Ros2的component，在玩Ros1的時候沒有實際寫過notelet，所以只能在這裡嘴炮XD<br/>
然而，在很多Ros2的範例code會看到他用component的形式寫，而且有一些還有一些syntax error(?!)，所以最好還是要看得懂或是要會改成正確的語法<br/>
當然最好還是要會寫！<br/>

じゃ、始めましょう！<br/>

### 撰寫方法<br/>
首先，當然拿msg最簡單的自定義talker和listener來試試，我寫的package範例[在此](https://github.com/JuFengWu/ros2_basic_test_and_example/tree/master/component_test)<br/>

1.關於CmakeList:<br/>
一般的node是加上``add_executable``、``ament_target_dependencies``以及``install``<br/>
在component是要加上``add_library``、``target_compile_definitions``、``ament_target_dependencies``、``set``以及``install``這五個東西<br/>
詳細就看code吧<br/>

2.package.xml則是一樣，要加上package name以及使用的library<br/>

3.再來就是.h的部份，基本上就是繼承``public rclcpp::Node``<br/>
要注意的是<br/>
a.在function之前要加上，pulic要加上``XXX_PUBLIC``，private要加上``XXX_LOCAL``，XXX是你設定的名字<br/>
b.在include資料夾內加上 ``visibility_control.h``這一個檔案，內容如[附件](https://github.com/JuFengWu/ros2_basic_test_and_example/blob/master/component_test/include/visibility_control.h),記得要把COMPOSITION改成XXX<br/>
c. 建議在建構子上加入``explicit``的關鍵字<br/>

4.最後是C++的部份，基本上就是完成這一個.h檔案，唯一要注意的是在最後一行要加上``RCLCPP_COMPONENTS_REGISTER_NODE(namespace::XXX)``<br/>
至於c++的寫法就一般的ros幾乎一樣，就是message怎樣寫就怎樣寫<br/>
只是因為不是node，所以不用寫node的部份<br/>
以及這算是外掛程式，所以不用寫main<br/>

### 使用方法（最簡單的）<br/>
使用方法比較麻煩一點，首先因為這一些東西可以幫他當作ros2的外掛程式，所以原則是你啟動一個node，其他人可以動態的去設定要不要執行這一個外掛<br/>
1.開啟第一個terminal，先source 自己的專案之後，下``ros2 run rclcpp_components component_container``,這樣主要的node被啟動了<br/>
這一個node基本上你可以想說就是之前我們寫的main的部份，然後等待外掛進入去執行外掛<br/>
唯一不一樣的是，這一個node是ros2 default的node，專門讓你安裝其他的component外掛用的<br/>
2.開啟第二的terminal,當然也要先source 自己的專案之後,下``ros2 component load /ComponentManager component_test composition::Talker``<br/>
在這裡``/ComponentManager``是剛剛的``component_container``算是別名的東西，代表說我的外掛要掛到這裡來<br/>
``component_test``是我的package的名稱<br/>
``composition::Talker``是在``rclcpp_components_register_nodes(talker_component "composition::Talker")``裡面的註冊名字，代表talker_component我用註冊的時候要用composition::Talke的名稱<br/>
這降就可以開心的把東西註冊上去了!<br/>

最近除了英文之外技術文章，也看了很多對岸和日文的技術文，發現寫的時候偶爾會熊熊忘記某一些用詞要怎麼寫<br/>
英文還好，反正在科技業基本上就是用克里奧語，所以中文加上英文<br/>
日文也不會有啥問題，畢竟日文的專有名詞都是從英文來的，也還好<br/>
但是對岸用語就比較麻煩，有得時候會熊熊給他忘記某一些詞台式中文應該的用法<br/>
例如外掛寫成差件，軟體寫成軟件<br/>
畢竟看一篇文章一下台式中文一下中式中文總覺得很不舒服<br/>
就像說話一下使用英式英文一下使用美式英文; 或是一下使用德式德文一下使用瑞士德文，感覺就是不舒服<br/>
中式中文就是要搭配簡體字來看才會覺得正確<br/>
台式中文就是要用繁體字看才對味<br/>
最痛恨就是明明寫繁體字，結果裡面用語全部都是中式中文，看到崩潰（很多農場文和繁中翻譯文都是這樣）<br/>
所以小弟會努力讓文章用台式中文個風格，畢竟中式中文的文章應該已經很多了<br/>
如果那一天想要來寫中式中文，應該會用簡字體來寫，這樣比較對味<br/>
