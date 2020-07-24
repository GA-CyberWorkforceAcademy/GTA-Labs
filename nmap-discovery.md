![](media/b80e0eacca6dad9d42b5dc3545946591.png)

Discovering Services with NMAP
=================================

Overview
========

This exercise explores the use of nmap to identify services on a target.

Lab Environment
===============

Once you have logged into your range account and accessed your Labtainer-VM,
open a terminal window.

Navigate to the “labtainer-student” directory and start the lab using the
command:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
>   labtainer nmap-discovery
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-   Links to this lab manual will be displayed if you wish to view the prompt
    from within your VM

Scenario
===============

Your boss Randall wants you to prepare for a meeting on a project you have not worked on in months. You have a summary file on the “friedshrimp” server that you previously accessed via ssh; however, you cannot remember the IP address of “friedshrimp”, and you also forgot which port the pesky IT staff assigned for ssh on that server. 

- You know it’s somewhere in between 2000 and 3000. 
- You remember that your username and password are both “ubuntu”. 
- You are left with only one option: use the nmap command to find the IP address and and port number used by the ssh service. 
- After finding that information review the contents of the “friedshrimp.txt” file from an ssh session to complete the Lab.

If you need any help with the nmap commands, you can use “man nmap” to view the manual. Note that in order to ssh to a host via a port other than the default one, use “ssh -p <port> <host>”.
Stop the labtainer.
    
When the lab is completed, or you’d like to stop working for a while, run:

```
stoplab
``` 

You can always restart the Labtainer and continue your work. When the Labtainer is stopped. 
