# Signals
- Signal is one mtthod of IPC(inter-process communication) and is used to notify a process about asynchronous events
- Signal is generated through system calls
- Signal can be only sent from the same user or superuser

- Common signals
  - SIGTERM: default signal for kill. Terminate signal. A process can handle it and perform graceful cleanup.
  - SIGKILL: kill a process. Cannot be handled. kill -9
  - SIGSTOP: stop signal. Same as ctrl + z

- kill: is the command to send out signals `kill [signal] [pid]`
- killall: kills all processes with a given name. `killall -9 python`
- pkill: kill with given pattern
