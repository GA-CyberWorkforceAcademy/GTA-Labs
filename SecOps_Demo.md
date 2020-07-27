DEV/TCP Connect Demo
=====

Netcat is a utility that is generally used to read of write information from network connections. Because of the many ways that this utility can be exploited by adversaries, it is highly 
recommend that it be removed from production servers. That being said, given access to a machine, even without netcat installed, you can use the principles upon
which it operates to transfer information, interact with application or port scan (just like you can do with netcat!)


*Follow the below steps*



Custom Interactions with /dev/tcp
=====

 
1) Log into the Ubuntu VM.  You will not need a Labtainer/container for this demo. You can make yourself root, or use sudo as invoke the command.

2) Create the following bash script on the machine to pull the webpage from the GTA Lab Prompt code repository:



```

#!/bin/bash

exec 3<>/dev/tcp/10.2.0.4/80

echo -e "GET /index.html/ HTTP/1.1\r\nhost: ga-cyberworkforceacademy.github.io/GTA-Labs\r\nConnection: close\r\n\r\n" >&3

cat <&3

```

3) Simply run the script, and you should be presented with the webpage index for this course's Lab Prompt Index.



File Transfer with /dev/tcp
=====
 
1) Start Labtainer "nmap-ssh".

2) You will establish a listening port on the analyst container and receive the contents of the file you created on the router container and sent over the socket. Use the command: "nc -l -p 1111 > filename.txt"

4) Now go to the router container and create a file that you will send to the analyst. Make sure to write a message you will recognize into the file.

5) Use the command: "cat filename.txt > /dev/tcp/172.25.0.2/1111" to send the file from the router to the analyst

5) You can go back to the analyst container now and check the contents of the file that you connected to the listener, it should display the file contents you sent from the router.



Custom Port Scanner with /dev/tcp
====


1) Start Labtainer "nmap-ssh". Log into the analyst container.

2) Create the following bash script on the machine to scan ports 20 thru 80 (you can do more/different ports but for our environment, this saves time) and saves the results to a file in your home directory:



```

#!/bin/bash

let p=20

while [ $p -lt 80 ];

do echo > /dev/tcp/172.25.0.3/$p;

  if [ $? == 0 ]; then
  
    echo $p "is open" >> /home/analyst/scan.txt; 
    
  fi;
  
let p=(p+1)

done

```



3) Simply run the script and check the file called "scan.txt" created in your home directory. This shows the ports that were open when scanned.


