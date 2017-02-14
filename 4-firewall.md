# Lesson 4 - Firewall / iptables

Start the emulator with this command:

/net/ens/qemunet/qemunet.sh -x -s /net/ens/qemunet/demo/dmz.tgz

Here is the network configuration.

                  opeth    grave
        INTERNET    \       /
                     \     /
                      \   /
           -------- immortal --------
                      /   \
               DMZ   /\    \   
                    /  \    \
                  syl  dt  nile

IPs and routing tables already configured for you :-)

## Firewall

The firewall will be configured on the immortal gateway.

**For all of this weeks tasks, show me your command(s), and also show me that it works (i.e. the result of a `ping` or a `telnet` command)**

- **(A)** Draw the IPs and network interfaces of all machines in a diagram. Check with ping that everyone can communicate together.
- **(B)** Set the default policy to DROP on all chains. (First read below what a policy and what a chain is).
- **(C)** Allow ping to the internet, but prohibit the reverse.
- **(D)** Allow access to the web to internal network machines, i.e. allow all DMZ machines to request and receive port 80 and port 443 TCP traffic (which is HTTP and HTTPS respectively)
- **(E)** Allow `grave` access to the `ssh` server (port 22) of `dt`. Try it.
Allow access to the web server of `syl`. Test with `telnet` or `netcat`.
- **(F)** From `opeth` test your firewall with `nmap`!

## IPtables crash course

To check the current configuration of your firewall (show all rules), use this command:

    iptables -n -L -v --line-numbers

    -L is "list the rules"
    -n means "with IPs instead of domain names"
    -v is verbose (more detailed output)
    --line-numbers... is pretty self-explanatory

Delete a specific rule from a specific chain (here delete rule in line 4 from chain INPUT):

    iptables -t filter -D INPUT 4

The configuration is based on the `filter` table (there are also other tables, but we won't discuss them here) and is split into 3 groups, called **"chains"**:

- INPUT: everything that goes into the machine;
- OUTPUT: everything that comes out of the machine;
- FORWARD: everything that goes through the machine (i.e. during routing).

To delete all added rules:

    iptables -t filter -F

    -F for "flush", i.e. delete everything

For each rule that is added, three actions are possible (denoted <POLICY>):

- ACCEPT: we accept;
- REJECT: politely rejects (error response sent to the sender);
- DROP: throws package in the trash (no error response).

To change the default firewall policy:

    iptables -t filter -P <CHAIN> <POLICY>

To add a new rule to a firewall string (pay attention to the order of the rules):

    iptables -t filter -A <CHAIN> <SRC> <DST> <...> -j <POLICY>

- With `<SRC>` indicating the origin of IP packets, such as `-i eth0` or `-s 192.168.0.0/24` or `-s 0/0`;
- With `<DST>` indicating he destination of the IP packets, for example: `-o eth1` or `-d 147.210.0.0/24`;
- With `<...>` additional information, for example on the nature of the protocol `-p icmp` or `-p tcp`, possibly with additional information specific to these protocols (`-dport 80` for TCP) `-m state -state NEW`, ...
- For more information, refer to the manual: `man iptables` or google your question :)
