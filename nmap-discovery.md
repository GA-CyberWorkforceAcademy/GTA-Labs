![](media/b80e0eacca6dad9d42b5dc3545946591.png)

Discovering Services with NMAP
=================================
__This lab was developed for the Labtainer framework by the Naval Postgraduate School,
Center for Cybersecurity and Cyber Operations under National Science Foundation Award
No. 1438893. This work is in the public domain, and cannot be copyrighted.__

Overview
========

This exercise explores the use of nmap to identify services on a target.
Estimated time to complete: 10-20 minutes.

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

Your boss Randall wants you to prepare for a meeting on a project, but you have not worked on it in months! You have a summary file on the “friedshrimp” server that you previously accessed via SSH, however you cannot remember the IP address of “friedshrimp”. To make matters worse, you also forgot which port the IT staff assigned for ssh on that server. They are using a non-standard SSH port for security purposes.

Task: Retrieve your project file!
===============

The good news is you do have SOME information about the target (friedshrimp):

- You IT assigned SSH to a port somewhere in between 2000 and 3000
- You remember that your username and password on the "friedshrimp" server are both “ubuntu” (you've been meaning to change that!)

You are left with only one option: use the nmap to find the IP address and and port number used by the ssh service. You are authorized to use this tool in your environment.  

- After finding the server information, you will need to access the server via SSH review the contents of the “friedshrimp.txt” file to complete the lab. Use the "ls" command to view the files in your present directory, and the "cat" command (followed by the filename) to view the contents of a file. 

If you need any help with the nmap commands, you can use “man nmap” to view the manual. Remember that to SSH to a host via a NON-STANDARD port other (such as the case here), use 
```
$ ssh ubuntu@<host IP> -p <port number>”
```
** Note: You will get a notice that the authenticity of the target host cannot be verified. You will need to accept the fingerprint offered to connect to the server. This is normal.

** Hint: To simplify this, first find the IP of the server by using host discovery with nmap. The "friedshrimp" server is the only other host on your subnet.  Then use a port scan targeting the range 2000-3000 to find the port the service is running on.

When the lab is completed, or you’d like to stop working for a while, run:

```
stoplab
``` 

You can always restart the Labtainer and continue your work. When the Labtainer is stopped. 
