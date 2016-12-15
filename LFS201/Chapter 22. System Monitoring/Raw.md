# Explore how does a process store in /proc
- ls -lF /proc (-F append indicator)
- Run a test python script in background, `./test_proc.py arg1 arg2`

```python
#!/usr/bin/python
while True:
    i = 0
````
- get pid by `ps aux | grep test_proc | less`
- `ls -lF /proc/1792`
- Read tldp, The Linux Documentation Project > Standard File Hierarchy > /proc  (http://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/proc.html)

- cat /proc/pid/cmdline
- cat /proc/pid/environ
- cat /proc/pid/status

- See more use `man proc`

- cat /proc/interrupts

- tunable system parameters `/proc/sys/`
- change a parameter
   - `echo 10000 > threads_max` won't work
   - `sudo echo 10000 > threads_max` won't work 
   - `sudo bash echo 10000 > threads_max` won't work 
   - `sudo bash -c 'echo 10000 > threads_max'` works
   
- ls -lF /sys/class/net/
