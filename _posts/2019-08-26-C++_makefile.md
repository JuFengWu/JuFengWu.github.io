---
title: C++ make file
author: Jufeng Wu
layout: post
---

----------------------
Makefile是使用gcc下指令的好幫手，畢竟當檔案連結越來越大的時候,一直下gcc大概會累死<br/>
寫好Makefile之後，只需要下``make``就可以自動編譯了，非常的方便<br/>
一個專案的架構大概如此[範例](https://github.com/JuFengWu/cpp_examples/tree/master/make_file_example)<br/>
裡面有可能有src(source code放的位置)，indclude(h file 的位置)，以及test(test code的位置)<br/>
這時候就需要makefile的幫忙了！<br/><br/>

在一個makefile中，可以看到一開始會有一些宣告，例如<br/>
```
CC = g++
GOOGLE_TEST_LIB = gtest
GOOGLE_TEST_INCLUDE = /usr/local/include
INCLUDE_DIR = ./include
G++_FLAGS = -c -Wall -I $(GOOGLE_TEST_INCLUDE) -I $(INCLUDE_DIR)
```
這一些是表示之後參數的別名，之後就可以用縮寫的方式來進行編譯<br/><br/>
接下來會使用all，例如<br/>
```
all:main.out test.out clean
```
這一個意思是最後會build出main.out以及test.out，並且會執行clean的動作<br/><br/>
接下來，會寫指令名稱，以及這一個指令的dependency，和指令，例如
```
main.out: main.o tm_move_api.o tm_test.o
	$(CC) -o main.out main.o tm_move_api.o tm_test.o
```
這表示，``main.out``會依賴 ``main.o``以及``tm_move_api.o``和``tm_test.o``<br/>
至於``main.o``以及``tm_move_api.o``和``tm_test.o``的定義，他會往後面去找<br/>
而下面的``$(CC) -o main.out main.o tm_move_api.o tm_test.o``則是指令<br/>
其中``$(CC)``就是之前定義的參數別名，在這裡``CC=g++``<br/>
而``-o``在gcc的指令代表產生output file，這一個output file的名稱是後面的那一個，在這裡是main.out，剩下就是要link的檔案們<br/><br/>

再來就是會看到類似這樣的指令<br/>
```
tm_test.o: src/tm_test.cpp src/tm_test.h
	$(CC) -I$(INCLUDE) $(CFLAGS) -c src/tm_test.cpp
```
這一個的話跟上面的類似，只是這次的dependency 是cpp 和h檔了，所以冒號後面就是cpp 和h檔<br/>
下面的指令跟之前的是一樣，其中``-I`` 是表示追加include檔案的搜尋路徑``-c``是指只編譯不連結，畢竟連結的動作上一行會做連結的動作<br/><br/>

最後就是``clean``，這應該很好懂，就是把這一些暫存檔都清除，這樣畫面比較乾淨，當然也可以不清除啦<br/>
``clean``通常會這樣寫<br/>

```
clean:
	rm -f main.o api.o tm_test.o tm_move_api.o main_test.o string_compare.o
```
就是把這一些o檔移除<br/><br/>

還有一行``.PHONY: all``，.PHONY 一般來說有兩個目的:<br/>

為了避免定義的規則和工作目錄下的檔案名稱發生衝突，以及改善效能<br/>

makefile 大概就是這樣，之後如果遇到問題就再繼續補充


