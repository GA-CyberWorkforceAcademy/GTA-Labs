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

Submission
=====

When the lab is completed, or you’d like to stop working for a while, run:

``
stoplab
``

You can always restart the labtainer to continue your work.
