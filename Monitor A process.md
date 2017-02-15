## Context
I went through system monitoring chapters including process, memory, io. I learnt several commands however I didn't  really grasp them until I happpened to face one performance problem.

## Motivation
I need a tailer program to tail data from a remote storage system. But I also need to ensure that the tailer program could fetch data fast enough to meet performance requirements. So I need to understand the performance of a tailer program, what's the bottleneck, and if we can improve.

## Scenario
We take a multithread download program as an example to demonstrate how we monitor the performance of a program.

## Steps
#### How to get basic host infos
- number of CPUs
```
lscpu
```
- Size of physical memory
```
free
cat /proc/meminfo
```

- Q: 
  A: 
