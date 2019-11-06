---
title: C++ smart pointer
author: Jufeng Wu
layout: post
---

----------------------
在C++ 中，最討厭的大概就是忘記清掉記憶體空間，使得memory的使用越來越多，最後當機<br/>

在很多語言都有所謂的GC(garbage collection)的功能，唯獨C++要自己釋放<br/>
因此，在c++ 11 之後就有所謂的smart pointer可以使用<br/>

c++的smart pointer有三個<br/>
1.unique_ptr<br/>
2.shared_ptr<br/>
3.weak_ptr<br/>

unique_ptr的範例[在此](https://github.com/JuFengWu/cpp_examples/blob/master/smart_pointer/unique_ptr.cpp)<br/>
基本上，unique_ptr只能夠產生一個指向一個物件，所以叫做unique ptr，這應該很好理解XD
此外，還有要注意的是，建立unique pinter的時候，有兩個方法，分別是``make_unique``以及``new``，make_unique要在c++14（包含）之後才會有<br/><br/>

shared_ptr的範例[在此](https://github.com/JuFengWu/cpp_examples/blob/master/smart_pointer/shared_ptr.cpp)<br/>
基本上，shared_ptr可以很多人同時指向<br/>
所以有一個方法為``use_count``去看說到底有誰指向他<br/>
當然，生成的方式主要是``make_shared``,這個和unique_ptr不一樣的是，在c++11就有make_shared了！<br/>

weak_ptr暫時還沒有寫範例QQ<br/>
小的會努力補齊全<br/>
weak_ptr主要用來指向shared_ptr<br/>
主要的用途是，當weak_ptr被釋放時，shared_ptr的counter，不會被-1<br/>
這樣的好處在於，有的時候在循環的況之下，如果使用shared_ptr而非weak_ptr，有可能會還在被指的東西不小心被釋放掉或是沒有被釋放<br/>

smart pointer 在ros2中的範例中常常被拿來使用<br/>
所以要學好，以免看不懂範例！<br/>