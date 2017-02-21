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

