
### Project Discovery tools


- subfinder
```bash
# scanning from  **Curated** passive sources to maximize results
subfinder -d "DOMAIN" -all > domains.txt

```
- subfinder + dnsx + httpx
	- dnsx is a DNS toolkit, provides resolution, bruteforcing and automatic wildcard support 
	- httpx is a HTTP toolkit with multithread support
```bash 
# subdomain enumertion + http probing
subfinder -d "DOMAIN" -all | dnsx | httpx 
#### dnsx
	# -silent
	# -a -resp = print A records for a given subdomain list
```
-  subdomain enumeration with subfinder and assetfinder
```bash
subfinder -dL domains.txt -all -recursive -o subs.txt
cat domains.txt | assetfinder --subs-only | tee -a subs2.txt

# combine and filter results + http probing with httpx
cat subs.txt subs2.txt | sort -u | tee -a all-subs.txt
cat all-subs.txt | httpx | tee -a live-subs.txt
# taking screenshots with the combined output
cat live_subs.txt | httpx -screenshotls
```

### DNS Info

**WHOIS**
Request<>Response protocol. A server listens on port 43 for incomming requests.
A domain registar is responsible for maintaining WHOIS records for the domain names it is leasing.

	**WHOIS response:**
	- Registar
	- Contact info of the registrant
	- Creation/update/expiration dates 
	- Name Server: Which server to ask to resolve the domain name


**nslookup => name server lookup**

**dig -> domain information cropper**


| Purpose                            | Commandline Example                     |
| ---------------------------------- | --------------------------------------- |
| Lookup WHOIS record                | `whois example.com`                     |
| Lookup DNS A records               | `nslookup -type=A example.com`          |
| Lookup DNS MX records at DNSserver | `nslookup -type=MX example.com 1.1.1.1` |
| Lookup DNS TXT records             | `nslookup -type=TXT example.com`        |
| Lookup DNS A records               | `dig example.com A`                     |
| Lookup DNS MX records at DNSserver | `dig @1.1.1.1 example.com MX`           |
| Lookup DNS TXT records             | `dig example.com TXT`                   |
**netcat**
supports both TCP/UDP protocols, it can function as a a client or as a server that listens on a chosen port
```bash
# listen on the default network interface on a given port
# ports less than 1024 require root privileges to listen on
nc -nvvlp <PORT> 
```