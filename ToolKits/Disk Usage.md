### Problem
The terminal prompts following error message when I press tab to auto fill file path. It means my disk is out of usage. So I want to first figure out what is my disk capacity, which files run out of my space, how can I solve it.

```
fmars$ cd /mbash: cannot create temp file for here-document: No space left on device
```

#### 1. Disk capacity
`df -h` shows the disk usage of each file system.
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
/dev/sda1 is the hard disk we're using which is mounted to /. Now it is exhausted. 

udev is the file system to handle new device insert and remove. It is mounted to /dev

tmpfs are file systems in memory such as /run. Also /dev/shm where data will be lost if power off.


Now we know the disk is full. next we want to know who runs most of our disk.

### 2. Disk usage
`du` shows the disk usage of each folder and file
```
f@f-VirtualBox:~$ sudo du -h -d 1 / 2>&1 | grep -v cannot
0	/sys
16K	/lost+found
424M	/lib
6.5M	/run
11M	/bin
432M	/var
4.0K	/srv
60M	/boot
4.0K	/cdrom
4.0K	/mnt
8.0K	/opt
52K	/tmp
3.0G	/usr
0	/proc
4.0K	/lib64
4.0K	/media
48K	/root
136K	/dev
22M	/home
14M	/sbin
13M	/etc
4.0K	/snap
4.0G	/
```
(Thanks to stupid virtual box, I have lost my previous VM. This is the new one I created, so out of disk issue never exists. But I'm still following the same process to pinpoint the problem to pretend there is one.)

Now we found /usr is the largest one. So we further dig into that folder. `sudo du -h -d 1/usr/lib`
The problem we realized is, since we installed too many packages which run out of disk space. Next step, how to deal with those packages.

### 3. Package info
- `which vim /usr/bin/vim`
- `ldd /us/bin/vim` check all libraries a package depends on
- `dpkg - s` shows size and dependencies'

   ```
    f@f-VirtualBox:~$ dpkg -s vim
    Package: vim
    Status: install ok installed
    Priority: optional
    Section: editors
    Installed-Size: 2432
    Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
    Architecture: amd64
    Version: 2:7.4.1829-1ubuntu2.1
    Provides: editor
    Depends: vim-common (= 2:7.4.1829-1ubuntu2.1), vim-runtime (= 2:7.4.1829-1ubuntu2.1), libacl1 (>= 2.2.51-8), libc6 (>= 2.15), libgpm2 (>= 1.20.4), libpython3.5 (>= 3.5.0~b1), libselinux1 (>= 1.32), libtinfo5 (>= 6)
    Suggests: ctags, vim-doc, vim-scripts
    Description: Vi IMproved - enhanced vi editor
     Vim is an almost compatible version of the UNIX editor Vi.
     .
     Many new features have been added: multi level undo, syntax
     highlighting, command line history, on-line help, filename
     completion, block operations, folding, Unicode support, etc.
     .
     This package contains a version of vim compiled with a rather
     standard set of features.  This package does not provide a GUI
     version of Vim.  See the other vim-* packages if you need more
     (or less).
    Homepage: http://www.vim.org/
    Original-Maintainer: Debian Vim Maintainers <pkg-vim-maintainers@lists.alioth.debian.org>
  ``` 
- `dpkg -L vim` shows all the files owned by the package

   ```
    f@f-VirtualBox:~$ dpkg -L vim
    /.
    /usr
    /usr/bin
    /usr/bin/vim.basic
    /usr/share
    /usr/share/bug
    /usr/share/bug/vim
    /usr/share/bug/vim/presubj
    /usr/share/bug/vim/script
    /usr/share/doc
    /usr/share/lintian
    /usr/share/lintian/overrides
    /usr/share/lintian/overrides/vim
    /usr/share/doc/vim
   ```
- `dpkg -S` shows which package a file belongs to 

    ```
    f@f-VirtualBox:~$ dpkg -S /usr/bin/vim.basic
    vim: /usr/bin/vim.basic
    ```
- List all the package and size ans sort them by size `dpkg-query -W -f '${Installed-Size}\t${Package}\n' | sort -k 1 -n`
