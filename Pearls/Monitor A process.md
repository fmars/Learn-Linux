## Context
I'm running a download program. It downloads a file at some speed. I want to know if I can increase that speed. So I need first profile the download process, have some sense of the performance and figure out what's the bottleneck of the process or system. Then decide what can I can do make it perform better.
(I went through system monitoring chapters including process, memory, io. I learnt several commands however I didn't  really grasp them until I happpened to face one performance problem, tried to solve it and got my hands dirty.)


## Motivation
I need a tailer program to tail data from a remote storage system. But I also need to ensure that the tailer program could fetch data fast enough to meet performance requirements. So I need to understand the performance of a tailer program, what's the bottleneck, and if we can improve.

## Scenario
We take a multithread download program as an example to demonstrate how we monitor the performance of a program.

## Steps
#### Host basic infos
- Number of CPUs. 

   `lscpu`. 
   
    Note: #CPU = Cores_per_socket * #socket * Threads_per_core
- Size of memory, physical memory and virtual memory 

   `free - h;`
   
   `cat /proc/meminfo`
   
   `swapon`
 
#### Start the testing program
```
f$ axel -a -n 2 http://download.thinkbroadband.com/1GB.zip
Initializing download: http://download.thinkbroadband.com/1GB.zip
File size: 1073741824 bytes
Opening output file 1GB.zip
Starting download

[  1%] [0                                                             .1                                                             ] [   1.2MB/s] [13:56][  1%] [0                                                             .1                                                             ] [   1.2MB/s] [13:46][  1%] [0                                                             .1                                                             ] [   1.2MB/s] [13:42][  2%] [0                                                             .1                                                             ] [   1.2MB/s] [13:36]
```
axel is a multithread downloading program. And we test to download a 1GB testing file with 2 threads. 

#### Monitoring
The porcess id is 2421
```
f$ ps -aux | grep axel
f         2421  0.0  0.1 182084  3608 pts/17   S+   15:15   0:00 axel -s 100 -a -n 2 http://download.thinkbroadband.com/1GB.zip
```

- Number of thread used
   ```
   f$ cat /proc/2421/status | grep Thread
   Threads:        1
   ```
   
- Process CPU and memory usage
   ```
   top -p 2421
   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                               
   2713 f         20   0  182084   3584   3064 S   0.0  0.2   0:00.00 axel  
   ```
   Axel almost takes no CPU, It uses around 177M virtual memory and 3.5M physical memory and 3.0M of which is shared labiary.
   
- System CPU and memory usage
   ```
   f$ vmstat 2
   procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
    r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
    1  0  20916 133012 100540 358292    0    6   199    30  436 1051 12  2 86  0  0
    1  0  20916 125164 100548 365888    0    0     0 17804 3568 5568 12  4 82  2  0
    0  0  20916 117536 100548 373416    0    0     0     2 2240 6165 10  3 88  0  0
    1  0  20916 110260 100556 380940    0    0     0    20 3277 5683 10  3 87  0  0
    1  0  20916 102940 100556 388472    0    0     0     0 2382 5034  8  2 90  0  0
    0  0  20916  95308 100556 395968    0    0     0     0 3285 5878 11  3 86  0  0
    0  0  20916  87952 100564 403468    0    0     0 17824 2661 5943  9  3 87  2  0
    0  0  20916  80296 100564 410944    0    0     0     2 2783 5638 11  3 87  0  0
    0  0  20916  86172 100352 404988    0    0     0    20 3166 6505 12  3 85  0  0
    0  0  20916  78852 100356 412580    0    0     2  9628 2781 5367 10  3 86  0  0
    1  0  20916  85992  99464 406656    0    0     0     0 2434 5330 11  3 86  0  0
    ```
    Most of time CPU stays in idle status. Few page swap happens. Around every 10 seconds, a good amount of blocks been sent to block device which is hard disk in this case.

- System disk usage
   ```
   f$ iostat 5
   Linux 4.8.0-38-generic (f-VirtualBox)   02/20/2017      _x86_64_        (4 CPU)

   avg-cpu:  %user   %nice %system %iowait  %steal   %idle
             11.96    0.06    2.39    0.38    0.00   85.21

   Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
   sda              47.64       697.94       522.37     719455     538472

   avg-cpu:  %user   %nice %system %iowait  %steal   %idle
              6.88    0.00    2.17    0.05    0.00   90.90

   Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
   sda              10.20         1.60      7269.60          8      36348

   avg-cpu:  %user   %nice %system %iowait  %steal   %idle
              5.61    0.00    2.46    0.05    0.00   91.88

   Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
   sda               1.20         0.00        49.60          0        248

   avg-cpu:  %user   %nice %system %iowait  %steal   %idle
             22.35    0.00    7.40    0.00    0.00   70.26

   Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
   sda              12.80         0.00      7368.00          0      36840

   avg-cpu:  %user   %nice %system %iowait  %steal   %idle
              5.18    0.00    1.88    0.89    0.00   92.05

   Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
   sda               1.20         0.00       131.20          0        656

   avg-cpu:  %user   %nice %system %iowait  %steal   %idle
              3.77    0.00    1.89    0.05    0.00   94.29

   Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
   sda              10.40         0.00      7466.40          0      37332
   ```
   Looks similar, arond every 10 senconds, data will be flushed into disk. 
   
- System network usage
   ```
                                     19.1Mb                         38.1Mb                         57.2Mb                         76.3Mb                   9[0/0]
   +------------------------------+------------------------------+------------------------------+------------------------------+------------------------------
   f-VirtualBox                                                    => download.thinkbroadband.com                                       334Kb   317Kb   292Kb
                                                                   <=                                                                  27.8Mb  28.9Mb  27.7Mb 
   f-VirtualBox                                                    => cdns02.comcast.net                                                  0b      0b    312b  
                                                                   <=                                                                     0b      0b    298b
   f-VirtualBox                                                    => cdns01.comcast.net                                                  0b      0b    148b
                                                                   <=                                                                     0b      0b    126b
   f-VirtualBox                                                    => gateway                                                             0b      0b     24b
   ```
   
#### Conclusion
The downloading process is network bound program. All other system resources are far away from exhuasting, CPU, Memory and Disk. As you can eee, VirtualBox receives data from download.thinkbroadban.com at 28Mb/s rate which is around 3MB/s. Since we know that's the network bandwidth, we know the the bottleneck of this process is network. If we get larger bandwidth network, we can increase the download speed.
