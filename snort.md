![](media/b80e0eacca6dad9d42b5dc3545946591.png)

Using Snort
===========

Overview
========

This exercise introduces the use of the snort system to provide intrusion
detection. Students will configure simple snort rules and experiment with a
network intrusion detection system, (IDS).

Lab Environment
===============

Once you have logged into your range account and accessed your Labtainer-VM,
open a terminal window.

Navigate to the “labtainer-student” directory and start the lab using the
command:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$  labtainer snort
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-   Links to this lab manual will be displayed if you wish to view the prompt
    from within your VM

Network Configuration
=====================

This lab includes several networked computers as shown in Figure 1. When the lab
starts, you will get (3) virtual terminals, each housing (2)
containers. 

**Terminal 1)

-   The external side of the gateway is with our external address, i.e.,
    203.0.113.10. and routes web traffic (ports 80 and 443) to the internal
    gatway container.

-   The remote workstation component includes the Firefox browser, and is
    configured with local resolution to reach the www.example.com website via
    the external IP: 203.0.113.10. The internal workstation (ws2) also includes
    Firefox and an entry in /etc/hosts for www.example.com. Both workstations
    also include the nmap utility. "Hank" is our external adversary.

**Terminal 2)

-   The internal side of the gateway is configured with NAT to translate sources
    addresses of traffic from internal IP addresses, e.g., 192.168.2.1, to our
    external address, i.e., 203.0.113.10. The iptables in the gateway also
    routes web traffic (ports 80 and 443) to the web server component by
    translating the externally visible destination address to the internal web
    server address. The gateway is configured to mirror traffic that enters the
    gateway via either the 203.0.113.10 link, or the link to the web server.
    This mirrored traffic is routed to the snort container. This mirroring
    allows the snort IDS to reconstruct TCP sessions between the web server and
    external addresses and monitor traffic.

-   The web server runs Apache and is configured to support SSL for web pages in
    the www.example.com domain.

**Terminal 3)

-   The snort container includes the Snort IDS utility and is assigned to "Tom"
    in Security. It also includes Wireshark to help you observe traffic being
    mirrored to the snort container.

-   The ws 2 is an internal workstaion used by a company employee (Mary).

-   Take an opportunity to familiarize yourself with these lab prompts 

![](media/de3a114c656a9d761046bd0ac8a0503e.jpg)

Lab Tasks
=========

Starting and stopping snort
===========================

The Snort utility is installed on the snort container. The home directory
includes a start snort.sh script that will start the utility in Network
Intrusion Detection Mode, and display alerts to the console. For this lab, you
are required to start snort with:

```
./start_snort.sh
```

When it comes time to stop snort, e.g., to add rules, simply use CTL-C.

Pre-configured Snort rules
==========================

While snort is running, open Firefox on the hank@remote_ws and view the webpage:

```
firefox www.example.com
```

-   You should not see any alerts, as this is normal, permissible traffic.

-   Close the firefox browser.

The Snort utility includes a set of pre-configured rules that create alerts for
known suspicious network activity. The configuration on the snort component is
largely as it exists after initial installation of the snort utility. To see an
example of some of the pre-configured rules, perform an nmap scan of
www.example.com from the remote ws container:

```
sudo nmap www.example.com
```

\*Note the alerts displayed at the snort console. The rules that generate these
alerts can be seen, along with all rules, in /etc/snort/rules/

Write a simple (bad) rule
=========================

Custom rules are typically added to the file at /etc/snort/rules/local.rules.
Take note of your current location in the file system (Use "pwd". You should be
in Tom's home directory.)sn

-   Open

-   Stop snort and add a rule that generates an alert for each packet within a
    TCP stream.

-   You will need to access the "nano" text editor, with root privileges and
    add a rule.

```
sudo nano /etc/snort/rules/local.rules
```

-   Now add to the bottom of the file:

```
alert tcp any any -> any any (msg:"TCP detected"; sid:00002;)
```

**The rule can be read as: “Generate an alert whenever a TCP packet from any
address on any port is sent to any address on any port, and include the message
tagged as “TCP detected”:, and give the rule an identifier of 00002.”

-   Restart snort using the ./start_snort.sh.

-   On the hank@remote ws container, test this rule by re-starting Firefox on the remote ws:

```
firefox www.example.com
```
    As you can see, the rule you wrote will overwhelm you with useless
    information because it is alerting to ALL the TCP traffic being produced by
    the remote ws's firefox connection to the website.

-   Stop snort and delete the rule.

Custom rule for CONFIDENTIAL traffic
====================================

**AS THE ATTACKER- HANK** - On the remote ws, open a firefox browser and ensure
you are connected to http://www.example.com.

-   Hank has learned about an unpublished webpage that exists on the website. In
    particular, he has learned that there is a confidential business plan at
    http://www.example.com/plan.html. MAKE SURE YOU ARE USING HTTP, not HTTPS.
    View the plan.

**AS THE DEFENDER- TOM** - Now let’s switch seats back into the defenders' role.
Working as Tom, on the snort container, add a rule to your local.rules file on
snort that will generate an alert whenever the text ”CONFIDENTIAL” is sent out
to the internet.

-   Reference the snort manual https://www.snort.org/ section 3.5 (starting at
    pg 190) to understand how to qualify alerts based on the content of packets.
    Be sure to include the word ”CONFIDENTIAL” in the alert message, and give
    the rule its own unique sid.

-   After adding the rule, restart snort.

**TEST YOUR RULE AS THE ATTACKER- HANK**

-   On the firefox browser at the remote ws, clear your history (Menu /
    Preferences / Security & Privacy).

-   Re-open the plan.html page. You should see an alert at the snort console
    indicating that your rule was successful.

Effects of encryption
=====================

Back at the Firefox browser, again clear the browser history. Now alter the URL
to make use of the web server SSL function. Change the url to
https://www.example.com/plan.html. Do you see a new snort alert? Why?

One solution to this problem is to use a reverse proxy in front of the web
server. This reverse proxy would handle the incoming web traffic and manage the
SSL connections. The web server would then receive only clear-text HTTP traffic,
and outgoing traffic from the web server could then be mirrored to the IDS. We
will not pursue that solution in this lab for the sake of resources and time.



Submission
==========

After finishing the lab, go to the terminal on your Linux system that was used
to start the lab and type:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ stoplab
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
