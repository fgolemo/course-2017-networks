# Lesson 6 - dealing with Windows machines

Start the emulator with this command:

    /net/ens/qemunet/qemunet.sh -s /net/ens/qemunet/demo/win.tgz

This is the topology of the network you just started:

               win10
                 |
           --------------   
                 | (eth1)
    gateway   immortal
                 | (eth0)
           --------------  
             |        |
            nile     syl  

The login for the Linux machines is "root" as usual, and the windows machine has user "**foo**", password "**toto**".

In order to do anything on windows, you should first


    Note : Under Windows, we will use the command prompt in administrator mode - the equivalent of a Linux shell. To do this, press the windows key (or click the windows logo bottom left), type in `cmd`, and right-click on this program to select: Run as Administrator.


- **(A)** What is the IP address of the Windows machine? What is its network mask? Look in the control panel.
- **(B)** On Windows: run the command prompt in Administrator mode. Using commands `ipconfig.exe` and `netsh.exe`, view and change the IP address of the Windows machine and give him the address 192.168.0.2/24. Write down the commands you used in your report, and show me a screenshot of the new IP set in the windows control panel.
- **(C)** Configure the `immortal` Linux machine in the same network as the Windows machine (`immortal` gets IP 192.168.0.1 on eth1). Ping `win10` to `immortal` and vice versa. What do you notice?
- **(D)** Disable the firewall of the Windows machine in Control Panel> System and Security> Windows Firewall . Check that the pings are working in both directions ...
- **(E)** Now configure all Linux machines: IP addresses and routing table. (`immortal` gets 192.168.1.1 on eth0, `nile` gets 192.168.1.2 and `syl` gets 192.168.1.3)
- **(F)** On `win10`, run the command prompt again in Administrator mode. Add a default route to the Windows machine by using the `route.exe` command. Check with ping that all machines can talk to each other. Show me the `route` command you used a screenshot of `nile` successfully pinging win10.
- **(G)** Activate the firewall again. In the advanced settings of the Windows Firewall add a rule to allow incoming ping. To do this, select Incoming Traffic Rules and then add a New>Custom Rule, which allows the ICPMv4 protocol. Ensure that Linux machines can ping Windows. Show me a screenshot.
- **(H)** We now want to run a `netcat` server on the `win10` machine and a client on the Linux machine. This interactive program allows you to simply exchange text between client and server (as you know). On Windows with the command prompt start the server program with `nc -l -p <port>`. Note that the program is located on the user "foo"'s desktop.
Windows asks if you want to add an exception in the firewall to allow inbound connections to this program. Answer Ok.
Now run the client on the machine `nile`: `nc <ipserver> <port>`. And verify that the messages you type are displayed on the server.

## Bonus

- **(I)** What is the disadvantage of this exception in the Windows firewall?
- **(J)** Delete the rule in Control Panel> System and Security> Windows Firewall> Allowed Applications.
In the advanced settings of the Windows Firewall now add a custom rule to allow `netcat` only on the listening port 8888 (TCP) and repeat the test with this port.
