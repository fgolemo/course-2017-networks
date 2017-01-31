# Lesson 2 - Routing

Reminder:

**User:** root

**Password:**


To start the virtual network with the configuration of the TP2, simply run the following command:

    /net/ens/qemunet/qemunet.sh -x -s /net/ens/qemunet/demo/gw0.tgz

The parameters do the following:
- `-s [filename].tgz` loads the saved session
- `-x` runs the emulation in a terminal instead of full screen (better if you have multiple windows/emulations open)

Here is the network configuration for this exercise:

               grave
                 | .2
             ------------  192.168.100.0/24
                 | .1 (eth0)
    Gateway   immortal
                 | .1 (eth1)             
           --------------  192.168.200.0/24
          .2 |        | .3              
          opeth      syl      

## Simple Routing

- **(A)** Configure the network interfaces of the different machines according to the diagram above. Use the ifconfig command. Be careful not to confuse `eth0` and `eth1`! Check with 'ping' that each pair of machines can communicate.
- **(B)** Display the routing tables with the `route -n` command. Describe what you see.
- **(C)** Configure the routing tables of the different machines using the `route` command. Below are some syntax examples. Check with `ping` that all machines are able to communicate with each other.
- **(D)** Ping between *opeth* and *grave*. Run `tcpdump -i any` on *immortal* to see the traffic. What do you notice?


## Advanced Routing

Here is a new configuration, consisting of the following 4 subnets: 147.210.12.0/24, 147.210.13.0/24, 147.210.14.0/24 and 147.210.15.0/24.



#TODO


# Routing Memento

- Enable routing on a machine (ip forward): `echo 1 > /proc/sys/net/ipv4/ip_forward`
- Show routing table: `route -n`
- Define a default route: `route add default gw <@gateway>`
- Add a route to a specific network: `route add -net <@network> netmask <mask> gw <@gateway>`
- Add a route to a particular machine: `route add -host <@host> gw <@gateway>`
- To delete a rule, you must type the command `route del <…>` with exactly the same arguments as for the add command.
- Check the route: `traceroute ip`
