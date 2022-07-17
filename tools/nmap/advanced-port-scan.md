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

## Spoofing And Decoys <a href="#advanced-scan-type" id="advanced-scan-type"></a>

### Spoofing <a href="#advanced-scan-type" id="advanced-scan-type"></a>

In some network setups, you will be able to scan a target system using a spoofed IP address and even a spoofed MAC address. Such a scan is only beneficial in a situation where you can guarantee to capture the response. If you try to scan a target from some random network using a spoofed IP address, chances are you won’t have any response routed to you, and the scan results could be unreliable.

The following figure shows the attacker launching the command _<mark style="color:blue;">`nmap -S SPOOFED_IP MACHINE_IP`</mark>_. Consequently, Nmap will craft all the packets using the provided source IP address _<mark style="color:blue;">`SPOOFED_IP`</mark>_. The target machine will respond to the incoming packets sending the replies to the destination IP address _<mark style="color:blue;">`SPOOFED_IP`</mark>_. For this scan to work and give accurate results, the attacker needs to monitor the network traffic to analyze the replies.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/45b982d501fd26deb2b381059b16f80c.png)

In brief, scanning with a spoofed IP address is three steps:

1. The attacker sends a packet with a spoofed source IP address to the target machine.
2. The target machine replies to the spoofed IP address as the destination.
3. The attacker captures the replies to figure out open ports.

In general, you expect to specify the network interface using -e and to explicitly disable ping scan -Pn. Therefore, instead of using_<mark style="color:blue;">`nmap -S SPOOFED_IP MACHINE_IP`</mark>_ you will need to issue _<mark style="color:blue;">`nmap -e NET_INTERFACE -Pn -S SPOOFED_IP MACHINE_IP`</mark>_ to tell Nmap explicitly which network interface to use and not to expect to receive a ping reply. It is worth repeating that this scan will be useless if the attacker system cannot monitor the network for responses.

When you are on the same subnet as the target machine, you would be able to spoof your MAC address as well. You can specify the source MAC address using _<mark style="color:blue;">`--spoof-mac SPOOFED_MAC`</mark>_. This address spoofing is only possible if the attacker and the target machine are on the same Ethernet (802.3) network or same WiFi (802.11).

### Decoy

Spoofing only works in a minimal number of cases where certain conditions are met. Therefore, the attacker might resort to using decoys to make it more challenging to be pinpointed.&#x20;

The concept is simple, make the scan appears to be coming from many IP addresses so that the attacker’s IP address would be lost among them.&#x20;

As we see in the figure below, the scan of the target machine will appear to be coming from 3 different sources, and consequently, the replies will go to the decoys as well.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/754fc455556a424ca83f512665beaf7d.png)

You can launch a decoy scan by specifying a specific or random IP address after _<mark style="color:blue;">`-D`</mark>_. For example, _<mark style="color:blue;">`nmap -D 10.10.0.1,10.10.0.2,ME MACHINE_IP`</mark>_ will make the scan of _<mark style="color:blue;">`MACHINE_IP`</mark>_ appearing as coming from the IP addresses 10.10.0.1, 10.10.0.2, and then _`ME`_ , indicating that your IP address should appear in the third order. Another example command would be:

_<mark style="color:blue;">`nmap -D 10.10.0.1,10.10.0.2,RND,RND,ME MACHINE_IP`</mark>_

where the third and fourth source IP addresses are assigned randomly, while the fifth source is going to be the attacker’s IP address. In other words, each time you execute the latter command, you would expect two new random IP addresses to be the third and fourth decoy sources.

## Fragmented Packets

### Firewall

A firewall is a piece of software or hardware that permits packets to pass through or blocks them. It functions based on firewall rules, summarized as blocking all traffic with exceptions or allowing all traffic with exceptions. For instance, you might block all traffic to your server except those coming to your web server. A traditional firewall inspects, at least, the IP header and the transport layer header. A more sophisticated firewall would also try to examine the data carried by the transport layer.

### IDS

A firewall is a piece of software or hardware that permits packets to pass through or blocks them. It functions based on firewall rules, summarized as blocking all traffic with exceptions or allowing all traffic with exceptions. For instance, you might block all traffic to your server except those coming to your web server. A traditional firewall inspects, at least, the IP header and the transport layer header. A more sophisticated firewall would also try to examine the data carried by the transport layer.

### Fragmentation of Packets

Nmap provides the option _<mark style="color:blue;">`-f`</mark>_ to fragment packets. Once chosen, the IP data will be divided into 8 bytes or less. Adding another _<mark style="color:blue;">`-f`</mark>_ (_<mark style="color:blue;">`-f -f`</mark>_ or _<mark style="color:blue;">`-ff`</mark>_) will split the data into 16 byte-fragments instead of 8. You can change the default value by using the _<mark style="color:blue;">`--mtu`</mark>_; however, you should always choose a multiple of 8.

if you prefer to increase the size of your packets to make them look innocuous, you can use the option

_<mark style="color:blue;">`--data-length NUM`</mark>_, where num specifies the number of bytes you want to append to your packets.

## Idle or Zombie Scan



spoofing the source IP address can be a great approach to scanning stealthily. However, spoofing will only work in specific network setups. It requires you to be in a position where you can monitor the traffic. Considering these limitations, spoofing your IP address can have little use; however, we can give it an upgrade with the idle scan.

The idle scan, or zombie scan, requires an idle system connected to the network that you can communicate with. Practically, Nmap will make each probe appear as if coming from the idle (zombie) host, then it will check for indicators whether the idle (zombie) host received any response to the spoofed probe. This is accomplished by checking the IP identification (IP ID) value in the IP header. You can run an idle scan using _<mark style="color:blue;">`nmap -sI ZOMBIE_IP MACHINE_IP`</mark>_, where _<mark style="color:blue;">`ZOMBIE_IP`</mark>_ is the IP address of the idle host (zombie).

The idle (zombie) scan requires the following three steps to discover whether a port is open:

1. Trigger the idle host to respond so that you can record the current IP ID on the idle host.
2. Send a SYN packet to a TCP port on the target. The packet should be spoofed to appear as if it was coming from the idle host (zombie) IP address.
3. Trigger the idle machine again to respond so that you can compare the new IP ID with the one received earlier.
