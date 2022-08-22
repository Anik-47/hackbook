# 👾 Host Discovery

| Scan type              | Command                                    |
| ---------------------- | ------------------------------------------ |
| ARP Scan               | sudo nmap -PR -sn MACHINE\_IP/24           |
| ICMP Echo Scan         | sudo nmap -PE -sn MACHINE\_IP/24           |
| ICMP Timestamp Scan    | sudo nmap -PP -sn MACHINE\_IP/24           |
| ICMP Address Mask Scan | sudo nmap -PM -sn MACHINE\_IP/24           |
| TCP SYN Ping Scan      | sudo nmap -PS22,80,443 -sn MACHINE\_IP/30  |
| TCP ACK Ping Scan      | sudo nmap -PA22,80,443 -sn MACHINE\_IP/30  |
| UDP Ping Scan          | sudo nmap -PU53,161,162 -sn MACHINE\_IP/30 |

| Options | Purpose                                         |
| ------- | ----------------------------------------------- |
| -n      | no DNS lookup                                   |
| -R      | reverse-DNS lookup for all hosts (even offline) |
| -sn     | Host Discovery only                             |

{% hint style="info" %}
Remember to add <mark style="color:blue;background-color:blue;">`-sn`</mark> if you are only interested in host discovery without port-scanning. Omitting <mark style="color:blue;">-sn</mark> will let Nmap default to port-scanning the live hosts.
{% endhint %}

### ARP SCAN

ARP scan is possible only if you are on the same subnet as the target systems. On an Ethernet (802.3) and WiFi (802.11), you need to know the MAC address of any system before you can communicate with it.&#x20;

The MAC address is necessary for the link-layer header; the header contains the source MAC address and the destination MAC address among other fields. To get the MAC address, the OS sends an ARP query.&#x20;

A host that replies to ARP queries is up. The ARP query only works if the target is on the same subnet as yourself, i.e., on the same Ethernet/WiFi. You should expect to see many ARP queries generated during a Nmap scan of a local network.

## --ICMP SCAN--

### ICMP Echo Scan

The _**Internet Control Message Protocol**_ (_ICMP_) is a network layer _protocol_ used by network devices to diagnose network communication issues. We can ping every IP address on a target network and see who would respond to our ping (ICMP Type 8/Echo) requests with a ping reply (ICMP Type 0).&#x20;

Although this would be the most straightforward approach, it is not always reliable. Many firewalls block ICMP echo; new versions of MS Windows are configured with a host firewall that blocks ICMP echo requests by default.&#x20;

{% hint style="info" %}
Remember that an ARP query will precede the ICMP request if your target is on the same subnet
{% endhint %}

### ICMP Time Stamp

Because ICMP echo requests tend to be blocked, you might also consider ICMP Timestamp or ICMP Address Mask requests to tell if a system is online.&#x20;

Nmap uses a timestamp request (ICMP Type 13) and checks whether it will get a Timestamp reply (ICMP Type 14). Adding the _<mark style="color:blue;">-PP</mark>_ option tells Nmap to use ICMP timestamp requests.

### ICMP Address Mask

Nmap uses address mask queries (ICMP Type 17) and checks whether it gets an address mask reply (ICMP Type 18). This scan can be enabled with the option -PM.

It is essential to note that this scan sent ICMP address mask requests to every valid IP address and waited for a reply. Each ICMP request was sent twice

{% hint style="warning" %}
The Firewall on the route might block this type of ICMP packet. Therefore, it is essential to learn multiple approaches to achieve the same result. If one type of packet is being blocked, we can always choose another to discover the target network and services.
{% endhint %}

##
