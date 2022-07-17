# Host Discovery

| Scan type              | Command                                    |
| ---------------------- | ------------------------------------------ |
| ARP Scan               | sudo nmap -PR -sn MACHINE\_IP/24           |
| ICMP Echo Scan         | sudo nmap -PE -sn MACHINE\_IP/24           |
| ICMP Timestamp Scan    | sudo nmap -PP -sn MACHINE\_IP/24           |
| ICMP Address Mask Scan | sudo nmap -PM -sn MACHINE\_IP/24           |
| TCP SYN Ping Scan      | sudo nmap -PS22,80,443 -sn MACHINE\_IP/30  |
| TCP ACK Ping Scan      | sudo nmap -PA22,80,443 -sn MACHINE\_IP/30  |
| UDP Ping Scan          | sudo nmap -PU53,161,162 -sn MACHINE\_IP/30 |

| Options | Purpose |
| ------- | ------- |
|         |         |
|         |         |
|         |         |
