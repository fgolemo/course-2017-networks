Syn Scan

Repeat the first configuration:

/net/ens/qemunet/qemunet.sh -x -s /net/ens/qemunet/demo/gw.tgz
What is a syn scan ?
What is the 'nmap' command for?
From opeth, run the command 'nmap -sS -n @syl' to scan the ports of syl ...
Remember the principle of handshake TCP.
Now start Scapy on opeth .
To make a syn scan with Scapy, try to establish a TCP / IP connection to all ports (1 to 65535) of syl . If a service is available on a port on the target machine, then the server accepts the connection request by responding favorably.

To do a Scapy test, create a TCP / IP packet to port 80 of syl with the TCP flags field equal to "S" (SYN). Send this package and observe the answer.
A = IP (???) / TCP (???)           
 b = sr1 (a)
What is the TCP flag in response.
Repeat for port 3333. What do you notice? To deduce a way to detect an open or closed port ...
Complete the following program, to discover the nile ports (from 1 to 1024) that are open and to display them as nmap.
For p in  range ( 1024 ) :
...   if ???:
...     print  "OPEN" , p
...       
Warning : The test ??? Is not so obvious to find! Google is your friend;-)
