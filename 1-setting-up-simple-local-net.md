# Lesson 1

Fire up the network simulator based on a saved session for this first exercise:

(assuming you are still in the qemunet/trunk directory)

    ./qemunet.sh -s ~/course-2018-networks/lan0.tgz

Here is the network configuration used for TP1, a local area network (LAN) interconnected via the Ethernet switch s1.

    grave   opeth
         \ /
        [s1]
         / \
      syl   immortal

Once the script has started you will see a VM (Virtual Machine under QEMU) for each of those 4 machines mentioned above. Each VM has their own terminal window that you can log in to.

**User:** root

**Password:**

## Configuring Network Interfaces with ifconfig

Once the 4 VM terminals have appeared (be careful, the 4 windows can be stacked): log in as root on immortal (without password). If you have to switch your keyboard layout, you can do that with


    dpkg-reconfigure keyboard-configuration

Now check out the `ifconfig` command (you can get an overview with `man ifconfig`).

- **(A)** List the available network interfaces.

Now please configure the network interface eth0 so that immortal has the IP address 172.16.0.1 on the network 172.16.0.0/24.

- **(B)** What is the class of this address?
- **(C)** What is the network mask?

- **(D)** Deduce all IP addresses from this network.

Configure immortal using the ifconfig command. Configure the other 3 machines in the same way. (grave: 2, syl: 3, opeth: 4)

Check your configurations using the ping command.

- **(E)** What is the protocol used by the ping program?

Look at the network traffic using the command (on any machine) `tcpdump -i eth0` which displays all the network traffic in and out of a certain machine (on eth0). Then ping the machine that is running tcpdump.

Try pinging the network broadcast address.

- **(F)** Describe what is happening.

Use this command on all machines:

    echo 0 > /proc/sys/net/ipv4/icmp_echo_ignore_broadcasts

Re-try a ping with the broadcast address of the network.

- **(G)** Describe what is happening now?

## Using the /etc/network/interfaces file

With the `reboot` command, restart a VM. Check that the eth0 interface is no longer configured correctly.
The file `/etc/network/interfaces`  permanently configures different network interfaces of a machine. Fill this file correctly for the different machines. Don't forget the loopback interface.

- **(H)** What does the sequence `iface eth0 inet static` do?

- **(I)** Specify the meaning of the following keywords: `address`, `netmask`, `network`, `broadcast`?
- **(J)** What is the role of the keyword 'auto'? For this you can consult the manual: man interfaces.

This file is interpreted at boot time, or when you call the script `/etc/init.d/networking restart`. Use the second method to interpret this file and verify that it has been taken into account.
Reboot the virtual machine with the command reboot. Check your configuration again.

## Using the /etc/hostname file

The file `/etc/hostname` contains the name of the machine. Just change it and make a reboot and the VM will have its name changed.
On VM immortal:

    echo immortal > /etc/hostname

then reboot. Do the same with the other 3 VMs.

## Using the /etc/sysctl.conf file

The file `/etc/sysctl.conf` configures sysctl - particularly for fine adjustments on ipv4.
To have a response to the ping broadcast, add this line at the end of the file: `net.ipv4.icmp_echo_ignore_broadcasts = 0`
Edit the file and reboot.
Try pinging again with the broadcast address.
That's it, well done. :)

To leave this QEMU environment cleanly, you must run the 'poweroff' command on each machine. To archive all machines with their configurations, you must save the contents of your session directory:

    cd session ; tar cvzf mysession.tgz * ; cd -

You can then move this archive to your $HOME for future use. To restore your saved session, simply do the following:

    qemunet.sh -s mysession.tgz
