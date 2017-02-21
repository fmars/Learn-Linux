## Motivation
I'm running my old desktop as a server hosting my homepage. However it suffers from several problems recently. It frequently takes very long time to access my homepage from outside. When I ssh into the server, it's extremely slow, and unstable. Instead of deploying my homepage to AWS again, I'm trying to figure out what's wrong with my old desktop, and if I can fix it.

## Referrences
- [Linux Foundation Certified System Administrator (LFCS)](https://training.linuxfoundation.org/certification/lfcs) Chapter 29. Performance Analysis
- [Sysadmin 101: Troubleshooting](http://northernmost.org/blog/troubleshooting-101/index.html)

## Principles
Following four resources are all the things we need to figure out whatever we do System Monitoring or Performance Analysis
- CPU utilization
- Memory
- Disk I/O
- Network I/O

Starting with throwing potential solutions is not the right way to solve problem. It's a wasting of time. Reproducing the problem, narrowing down, finding the root cause, is the procedure.

## Story
So now I logged into my desktop and started to figure out the problem.
#### CPU
Is something hopping the CPUs?
- `top`
  It doens't seem that any process takes too much CPU
  ```
  top - 18:31:49 up 4 min,  2 users,  load average: 0.22, 0.64, 0.34                              [0/1]
  Tasks: 252 total,   1 running, 251 sleeping,   0 stopped,   0 zombie
  %Cpu(s):  0.5 us,  0.3 sy,  0.0 ni, 99.0 id,  0.2 wa,  0.0 hi,  0.0 si,  0.0 st
  KiB Mem :  3990828 total,  2083996 free,  1140808 used,   766024 buff/cache
  KiB Swap:  4139004 total,  4139004 free,        0 used.  2588224 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                          
      6 root      20   0       0      0      0 S   0.3  0.0   0:00.66 kworker/u8:0                     
     47 root      20   0       0      0      0 S   0.3  0.0   0:00.68 kworker/u8:1                     
    127 root      20   0       0      0      0 S   0.3  0.0   0:00.17 kworker/u8:3                     
    714 root      20   0  556648  19940  15420 S   0.3  0.5   0:00.86 NetworkManager                   
    998 root      20   0  342344  71576  51148 S   0.3  1.8   0:06.19 Xorg                             
   1784 fmars     20   0  217768   7776   6992 S   0.3  0.2   0:00.18 ibus-engine-sim                  
   2162 fmars     20   0  737620  38276  28324 S   0.3  1.0   0:01.06 gnome-terminal-                  
   2645 fmars     20   0   36472   3424   2928 S   0.3  0.1   0:00.01 tmux                             
   2661 fmars     20   0   49220   3800   3016 R   0.3  0.1   0:00.05 top                              
      1 root      20   0  137332   7172   5156 S   0.0  0.2   0:01.18 systemd                          
      2 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kthreadd                         
      3 root      20   0       0      0      0 S   0.0  0.0   0:00.00 ksoftirqd/0                      
      4 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kworker/0:0                      
      5 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/0:0H                     
      7 root      20   0       0      0      0 S   0.0  0.0   0:00.28 rcu_sched                        
      8 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcu_bh                           
      9 root      rt   0       0      0      0 S   0.0  0.0   0:00.11 migration/0                      
     10 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 lru-add-drain                    
     11 root      rt   0       0      0      0 S   0.0  0.0   0:00.00 watchdog/0                       
     12 root      20   0       0      0      0 S   0.0  0.0   0:00.00 cpuhp/0                          
     13 root      20   0       0      0      0 S   0.0  0.0   0:00.00 cpuhp/1     
   ```
   
- `uptime`: average CPU load looks ok
  
  ```
  fmars$ uptime
  18:33:41 up 6 min,  2 users,  load average: 0.23, 0.50, 0.33
  ```
  
  Average CPU load in 5, 10, 15mins are 0.23, 0.50, 0.33.
  
- `vmstat 10` overall CPU utilization looks ok
  ```
  fmars$ vmstat 10
  procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
   r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
   1  0      0 2027048  49600 737604    0    0   411    31  318  856  5  1 86  8  0
   0  0      0 2036392  49600 737604    0    0     0     0 1759 3943  5  1 94  0  0
   0  0      0 2030068  49600 737752    0    0     0     0  793 1975  6  1 94  0  0
   0  0      0 2014708  49600 737752    0    0     0     1  825 2183  7  1 92  0  0
   0  0      0 2008796  49600 737892    0    0     0     0  825 3468  5  1 93  0  0
   0  0      0 2015872  49660 734960    0    0   124   101 1327 3244 12  1 86  1  0
  ```
  In CPU column set, each column stands for user util, system util, idle, wait. Most of time CPUs are in idle state.
  
  #### Memory
  - `free -h`: plenty of memory and swap
  ```
  fmars$ free -h
                total        used        free      shared  buff/cache   available
  Mem:           3.8G        1.2G        1.9G         52M        771M        2.4G
  Swap:          3.9G          0B        3.9G
  ```
  
  - `swapon`: plenty of virtual memory
  ```
  fmars$ swapon
  NAME      TYPE      SIZE USED PRIO
  /dev/sda3 partition   4G   0B   -1
  ```
  
  - `vmstat 5`
  ```
  fmars$ vmstat 5
  procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
   r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
   1  0      0 2001476  49916 722028    0    0   215    19  295  825  5  1 90  4  0
   0  0      0 2001600  49916 722028    0    0     0     0  482  867  0  0 99  0  0
   0  0      0 2001316  49916 722028    0    0     0     0  535 1098  0  0 99  0  0
   0  0      0 2001192  49916 722028    0    0     0     0  488 1024  0  0 100  0  0
   1  0      0 1987436  49916 735920    0    0     0     0  737 2569  2  1 97  0  0
   1  0      0 1982432  49916 735924    0    0     0     0  766 1939  6  1 93  0  0
  ```
  In memory column we can see, there is enough memory and swap space. In swap column, it shows no high frequent swap in/out happens. System interrupt and context switch are normal.
  
#### Disk 
- `iostat 5`: didn't see abnormal behavior
  ```
  fmars$ iostat 5
  Linux 4.8.0-38-generic (ubuntu-servder)         02/20/2017      _x86_64_        (4 CPU)

  avg-cpu:  %user   %nice %system %iowait  %steal   %idle
             5.07    0.04    1.08    3.40    0.00   90.41

  Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
  sda              18.80       711.62        62.99     692504      61297

  avg-cpu:  %user   %nice %system %iowait  %steal   %idle
             4.60    0.00    1.29    0.10    0.00   94.02

  Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
  sda               0.40         2.40         0.00         12          0

  avg-cpu:  %user   %nice %system %iowait  %steal   %idle
             0.75    0.00    0.55    0.00    0.00   98.69

  Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
  sda               2.00         0.00         8.00          0         40
  ```
  
#### Network 
- `sudo iftop`: didn't see anything
- `netstat`: TODO

## Conclusion
Based on investigation so far, I didn't see any exhuastaion happend in either CPU, memory, disk. So I'm suspecting network is unstable because the connection frequetly break when I ssh into the host. So I swapped network adapter of my old desktop and the new one. The old desktop now performs good however the new one stuck. Thus I believe the problem is network adapter is too old to work. And I'm going to get a new one. 


TODO: didn't dig into network performance analyze. Need to learn more and fill this section later. But most likely when some related problem occurs.

  
