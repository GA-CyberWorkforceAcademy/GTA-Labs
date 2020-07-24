![](media/b80e0eacca6dad9d42b5dc3545946591.png)

Telnet Lab
=================================

Overview
========

This labtainer exercise illustrates the use of a telnet client to access
resources on a server. It is a simple lab intended to illustrate basic client
server networking and the transmission of plaintext passwords over a network by
telnet.

Lab Environment
===============

Once you have logged into your range account and accessed your Labtainer-VM,
open a terminal window.

Navigate to the “labtainer-student” directory and start the lab using the
command:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
>   labtainer telnetlab
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-   Links to this lab manual will be displayed if you wish to view the prompt
    from within your VM

Tasks
=====

Determine the server IP address
=====

In the server window, type “ifconfig” to view the IP address of the server. The
server IP address will follow the “inet addr:” label.

Telnet to telnet server and display a file on the server

On the client computer, use the telnet command to access the server using its IP
address:

```
>   telnet \<IP\
```

You will be prompted for a user ID and then a password. Both of them are
“ubuntu” There is a pre-created file on the server named “filetoview.txt”.

View the file content by typing:
``
>   cat filetoview.txt
``
Exit the telnet session on the client via the “exit” command.

View plaintext passwords
=====

On the server, start tcpdump to display TCP network traffic with this command:
```
>   sudo tcpdump -i eth0 -X tcp
```
On the client start a telnet session, but when prompted for the password type
“mydoghasfleas” (as you know this password is incorrect). As you type each
letter of the password, observe the tcpdump of the traffic. Keeping in mind that
every other packet is an “ack”, do you see the password. What do you notice?

Use SSH to protect communications with the server
=====

From the client computer, use the SSH command to access the server using its IP
address:
```
>   ssh \<IP\>
```
The first time you SSH to a server, SSH will warn you that the “authenticity of
the host… can’t be established”. Type “yes” at the prompt.

View the file content by typing:
```
>   cat filetoview.txt
```

Observe the tcpdump output and note that there is no readable plain text.

Stop the Labtainer
==================

When the lab is completed, or you’d like to stop working for a while, run:
```
>   stoplab telnetlab
```
From the host labtainer working directory. You can always restart the labtainer
and continue your work where you left off. 
