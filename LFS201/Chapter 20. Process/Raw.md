# Process
- A process is a running program with resources including environment, files, signal handlers and etc.
- pid, ppid, pgid
- init: is the first user process which pid is 1
- kthreadd: will become the parent process of a process whose parent process terminates before it
- ulimit: to set resource limit that all processes can use in a shell

- Process status:
  - running
  - sleeping
  - stopped
  - zombie
  
- user mode and kernel mode
- Daemons: names often end with 'd'
  - httpd
  - sshd
  - inited from /etc/init.d/
  
- kernel created process:
  - has kthreadd (pid=2) as parent
  - in ps -elf names are encapsulated in square brackets such as sigmask_new
  
- create a process from bash
- nice and renice: priority of a process

- static and shared library
- ldd: ascertain shared library dependencies of a program
- shared library searching path: 
  - ldconfig builds /etc/ld.so.conf.d/
  - $PATH
  - $LD_LIBRARY_PATH
