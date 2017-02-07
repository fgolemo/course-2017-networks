# Lesson 3 - Network Tools/Utilities

First download the file `gw3.tgz` from this repository and save it in your home directory.

Then start the emulator with this command:

    /net/ens/qemunet/qemunet.sh -x -s ~/gw3.tgz  

You have the following network, IP addresses and routing tables are already configured.


               grave
                 | .2
             ------------  192.168.100.0/24
                 | .1 (eth0)
    gateway   immortal
                 | .1 (eth1)             
           --------------  192.168.200.0/24
          .2 |        | .3              
          opeth      syl  

## Various Tools (nmap, netstat, netcat)

- **(A)** Using the `nmap` command on *immortal* (with options), try to discover the computers in the 192.168.200.0/24 network that are online. Again, on *immortal* try to discover the network services available on a remote machine, for example *opeth*. Show me the nmap output for that and explain what you see.
- **(B)** On *opeth*, use the `traceroute` command to discover the way to *grave*. How does this program work?
- **(C)** By using the `netcat` command on the `grave` machine as a server, and `opeth` as a client, establish a TCP/IP connection. Run the command `tcpdump -n -A -i any` on `immortal` beforehand and observe the frames exchanged when establishing the connection (SYN / SYN-ACK / ACK). Show me the output of tcpdump and netcat, and explain what you see.
- **(D)** (SKIP THIS QUESTION FOR NOW. DO NOT ANSWER THIS QUESTION BECAUSE ON THE EMULATED MACHINES THERE IS A PROBLEM WITH NETSTAT) Using the command `netstat` with options, display the state of TCP connections (LISTEN, ESTABLISHED) on the `grave` machine. Do this again when netcat is started. What do you notice?

**HINT** to get the documentation of these tools, type into the shell `man nmap` or `man tcpdump`,...  Just `man [command]` and hit `ENTER`.


## Telnet and Wireshark

`telnet` is the ancestor of the program `ssh`, which can connect remotely to a machine. Each virtual machine has an account **toto** (password **toto**). Unlike ssh, telnet is not secure: all communications are carried out "in the open", including sending the password! I want to show you this now.

On `immortal`, run the following command to capture all network traffic:

    Root@immortal# tcpdump -i any -w /mnt/host/capture
    |     |      |
    |     |      ^ --- this means you are connected to a shell
    |     ^ --- computer you are working on (here: immortal)
    ^ --- your username (here: root)

Now login to `grave` and from there connect to `syl`:

    Root@serious# telnet 192.168.200.3 -l toto
    Toto@syl$ logout
    Root@serious#

That's it. Now go back to `immortal` and stop capturing with `CTRL+c`. The file with the tcpdump recording at `/mnt/host/capture` can be opened on the **HOST computer** (your real lab computer where you started QemuNet) in the directory `session/immortal`. If you don't know where to find the `session` folder, ask me. Now open this file with the program `wireshark`.

- **(E)** Describe what you see. Which port does `telnet` use?

- **(F)** Select a TCP/IP frame and with a right click, 'Follow TCP Stream'. Check that you have captured the password toto! Highlight the password and show me a screenshot of you finding the password.




## ARP

- **(G)** What is the ARP protocol used for? Explain the basic idea of how it works.
- **(H)** Using the `arp -n` command, display the ARP table. Interpret the different fields.

## BONUS: ARP Spoofing

The ARP spoofing attack is a simple attack that targets Ethernet local area networks. It consists in intercepting the traffic between two machines in a transparent way for the latter (man-in-the-middle attack).

- **(I)** What is the principle idea of an ARP spoofing attack?
- **(J)** Set up this attack using the `arpspoof` command (only installed on `immortal`), so that immortal intercepts traffic between `syl` and `opeth`. Using the `tcpdump` command, highlight the operation of this attack.

For an introduction to the arpspoof command and how to use it, check out this website:
https://pdworks.wordpress.com/2009/03/29/arpspoof-for-dummies-a-howto-guide/
