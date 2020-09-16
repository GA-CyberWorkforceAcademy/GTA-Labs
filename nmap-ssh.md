![](media/b80e0eacca6dad9d42b5dc3545946591.png)

Recon and Exploitation (w/NMAP and TCPDUMP)
=================================

Overview
========

This labtainer exercise uses nmap and skills exercised in previous labtainer
labs to identify and exploit a weakness in a system.

Lab Environment
===============

Once you have logged into your range account and accessed your Labtainer-VM,
open a terminal window.

Navigate to the “labtainer-student” directory and start the lab using the
command:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
>   labtainer nmap-ssh
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-   Links to this lab manual will be displayed if you wish to view the prompt
    from within your VM.
    
    
- You will see (2) virtual terminals “analyst@mycomputer” and "analyst@router". Pay close attention to which terminal you are using.
- The nmap utility is pre-installed on the "mycomputer" terminal. 
- The "router" terminal sits between the organization’s client workstations (including your "mycomputer" workstation) and the servers.

Scenario
===============

This labtainer exercise uses nmap and tcpdump along with and skills exercised in previous labs to identify and exploit a weakness in a system.

You are performing ad-hoc security testing for a client who believes their internal SSH server is relatively secure, but you would like to confirm the
validity of this. Your goal is to attempt to remotely access that SSH server and disclose the contents of a selected file.

Tasks
===============

You have been told the target SSH server's IP address is 172.25.0.2 and the SSH port number changes frequently, within the range of 2000-3000. You have been
given an account, “analyst” on the client "mycomputer" and on the "router".

The network is situated like this: 
Client computers <===> [Router]<===> servers

- Your goal is to successfully SSH from “mycomputer” into the SSH server. You have information indicating that the organization uses the default login of "ubuntu" with a common, unknown password. They use this on multiple servers.

Information to consider:

-   nmap is installed on "mycomputer"

-   tshark and tcpdump are installed on the "router"

-   There are other password protected network services being used on the network...this should be a starting point for determining the password for the "ubunutu" login used across the organization.

- The password used with "ubuntu" is alpha-numeric, relativly short, but hard to guess without a password cracker.


Hints
======

- Figure out what the server's subnet is. (The router is connected to it, check there)
- Discover hosts on the server network with nmap (from mycomputer)
- Figure out what services those servers are running with nmap (from mycomputer)
- Start snooping on traffic that you may be able to use (on the router)
- Be specific when you are sniffing traffic.  And use "-vv" flag for verbosity/details, and the "-X" flag to view the ascii interpretation of the hexcode for each packet.
- Try using "egrep" to locate the string "ubuntu" indicating that a logon is happening
-  View a segment/number of packets that flow in after that with the -A<number of lines to view>; this narrows down the traffic you are looking at.

Example-from the router

```

$ tcpdump -i eth1 dst port <port number> -vvX | egrep -A15 ubuntu

```
Submission
=====

When the lab is completed, or you’d like to stop working for a while, run:

``
stoplab
``

You can always restart the labtainer to continue your work.
