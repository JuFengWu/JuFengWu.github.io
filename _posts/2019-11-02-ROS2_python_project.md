---
title: Ros2 python project
author: Jufeng Wu
layout: post
---

----------------------
今天要寫ros2的python的使用方式<br/>

基本上,ros2除了c++之外，還有支援python，使用python作為ros2的開發的好處有幾個<br/>
a. python開發速度比c++快很多<br/>
b. ros2的compile時間比較短<br/>
c. 有一些python的api可以直接就拿來使用<br/>

但是還是有一些缺點，例如<br/>
a. python如果有一些error要跑得時候才知道<br/>
b. ros2 python的資源又比c++少，所以也會出現疑似bug的問題<br/>

じゃ、始めましょう〜<br/>
首先，python不需要CMakeLists.txt，所以把這一個刪掉吧，然後創建setup.py<br/>
setup.py的格式如[範例](https://github.com/JuFengWu/ros2_basic_test_and_example/blob/master/python_topic_test/setup.py)<br/>
最主要的應該還是這一段<br/>
```
entry_points={
        'console_scripts': [
            'talker = your_package_name.topic_talker:main',
            'listener = your_package_name.topic_listener:main'
        ],
    },
```
talker和listener是程式的名稱<br/>
``your_package_name.topic_talker``是檔案名稱<br/>
這是程式的名稱和進入點<br/><br/>
然後建立setup.cfg<br/>
內容如下<br/>
```
[develop]
script-dir=$base/lib/your_package_name
[install]
install-scripts=$base/lib/your_package_name
```
接著，關鍵來了，建立一個resource的資料夾<br/>
裡面放一個檔案，名稱為``your_package_name``<br/>
然後檔案裡面是空的<br/><br/>

最後，創立一個 your_package_name 的資料夾，裡面放上python的source code<br/>
但是記得這一個資料夾要加上 ``__init__.py``這一個檔案，這樣資料夾才能當python的檔案<br/>

接下來就是寫python的程式了<br/>
總覺得東西已經有一點多，放到下一篇吧!<br/>

