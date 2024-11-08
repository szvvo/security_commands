We want to scan a target network. Is there an efficient tool to help us with that?

**When we are doing active reconnaissance we want to know the following:**
- Network Topology
- Which systems are up?
- What services are running on those active systems? :LiEye:
	- Fingerprinting


- ARP Scan
	- ARP requests
	- Aims to get the MAC address of a host so that communication over the lini-layer becomes possible
	- Only allows to discover hosts on a same subnet AKA same broadcast domain
- ICMP Scan
	- ICMP request (Type8/Echo) <> ICMP TYPE0
- TCP/UDP ping scan
	- TCP/UDP packets are sent to port to determine live host

ARP precedes Network-Layer scanning when on the same broadcast domain

![[Pasted image 20241105205337.png]]

### NMAP 

Nmap, by default, uses a ping scan to find live hosts, then proceeds to scan live hosts only 

If one type of packet is blocked, we can always try more packet types

nmap scan phases
- Target Enumeration
- Discover live hosts
- Reverse DNS lookups
- Scan ports
- Detect versions
- Detect OS
- Traceroute
- Scripts

### Host Discovery
```bash
# nmap commands

# scan a range of ip addresses
nmap -sL -n 10.10.0-255.101-125

# discover online hosts without port-scanning
nmap -sn TARGETS

### ARP scan without port scanning 
nmap -PR -sn TARGETS 

### ICMP scan without port scanning
nmap -PE -sn TARGETS/CIDR

### ICMP scan using Type 13 (timestamp request)
nmap -PP -sn TARGET

### ICMP scan using Type 17/18 (address mask queries
nmap -PM -sn TARGET/CIDR

### TCP SYN ping (SYN Flag set, expecting a SYN/ACK packet)
# Closed port will result in an RST (Reset)
# root users can bypass the TCP 3-Way handshake
nmap -PS<PORT> -sn TARGET
# SYN > <SYN/ACK > RST

### TCP ACK ping (ACK Flag Set)
nmap -PA<PORT> -sn TARGET
# ACK > < RST

### UDP Ping
# we do not expect to get any reply on open ports, however on closed port we will get an 'ICMP port unreachable' Type 3 packet
nmap -PU<PORT> -sn TARGET

### Reverse-DNS lookup
# nmap default behaviour is to use reverse-dns online hosts 
# -n to skip this
# -R to query for offline hosts (all hosts)
# --dns-servers DNS_SERVER

```


### Basic Port Scans

![[Pasted image 20241107210243.png]]

### Flags
- URG
- ACK: 
- PSH: Push data to the application 
- RST: Reset connection, tear a TCP connection
- SYN: Initiate a TCP 3-Way Handshake and synchorize sequence numbers
- FIN: Sender has no more data to send

At the most simplistic way, we can classify ports in two states:
- Open ports (No service is running)
- Closed ports (Port is accesible)
- Filtered: Cannot determine if the port is either open/closed (it usually means that a firewall is blocking either by blocking the port or blocking the host)
- Ulfiltered : Cannot determine if the port is open or closed, although the port is accesible - Encountered when using ACK `nmap-sA`
- Open|Filtered: Port Either open or filtered
- Closed|Filtered: Port Either closed or filtered 
Considering firewalls in place:

### TCP SYN Scan
![[Pasted image 20241107211652.png]]

```bash
### TCP connect port scan
# initiates a 3-way handshake and sends a RST/ACK PACKET
nmap -sT -F TARGET
-- A closed port will response to a SYN with a RST/ACK packet


### TCP SYN port scan 
# tears down connection once it receives a SYN/ACK packet
nmap -sS


### UDP port scan
# since udp is connectionless it will be listening for any ICMP type 3 destination unreachable packet and will consider open ports to those that does not receive a type 3
nmap -sU TARGET


### Setting up scan speed 
- paranoid (0)
- sneaky (1)
- polite (2)
- normal (3)
- aggressive (4)
- insane (5)
# `-T0` scans one port at a time and waits 5 minutes between sending each probe,
# -T3 by default
# --max-rate=10 - no more than 10 packets p/s
# control probing parallelization using `--min-parallelism <numprobes>`
nmap -sS -p- -T5 -v

```

