
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