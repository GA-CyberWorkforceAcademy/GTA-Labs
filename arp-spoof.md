![](media/b80e0eacca6dad9d42b5dc3545946591.png)

ARP Spoofing / Man-in-the-Middle
=================================

Overview
========

This exercise explores the use of ARP spoofing as a means to sniff local network traffic. Modern Local Area Networks (LANs) use ethernet switches, which prevent passive sniffing of network traffic between other components. This lab assumes you have separately learned about the ARP protocol. ARP spoofing is a technique by which the attacker sends spoofed ARP messages into the LAN, with a goal of causing traffic meant for one IP address to be routed to the attacker’s computer instead. The attacker’s computer then forwards the traffic to the intended destination. This puts the attacker into the middle of the traffic exchange, hence the name ”Man in the Middle” attack.

Lab Environment
===============

Once you have logged into your range account and accessed your Labtainer-VM,
open a terminal window.

Navigate to the “labtainer-student” directory and start the lab using the
command:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ labtainer arp-spoof
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-   Links to this lab manual will be displayed if you wish to view the prompt
    from within your VM


Network Configuration
=====================

 This lab includes four networked computers as shown in Figure
>   [1,](#_bookmark0) which illustrates the intended flow of traffic between the user computer and the Webserver via the gateway.

![](media/531868e3dcb6df788fffbd2b105932c8.jpg)

Figure 1: Intended traffic from between User and Webserver

**All 3 attacker terminals are spawned from the same attack host**

Lab Tasks
=========

In this lab, you will use the arpspoof tool to convince the User computer that traffic destined for Gateway should instead be sent to the Attacker computer – and convince the Gateway that traffic destined for the User should be sent to the Attacker computer, as illustrated in
[Figure2](#_bookmark1)

![](media/b96f350b01df9d918613fad019d242b8.jpg)

Figure 2: Man-in-the-middle attack via ARP Spoofing

The arpspoof tool is installed on the Attacker computer, as is Wireshark. The Attacker computer is configured to forward IP packets that is receives which are destined for elsewhere. You can confirm this with this command, which should reflect a value of ’1’:
```
$ sysctl net.ipv4.conf.all.forwarding

```

Task 1: Sniff the LAN from the Attacker
=========

Before you engage in ARP spoofing, first look at network traffic as seen by the attacker. Start wireshark on the attacker computer, selecting the ”eth0”
interface:  

```
$ wireshark -ki eth0
```

On the User computer, use wget to retrieve a web page from the Webserver:
```
$ wget <address of webserver>
```

- Observe the Wireshark display. 

- What type of traffic did you capture? Look back at the network diagram and think about the traffic and where you are positioned.

- Do you see either the web query or the response? Why not?

Task 2: Spoof the ARP cache on the User and Gateway Computers
=========

- Use the arpspoof tool on the attacker computer to perform your ARP spoofing. You will need to move to new terminal tabs in the attacker terminal window (not the one that you have used to start wireshark).

- You must target both the user and gateway computers; this is easiest if you start the arpspoof program in two different "attacker" virtual terminals tabs, with one command in each:
```
2nd terminal tab:
$ sudo arpspoof -t <user IP> <gateway IP>

3rd terminal tab:
$ sudo arpspoof -t <gateway IP> <user IP>
```
After your ARP spoofing has commenced you should see your spoofed ARP traffic in wireshark. 

- Return to the user computer and refetch the webpage, using wget command again. 

- You should see TCP traffic in your wireshark display. 

- Stop the capture, (red button).

- There is a lot of traffic (mostly ARP) in this pcap.  Click the "Protocol" column to sort and scroll to the end of the ARP packets. 

- Do you see the web query or response this time?  (You should, since the attack machine is acting an a Man-in-the-Middle).

**Reflect on how simple this was. Effectively all traffic that the user computer sends/receives through the gateway would be captured by the attacker!**

Submission
==========

After finishing the lab, go to the terminal on your Linux system that was used to start the lab and type:
```
$ stoplab
```
