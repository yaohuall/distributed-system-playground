# distributed-system-playground

## Synchronous system mode
**同步系统模型**中，进程以锁步方式执行；消息传输延迟有已知的上限；每个进程都有准确的时钟。<br>
在同步系统模型下，所有进程按照相同的节奏运行，并且它们之间的通信会受到精密的时间控制，使得整个分布式系统能够非常高效地协同工作。<br>
这样的系统需要确保各个进程在执行某个操作之前都已经达成共识，同时需要协调所有进程的时间和顺序，以避免进程之间的竞态条件和错误发生。

### reference
https://book.mixu.net/distsys/single-page.html
