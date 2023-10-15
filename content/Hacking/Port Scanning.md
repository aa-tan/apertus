---
title: "Port Scanning"
tags:
- enumeration
- nmap
- hacking
---
[[Hacking/Port|Port]] Scanning, also known as Port Knocking, is the process of checking the status of a machine's ports to determine the running services of that machine.

# Nmap
One of the most common tools for port scanning is `nmap`.
## Basic Scanning techniques

### Stealth scan / SYN Scan
Skips sending final `ACK` packet. By not completing the [[TCP Handshake]], detection by certain firewalls can be minimized as a successful connection was not made. This also has the side benefit of reducing traffic and speeding up the overall scan.

`sudo nmap -sS <IP ADDRESS>`

### TCP Connect scan
Default scan when run without sudo privileges. Executes a system call to `connect` with each port, mimicking the behavior most applications use in order to first establish a connection.

`nmap -sT <IP ADDRESS>`

### UDP Scan
Targets ports that respond to the [[UDP protocol]]. Due to the nature of UDP, this scan type is much slower to execute.
`sudo nmap -sU <IP ADDRESS>`

## Ping Sweeping
In order to conserve network traffic, it can be important to first identify the active machines within a given IP range. To accomplish this, a ping sweep can be run that sends ICMP ping requests to check for a host's response and skips any port scanning.
`nmap -sn <IP ADDRESS>`
`nmap -sn 10.11.1.1-254`


Simple grep notation bash one liner to check for hosts that are up
`nmap -sn 10.11.1.1-10 -oG - | cut -d " " -f 2 | head --lines=-1 | tail -n +2`
or to save to a file
```shell
$ nmap -sn <IP ADDRESS> -oG ping-sweep.txt
$ grep Up ping-sweep.txt | cut -d " " -f 2 | head
 ```