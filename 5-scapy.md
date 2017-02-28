# Lesson 5 - Scapy, modifying packages

First download the file `gw3.tgz` from this repository and save it in your home directory. (You should have done this already for exercise 3).P

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
            opeth     syl    

**Scapy** is a Python command interpreter that makes it possible to forge IP packets. You will find documentation and a tutorial on Scapy here:

http://www.secdev.org/projects/scapy/demo.html

or a bit more modern

http://www.secdev.org/projects/scapy/doc/index.html
http://www.secdev.org/projects/scapy/doc/usage.html

On `opeth`, run the `scapy` command. The prompt `>>>` indicates that you are now in the Scapy interpreter. To quit Scapy, hit <kbd>ctrl</kbd>+<kbd>d</kbd>.

## Ping & Pong

To forge an IP packet, just call the `IP()` constructor. The `show()` method shows all IP packet fields initialized with default values:

    x = IP()
    x.show() # after this command you will see some output

To ping `syl`, an `echo-request` must be sent via the ICMP protocol over IP. Type the following commands:

    ping = IP( dst="192.168.200.3" ) / ICMP( type="echo-request" )
    pong = sr1( ping ) # if you can't read this properly, the command is sr one, not sr L

- **(A)** What are the ping and pong variables - what do they do? What is the sr1() function good for?
- **(B)** Use the `show()` method of both `ping` and `pong` to display the contents of the packages. Note the encapsulation of ICMP in IP. Show me the output of the `show()` method for both variables and comment on each line what you think it means.
- **(C)** Now, do a ping towards `grave` in the same way and show me the output of the `pong.show()`

## Traceroute

In this exercise, we will code a manual traceroute into scapy. To do this, we will use the virtual network chain from TP2. Launch this topology:

    /net/ens/qemunet/qemunet.sh -x -s /net/ens/qemunet/demo/chain.tgz

IPs and routing tables are already configured.

First, test the traceroute command between opeth and nile.
Start scapy on opeth . Test the code below:

    x = IP( dst="147.210.15.2", ttl=1 ) / ICMP()  # @nile
    y = sr1(x)
    y.show()

- **(D)** What does the TTL field represent? Who answered? Why? Detail the encapsulation of the response received.
- **(E)** Repeat with `ttl=2`. What do you notice?
- **(F)** By relying on this principle, write a scapy program that performs a traceroute!

Tip: To write a Python loop ...

    >>>  for i in  range(10):
    ...    print  "iteration", i
    iteration 0
    iteration 1
    ...
    iteration 8
    iteration 9


Attention : Do not forget to add one or two spaces after '...' in the body of the loop (after the line `for` and before the `print` function).
