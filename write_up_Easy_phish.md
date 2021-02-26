## Easy Phish Write up

#### Challenge description
```
Customers of secure-startup.com have been recieving some very convincing phishing emails, 
can you figure out why?
``` 

I decided to use a simple google search for the `secure-starup.com` domain. I found something about SPF-records and decided to look further into that. 

It seems that SPF is a sender policy framework record, and is used to indicate to mail exchanges which hosts are authorized to send mail for a domain. 

I decided to use `nslookup -type=any secure-startup.com` to look up the dns records:
```bash
╰─$ nslookup -type=any secure-startup.com 
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	secure-startup.com
Address: 34.102.136.180
secure-startup.com	nameserver = ns69.domaincontrol.com.
secure-startup.com	nameserver = ns70.domaincontrol.com.
secure-startup.com
	origin = ns69.domaincontrol.com
	mail addr = dns.jomax.net
	serial = 2020070800
	refresh = 28800
	retry = 7200
	expire = 604800
	minimum = 600
secure-startup.com	text = "v=spf1 a mx ?all - HTB{RIP_SPF_Always_2nd"
Authoritative answers can be found from:

```
Where i found some part of the flag: `HTB{RIP_SPF_Always_2nd`

However this didnt give me the full flag, and therefor i decided to use dnsrecon, so scan further for the flag.

`
python3 dnsrecon.py -d secure-startup.com
`

```
╰─$ python3 dnsrecon.py -d secure-startup.com                                                                     2 ↵
[*] Performing General Enumeration of Domain: secure-startup.com
[-] All nameservers failed to answer the DNSSEC query for secure-startup.com
[*] 	 SOA ns69.domaincontrol.com 97.74.104.45
[*] 	 NS ns69.domaincontrol.com 97.74.104.45
[*] 	 NS ns69.domaincontrol.com 2603:5:2184::2d
[*] 	 NS ns70.domaincontrol.com 173.201.72.45
[*] 	 NS ns70.domaincontrol.com 2603:5:2284::2d
[-] Could not Resolve MX Records for secure-startup.com
[*] 	 A secure-startup.com 34.102.136.180
[*] 	 TXT secure-startup.com v=spf1 a mx ?all - HTB{RIP_SPF_Always_2nd
[*] 	 TXT secure-startup.com v=DMARC1;p=none;_F1ddl3_2_DMARC}
[*] Enumerating SRV Records
[+] 0 Records Found
```
and it seems that the TXT records SPF and DMARC stores the flag:

`_F1ddl3_2_DMARC}`

```
DMARC is an email authentication protocol. 
It is designed to give email domain owners the ability to protect
their domain from unauthorized use like email spoofing etc. 
DMARC is to protect a domain from being used in business email compromise attacks, 
phishing emails, email scams and other cyber threat 
```


So the full flag is : `HTB{RIP_SPF_Always_2nd_F1ddl3_2_DMARC}`

