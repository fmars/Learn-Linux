### Problem
The terminal prompts following error message when I press tab to auto fill file path. It means my disk is out of usage. So I want to first figure out what is my disk capacity, which files run out of my space, how can I solve it.

```
fmars$ cd /mbash: cannot create temp file for here-document: No space left on device
```

#### 1. Disk capacity
`df -h` shows file system disk space usage.
```
fmars$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            1.9G     0  1.9G   0% /dev
tmpfs           386M   26M  361M   7% /run
/dev/sda1       6.2G  6.1G     0 100% /
tmpfs           1.9G   15M  1.9G   1% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           1.9G     0  1.9G   0% /sys/fs/cgroup
tmpfs           386M   64K  386M   1% /run/user/1000
```
