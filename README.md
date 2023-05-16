# distributed-system-playground

## Synchronous system mode
**同步系统模型**中，进程以锁步方式执行；消息传输延迟有已知的上限；每个进程都有准确的时钟。<br>
在同步系统模型下，所有进程按照相同的节奏运行，并且它们之间的通信会受到精密的时间控制，使得整个分布式系统能够非常高效地协同工作。<br>
这样的系统需要确保各个进程在执行某个操作之前都已经达成共识，同时需要协调所有进程的时间和顺序，以避免进程之间的竞态条件和错误发生。

## 系統操作原子性
即一个操作或者多個操作，要麼全部執行并且執行的過程不會被任何因素打斷，要麼就都不執行。<br>
A Java example:

    i = 0;       //1. 在Java中，對基本數據類型的变量和賦值操作都是原子性操作； 
    j = i ;      //2. 包含了兩個操作：讀取i，將i值賦值給j 
    i++;         //3. 包含了三個操作：讀取i值，i + 1 ，將 +1 結果賦值給i； 
    i = j + 1;   //4
    
In singel-thread: All 4 are atmoicity <br>
In multi-thread: Only 1 is atmoicity <br>

### reference
https://book.mixu.net/distsys/single-page.html <br>
https://www.cnblogs.com/yeyang/p/13576636.html <br>
