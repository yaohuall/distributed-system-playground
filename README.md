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

## Apache Spark
### Deploy mode
Client: If submit your application from a gateway machine that is physically co-located with your worker machines (e.g. Master node in a standalone EC2 cluster). In this setup, client mode is appropriate. In client mode, the driver is launched directly within the spark-submit process which acts as a client to the cluster. The input and output of the application is attached to the console. Thus, this mode is especially suitable for applications that involve the REPL (e.g. Spark shell). **Spark standalone mode doesn't support cluster mode**.

Cluster: If application is submitted from a machine far from the worker machines (e.g. locally on your laptop), it is common to use cluster mode to minimize network latency between the drivers and the executors. **In spark client mode, the driver process must be up and running the whole time when the spark job is in execution. So if you have a truly long job that say take many hours to run, you need to make sure the driver process is still up and running, and that the driver session is not auto-logout.** **After submitting a job to run in cluster mode, the process can go away. The cluster mode will keep running. So this is typically how a production job will run: the job can be triggered by a timer, or by an external event and then the job will run to its completion without worrying about the lifetime of the process submitting the spark job.**

**In client mode, you can call sc.collect() to gather all the data back from all executors, and then write/save the returned data to a local Linux file on local disk.** **Now this may not work for cluster mode, as the 'driver' typically run in a different remote host.** The data written up therefore need to be persisted in a common mounted volume (such as GPFS, NFS) or in distributed file system like HDFS. If the job is running under Hadoop/YARN, the more common way for cluster mode is simply ask each executor to persist the data to HDFS, and not to run collect( ) at all. Collect() actually have scalability issue when there are a large number of executors returning large amount of data - it can overwhelm the driver process.

if in the **same local network with the cluster, it's better using the client mode and submit it from local machine.** If the cluster is far away, either submit locally with cluster mode, or rsync the jar to the remote cluster and submit it there, in client or cluster mode, depending on how heavy the driver program is on resources.*

### reference
https://book.mixu.net/distsys/single-page.html <br>
https://www.cnblogs.com/yeyang/p/13576636.html <br>
