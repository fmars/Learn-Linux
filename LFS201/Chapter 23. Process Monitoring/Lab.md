## Exercise 23.2 Monitoring Process States
1. Use dd to start a background process which reads from /dev/urandom and writes to /dev/null.
  - dd if=/dev/urandom of=/dev/num &
  - &: run background
  - Ctrl+C: send SIGNINT to kill a process
  - Ctrl+Z: send SIGNSTOP to sleep a process

2. Check the process state. What should it be?
  - top > L dd > &
3. Bring the process to the foreground using the fg command. Then hit Ctrl-Z. What does this do? Look at the process state again,
what is it?
  - job
  - fg 1
4. Run the jobs program. What does it tell you?
5. Bring the job back to the foreground, then terminate it using kill from another window.
