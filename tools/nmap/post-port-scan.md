---
description: >-
  we focus on the steps that follow port-scanning: in particular, service
  detection, OS detection, Nmap scripting engine, and saving the scan results.
---

# 😈 Post Port Scan

## Service Detection

Adding _<mark style="color:blue;">`-sV`</mark>_ to your Nmap command will collect and determine service and version information for the open ports. You can control the intensity with _<mark style="color:blue;">`--version-intensity LEVEL`</mark>_ where the level ranges between 0, the lightest, and 9, the most complete. _<mark style="color:blue;">`-sV`</mark>_ _<mark style="color:blue;">`--version-light`</mark>_ has an intensity of 2, while _<mark style="color:blue;">`-sV`</mark>_ _<mark style="color:blue;">`--version-all`</mark>_ has an intensity of 9.

It is important to note that using _<mark style="color:blue;">`-sV`</mark>_ will force Nmap to proceed with the _**TCP 3-way handshake**_ and establish the connection. The connection establishment is necessary because Nmap cannot discover the version without establishing a connection fully and communicating with the listening service. In other words, a _**stealth SYN scan**_ _<mark style="color:blue;">`-sS`</mark>_ is not possible when _<mark style="color:blue;">`-sV`</mark>_ option is chosen.

_<mark style="color:blue;">`-sV`</mark>_ required connecting to this open port to grab the service banner and any version information it can get, such as _**nginx 1.6.2**_. Hence, unlike the service column, the version column is not a guess.

## OS Detection

Nmap can detect the Operating System (OS) based on its behaviour and any telltale signs in its responses. OS detection can be enabled using _<mark style="color:blue;">`-O`</mark>_; this is an uppercase O as in OS. For example, we can run _<mark style="color:blue;">`nmap -sS -O MACHINE_IP`</mark>_ . Nmap detected the OS to be _`Linux 3.X`_, and then it guessed further that it was running _`kernel 3.13`_.

The OS detection is very convenient, but many factors might affect its accuracy. Therefore, always take the OS version with a grain of salt.

## Nmap Scripting Engine

A script is a piece of code that does not need to be compiled. In other words, it remains in its original human-readable form and does not need to be converted to machine language.&#x20;

A part of Nmap, Nmap Scripting Engine (NSE) is a Lua interpreter that allows Nmap to execute Nmap scripts written in Lua language.

You can specify to use any or a group of these installed scripts; moreover, you can install other user’s scripts and use them for your scans. Let’s begin with the default scripts. You can choose to run the scripts in the default category using _<mark style="color:blue;">`--script=default`</mark>_ or simply adding _<mark style="color:blue;">`-sC`</mark>_.

&#x20;In addition to default, categories include&#x20;

* <mark style="color:red;">auth:</mark> Authentication related scripts
* <mark style="color:red;">broadcast:</mark> Discover hosts by sending broadcast messages
* <mark style="color:red;">brute:</mark> Performs brute-force password auditing against logins
* <mark style="color:red;">discovery:</mark> Retrieve accessible information, such as database tables and DNS names
* &#x20;<mark style="color:red;">dos:</mark> Detects servers vulnerable to Denial of Service (DoS)
* <mark style="color:red;">exploit:</mark> Attempts to exploit various vulnerable services
* <mark style="color:red;">external:</mark> Checks using a third-party service, such as Geoplugin and Virustotal
* <mark style="color:red;">fuzzer:</mark> Launch fuzzing attacks
* <mark style="color:red;">intrusive:</mark> Intrusive scripts such as brute-force attacks and exploitation
* <mark style="color:red;">malware:</mark> Scans for backdoors
* <mark style="color:red;">safe:</mark> Safe scripts that won’t crash the target
* <mark style="color:red;">version:</mark> Retrieve service versions
* <mark style="color:red;">vuln:</mark> Checks for vulnerabilities or exploit vulnerable services

You can also specify the script by name using _<mark style="color:blue;">`--script "SCRIPT-NAME"`</mark>_ or a pattern such as

&#x20;_<mark style="color:blue;">`--script "ftp*"`</mark>_, which would include _<mark style="color:blue;">`ftp-brute`</mark>_. If you are unsure what a script does, you can open the script file with a text reader, such as less, or a text editor. In the case of _<mark style="color:blue;">`ftp-brute`</mark>_, it states: “Performs brute force password auditing against FTP servers.” You have to be careful as some scripts are pretty intrusive. Moreover, some scripts might be for a specific server and, if chosen at random, will waste your time with no benefit.

## Saving the Output

### Normal

As the name implies, the normal format is similar to the output you get on the screen when scanning a target. You can save your scan in normal format by using

&#x20;_<mark style="color:blue;">`-oN FILENAME`</mark>_; N stands for normal.

### Grepable

The grepable format has its name from the command grep; grep stands for Global Regular Expression Printer. In simple terms, it makes filtering the scan output for specific keywords or terms efficient. You can save the scan result in grepable format using _<mark style="color:blue;">`-oG FILENAME`</mark>_.

### XML

The third format is XML. You can save the scan results in XML format using _<mark style="color:blue;">`-oX FILENAME`</mark>_. The XML format would be most convenient to process the output in other programs. Conveniently enough, you can save the scan output in all three formats using _<mark style="color:blue;">`-oA FILENAME`</mark>_ to combine _<mark style="color:blue;">`-oN`</mark>_, _<mark style="color:blue;">`-oG`</mark>_, and _<mark style="color:blue;">`-oX`</mark>_ for normal, grepable, and XML.

## Commands

| options                 | meaning                                         |
| ----------------------- | ----------------------------------------------- |
| -sV                     | determine service/version info on open ports    |
| -sV --version-light     | try the most likely probes (2)                  |
| -sV --version-all       | try all available probes (9)                    |
| -O                      | detect OS                                       |
| --traceroute            | run traceroute to target                        |
| --script=SCRIPTS        | Nmap scripts to run                             |
| -sC or --script=default | run default scripts                             |
| -A                      | equivalent to -sV -O -sC --traceroute           |
| -oN                     | save output in normal format                    |
| -oG                     | save output in grepable format                  |
| -oX                     | save output in XML format                       |
| -oA                     | save output in normal, XML and Grepable formats |
