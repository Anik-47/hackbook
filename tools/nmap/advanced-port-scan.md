---
description: TCP Advanced Scans for firewall and IDS evasion
---

# 👹 Advanced Port Scan

## ADVANCE SCAN

| Scan Type                       | Command                                              |
| ------------------------------- | ---------------------------------------------------- |
| TCP Null Scan                   | sudo nmap -sN MACHINE\_IP                            |
| TCP FIN Scan                    | sudo nmap -sF MACHINE\_IP                            |
| TCP Xmas Scan                   | sudo nmap -sX MACHINE\_IP                            |
| TCP ACK Scan                    | sudo nmap -sA MACHINE\_IP                            |
| TCP Window Scan                 | sudo nmap -sW MACHINE\_IP                            |
| Custom TCP Scan                 | sudo nmap --scanflags URGACKPSHRSTSYNFIN MACHINE\_IP |
| Spoofed Source IP               | sudo nmap -S SPOOFED\_IP MACHINE\_IP                 |
| Spoofed MAC Address             | --spoof-mac SPOOFED\_MAC                             |
| Decoy Scan                      | nmap -D DECOY\_IP,ME MACHINE\_IP                     |
| IDLE (Zombie) Scan              | sudo nmap -sI ZOMBIE\_IP MACHINE\_IP                 |
| Fragment IP Data into 8 bytess  | -f                                                   |
| Fragment IP Data into 16 bytess | -ff                                                  |

| Option                  | Purpose                                  |
| ----------------------- | ---------------------------------------- |
| --source-port PORT\_NUM | specify source port number               |
| --data-length NUM       | append random data to reach given length |

### NULL SCAN

The null scan does not set any flag; all six flag bits are set to zero. You can choose this scan using the -sN option. A TCP packet with no flags set will not trigger any response when it reaches an open port, as shown in the figure below.&#x20;

Therefore, from Nmap’s perspective, a lack of reply in a null scan indicates that either the port is open or a firewall is blocking the packet.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/04b178a9cf7048c21256988b8b2343e3.png)

However, we expect the target server to respond with an RST packet if the port is closed. Consequently, we can use the lack of RST response to figure out the ports that are not closed: open or filtered.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/224e01a913a1ce7b0fb2b9290ff5e1c8.png)

### FIN SCAN

The FIN scan sends a TCP packet with the FIN flag set.&#x20;

You can choose this scan type using the <mark style="color:blue;">`-sF`</mark> option. Similarly, no response will be sent if the TCP port is open. Again, Nmap cannot be sure if the port is open or if a firewall is blocking the traffic related to this TCP port.

However, the target system should respond with an RST if the port is closed. Consequently, we will be able to know which ports are closed and use this knowledge to infer the ports that are open or filtered.

{% hint style="danger" %}
It's worth noting some firewalls will 'silently' drop the traffic without sending an RST.
{% endhint %}

### XMAS SCAN

The Xmas scan gets its name after Christmas tree lights. Xmas scan sets the _FIN, PSH, and URG_ flags simultaneously. You can select Xmas scan with the option -sX.

Like the Null scan and FIN scan, if an RST packet is received, it means that the port is closed. Otherwise, it will be reported as open|filtered.

{% hint style="warning" %}
On scenario where these three scan types can be efficient is when scanning a target behind a stateless (non-stateful) firewall. A stateless firewall will check if the incoming packet has the SYN flag set to detect a connection attempt. Using a flag combination that does not match the SYN packet makes it possible to deceive the firewall and reach the system behind it. However, a stateful firewall will practically block all such crafted packets and render this kind of scan useless.
{% endhint %}

### TCP ACK SCAN

As the name implies, an ACK scan will send a TCP packet with the ACK flag set. Use the <mark style="color:blue;">`-sA`</mark> option to choose this scan. The target would respond to the ACK with RST regardless of the state of the port. This behavior happens because a TCP packet with the ACK flag set should be sent only in response to a received TCP packet to acknowledge the receipt of some data, unlike in our case. Hence, this scan won’t tell us whether the target port is open in a simple setup.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/a991831cedbb2761dde1fe66012a7311.png)

{% hint style="info" %}
This kind of scan would be helpful if there is a firewall in front of the target. Consequently, based on which ACK packets resulted in responses, you will learn which ports were not blocked by the firewall. In other words, this type of scan is more suitable to discover firewall rule sets and configurations.
{% endhint %}

### WINDOW SCAN

The TCP window scan is almost the same as the ACK scan; however, it examines the TCP Window field of the RST packets returned. On specific systems, this can reveal that the port is open. You can select this scan type with the option <mark style="color:blue;">`-sW`</mark>. We expect to get an RST packet in reply to our “uninvited” ACK packets, regardless of whether the port is open or closed.

Launching a TCP window scan against a Linux system with no firewall will not provide much information.

However, as you would expect, if we repeat our TCP window scan against a server behind a firewall, we expect to get more satisfying results. Ports might show closed, but we know that these ports are not closed, we realize they responded differently, indicating that the firewall does not block them.

\
 <a href="#advanced-scan-type" id="advanced-scan-type"></a>
-----------------------------------------------------------
