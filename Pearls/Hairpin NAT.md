### Problem
I have clients, a server and a NAT router. I have a external domain name which is served by intern server through port forwarding. Now I can use an internal client to access the internal server by local ip address. However I cannot access it by external domain name, or external ip address.

### Reason
This is because my NAT router is not smart which causes hairpin NAT problem. [A great explanation](http://serverfault.com/a/557776).

### Solution
Not solved yet. Don't know how to config router to make SNAT work or make server routing correctly.

