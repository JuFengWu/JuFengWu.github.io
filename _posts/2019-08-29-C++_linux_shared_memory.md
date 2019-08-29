---
title: C++ linux shared memory
author: Jufeng Wu
layout: post
---

----------------------
兩個執行檔如果要資料交換，除了使用tcp/ip之外，還可以使用shared memory的方式來進行資料交換<br/>

然而使用shared memory進行資料交換要注意關於鎖的機制，以免當其中一個存取的時候，另一個也來存取<br/>
在shared memory的[範例]中(https://github.com/JuFengWu/cpp_examples/tree/master/test_shared_memory)，我們創立了兩個檔案，一個是client,另一個是server<br/>
server寫入資料到shared memory中，client讀此資料（所以server要先打開），然後client在寫*的符號，server看到這一個符號就離開<br/>
先看[client](https://github.com/JuFengWu/cpp_examples/blob/master/test_shared_memory/client.cpp)的程式碼，其中有一段
```
 shmid = shmget(key, SHMSZ, 0666)) < 0 
```
這是 get shared memory 的函數<br/>
其中<br/>
```
int shmget(key_t key, size_t size, int shmflg);
```
的參數意思為:<br/>
```
key: 產生一個新的shared memory區段
size: shared memory的大小，以Byte為單位
shmflg: 其中shmflg有三種參數可以使用，分別為
S_IRUSR : 讀記憶體分段
S_IWUSR : 寫記憶體分段
IPC_CREAT : 確保開啟的記憶體是新的,而不是現存的記憶體.
| 0666 : 作為校驗 ,  ubuntu要加
成功：回傳 shared memory的key
失敗：回傳-1
```
所以在例子中，寫法為
```
if ((shmid = shmget(key, SHMSZ, 0666)) < 0) {
        perror("shmget");//描述錯誤訊息到stderr
        exit(1);
    }
```
至於| 0666，寫法為
```
if ((shmid = shmget(key, SHMSZ,S_IRUSR|S_IWUSR| 0666)) < 0) {
        perror("shmget");//描述錯誤訊息到stderr
        exit(1);
    }
```
再來，會看到另外一個函式shmat <br/>
```
(shm = (char*)shmat(shmid, NULL, 0)) == (char *) -1
```
這是把shared memory映射目前程式中的函式<br/>
其中<br/>
```
void *(int shmid, const void *shmaddr, int shmflg)
```
的參數意思為:<br/>
```
msqid :shared memory的標籤
shmaddr：指定共shared memory在程式中的地址，直接指定为NULL代表讓系統自己決定
shmflg: 如果下SHM_RDONLY表示唯讀，其他為讀寫，所以範例下0
```
接下來來看server的[程式碼](https://github.com/JuFengWu/cpp_examples/blob/master/test_shared_memory/server.cpp)<br/>
可以除了一些判斷之外，剩下跟client大同小異<br/>
唯一差別的是在 shmget 的地方，他使用IPC_CREAT 表示開新的記憶體位置，而不是用舊的<br/><br/>

shared memory 是一個很好用的東西，之後如果有其他的範例（例如使用自定義的變數等），再加進來！
