---
description: BASIC TCP & UDP Scan
---

# 🤖 Port Scan

{% hint style="info" %}
A port scan is a common technique hackers use to discover open doors or weak points in a network. A port scan attack helps cyber criminals find open ports and figure out whether they are receiving or sending data. It can also reveal whether active security devices like firewalls are being used by an organization.
{% endhint %}

## &#x20;TCP FLAGS



1. <mark style="color:red;">**URG**</mark>: Urgent flag indicates that the urgent pointer filed is significant. The urgent pointer indicates that the incoming data is urgent and that a TCP segment with the URG flag set is processed immediately without consideration of having to wait on previously sent TCP segments.
2. <mark style="color:red;">**ACK**</mark>: The Acknowledgement flag indicates that the acknowledgement number is significant. It is used to acknowledge the receipt of a TCP segment.
3. <mark style="color:red;">**PSH**</mark>: Push flag asking TCP to pass the data to the application promptly.
4. <mark style="color:red;">**RST**</mark>: Reset flag is used to reset the connection. Another device, such as a firewall, might send it to tear a TCP connection. This flag is also used when data is sent to a host and there is no service on the receiving end to answer.
5. <mark style="color:red;">**SYN**</mark>: Synchronize flag is used to initiate a TCP 3-way handshake and synchronize sequence numbers with the other host. The sequence number should be set randomly during TCP connection establishment.
6. <mark style="color:red;">**FIN**</mark>: The sender has no more data to send.

## TCP HEADER

This figure looks sophisticated at first; however, it is pretty simple to understand. In the first row, we have the source TCP port number and the destination port number.&#x20;

We can see that the port number is allocated 16 bits (2 bytes). In the second and third rows, we have the sequence number and the acknowledgment number.&#x20;

Each row has 32 bits (4 bytes) allocated, with six rows total, making up 24 bytes.

![TCP HEADER](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/79ca8e4acbd573a27cee413cde927769.png)

## UDP SCAN

If we send a UDP packet to an open UDP port, we cannot expect any reply in return. Therefore, sending a UDP packet to an open port won’t tell us anything.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/085088cd1b2b122312b1ee952c4aa0f7.png)

However, as shown in the figure below, we expect to get an ICMP packet of type 3, destination unreachable, and code 3, port unreachable. In other words, the UDP ports that don’t generate any response are the ones that Nmap will state as open.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/8b8b32517699b96777641a97dbf9d880.png)

## Types of Scan

|                       |                           |
| --------------------- | ------------------------- |
| TCP Connect Scan      | nmap -sT MACHINE\_IP      |
| TCP SYN Scan(stealth) | sudo nmap -sS MACHINE\_IP |
| UDP Scan              | sudo nmap -sU MACHINE\_IP |

| Arguments             | Description                              |
| --------------------- | ---------------------------------------- |
| -p-                   | all ports                                |
| -p1-1023              | scan ports 1 to 1023                     |
| -F                    | 100 most common ports                    |
| -r                    | scan ports in consecutive order          |
| T<0-5>                | -T0 being the slowest and T5 the fastest |
| --max-rate 50         | rate <= 50 packets/sec                   |
| --min-rate 15         | rate >= 15 packets/sec                   |
| --min-parallelism 100 | --min-parallelism 100                    |
