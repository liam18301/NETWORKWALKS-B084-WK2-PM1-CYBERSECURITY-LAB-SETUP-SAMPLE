# NETWORKWALKS-B084-WK2-PM1-CYBERSECURITY-LAB-SETUP-SAMPLE
</div> <p align="center"> <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" /> <img src="https://img.shields.io/badge/Kali%20Linux-404040?style=flat-square&labelColor=C00000&logo=kalilinux&logoColor=white" /> <img src="https://img.shields.io/badge/Zenmap-Nmap%20GUI-0070C0?style=flat-square&labelColor=000000" /> <img src="https://img.shields.io/badge/Phase%201-Reconnaissance-E87500?style=flat-square&labelColor=000000" /> <img src="https://img.shields.io/badge/Phase%202-Scanning-238F89?style=flat-square&labelColor=000000" /> <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" /> <img src="https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000" /> <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" /> <img src="https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000" /> <img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" /> </p>
📌 Project Overview

This project covers the first two steps of a penetration test:

Reconnaissance & Footprinting: collecting publicly available information about the networkwalks.com domain using six Kali Linux tools.
Scanning & Network Discovery: using Zenmap (the Nmap GUI) to find live hosts, IP addresses and MAC addresses on my own local network and to view the network topology.

Together they show how an attacker moves from gathering public information to mapping live hosts on a network. This is the Week 2 part of the Cybersecurity program at Networkwalks.

🎯 Objectives

The main objectives of this project are to:

Collect domain registration details with WHOIS.
Identify web technologies with WhatWeb.
Resolve the domain to its IP address with Nslookup.
Read HTTP response headers with Curl.
Detect a Web Application Firewall with Wafw00f.
Enumerate DNS records with DNSRecon.
Discover live hosts on my local network with Zenmap (ping scan).
View the network topology of the discovered hosts.
Analyse the risk of the information exposed.
Document the complete process.
🛡️ Authorization & Scope
🎯 Target	🔐 Permission
networkwalks.com	✅ Written permission secured
My own local LAN (192.168.180.0/24)	✅ Network that I own

⚠️ Important: Reconnaissance and scanning must only be performed on systems that you own or have explicit written permission to test. Do not use these tools against unauthorized systems.

🧰 Tools Used
🛠️ Tool	🎯 Purpose
🐉 Kali Linux & Ubuntu	Operating systems used for the activities
🌐 WHOIS	Find domain registration details (registrar, dates, name servers)
🧩 WhatWeb	Fingerprint web technologies (server, CMS, plugins, IP)
📡 Nslookup	Resolve the domain name to its IP address using DNS
📨 curl -I	Read the HTTP response headers of the website
🧱 Wafw00f	Detect whether a Web Application Firewall protects the site
🗂️ DNSRecon	Enumerate DNS records (SOA, NS, MX, A, TXT/SPF, SRV)
🗺️ Zenmap (Nmap GUI)	Scan the local subnet for live hosts, IPs and MAC addresses
💻 Linux terminal	Local IP address identification
🪜 Phase 1: Reconnaissance & Footprinting

Target: networkwalks.com

Step 1. WHOIS
bash
whois networkwalks.com

What I found:

Registrar: GoDaddy.com, LLC
Created: 2019-11-06, expires: 2027-11-06
Name servers: NS6135.HOSTGATOR.COM and NS6136.HOSTGATOR.COM
DNSSEC: unsigned
Domain status includes the clientDeleteProhibited, clientTransferProhibited, clientUpdateProhibited and clientRenewProhibited locks.

Why it matters: the registrar, dates and name servers tell an attacker where the domain is registered and which company hosts its DNS.

<img width="1279" height="613" alt="who is" src="https://github.com/user-attachments/assets/62d9d7a8-021e-4eea-a914-65c070f6dee5" />


Step 2. WhatWeb
bash
whatweb networkwalks.com

What I found:

Web server: Apache
CMS: WordPress 7.1.4 with the WordPress Download Manager 3.3.76 plugin
Other technologies: jQuery 3.7.1, Google Tag Manager, HTML5
Server IP: 192.232.216.135
Contact email exposed on the page: info@networkwalks.com
http:// redirects (301) to https://

Why it matters: exposed CMS, plugin and version information lets an attacker look for known weaknesses in that software.

<img width="1279" height="613" alt="what is" src="https://github.com/user-attachments/assets/8ff9168d-d956-4e0a-a029-00810cd88336" />


Step 3. Nslookup
bash
nslookup networkwalks.com

What I found: the domain resolves to 192.232.216.135 (non-authoritative answer from the 8.8.8.8 DNS server).

Why it matters: the IP address identifies where the web service is hosted and gives a target for later scanning.

<img width="1279" height="613" alt="nslookup" src="https://github.com/user-attachments/assets/7ce222c9-45b5-4019-9f3c-684a4df18fe8" />


Step 4. Curl (HTTP headers)
bash
curl -I https://networkwalks.com

What I found:

HTTP/2 200 response, server: Apache
x-nginx-cache: WordPress and x-endurance-cache-level headers
The WordPress REST API endpoint /wp-json/ is advertised in the link header
A __wpdm_client cookie is set with the secure and HttpOnly flags
referrer-policy and permissions-policy headers are present

Why it matters: headers reveal the technology stack and expose the /wp-json/ endpoint, which helps further enumeration.

<img width="1279" height="613" alt="curl -I" src="https://github.com/user-attachments/assets/c717e58d-b463-4de5-803c-d21bda1419e7" />


Step 5. Wafw00f
bash
wafw00f networkwalks.com

What I found: the site is behind a ModSecurity (SpiderLabs) Web Application Firewall (2 requests used).

Why it matters: knowing a WAF is present tells an attacker that simple attacks will likely be blocked, and it reveals part of the security architecture.

<img width="1279" height="613" alt="wafwoof" src="https://github.com/user-attachments/assets/749a4533-57a6-4e2a-a4b5-561b4c6066c7" />


Step 6. DNSRecon
bash
dnsrecon -d networkwalks.com

What I found:

SOA and NS records pointing to ns6135.hostgator.com and ns6136.hostgator.com
MX record: mail.networkwalks.com (192.232.216.135)
A record: 192.232.216.135
SPF record: v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all
A Google site verification TXT record
8 SRV records for _autodiscover._tcp (cPanel email discovery)
No answer for the DNSSEC query
DNSRecon reported recursion enabled on both name servers

Why it matters: DNS records help build a broader profile of the mail, hosting and email infrastructure.

<img width="1279" height="613" alt="dnsrecon" src="https://github.com/user-attachments/assets/db3c5294-e79c-49a0-beca-929c64486c4e" />


🗺️ Phase 2: Scanning & Network Discovery with Zenmap

Target: my own local network 192.168.180.0/24

Step 7. Ping scan

I identified my local IP address and LAN subnet, entered the subnet into Zenmap and selected the Ping scan profile, which runs:

bash
nmap -sn 192.168.180.0/24

What I found:

Multiple live hosts on the subnet, for example 192.168.180.39, 192.168.180.40, 192.168.180.41 and 192.168.180.52
The Nmap output lists the IP and MAC address of each host
Several consecutive addresses (for example 192.168.180.50 to 192.168.180.56) reported the same MAC address, 6C:3B:6B:F7:46:CD (Routerboard.com). This may indicate that one device, likely the router or gateway, is responding on behalf of several addresses.

Why it matters: host discovery shows an attacker (or a defender) which devices are present on the network.

<img width="725" height="614" alt="image" src="https://github.com/user-attachments/assets/6d5176c9-dd11-47e9-a3c0-b3eef1e25300" />


Step 8. Network topology

After the scan, I opened the Topology tab in Zenmap to view the discovered hosts around the scanning machine.

Why it matters: the topology gives a visual map of the network, which helps with documentation and with spotting unknown devices.

<img width="725" height="614" alt="image" src="https://github.com/user-attachments/assets/a8eaf7f0-c7a2-4fe8-b352-6366fd6e1084" />


⚠️ Risk Analysis
#	Finding	Evidence	Potential Impact	Risk
1	Web technology information exposed	WhatWeb identified WordPress and WP Download Manager	Version information can help find software that needs a security review	🟡 Medium
2	Server IP address identifiable	Nslookup resolved the domain to 192.232.216.135	Shows where the web service is hosted	🟢 Low
3	HTTP technical information exposed	Curl returned headers and /wp-json/	May help technology fingerprinting and enumeration	🟢 Low
4	WAF technology identifiable	Wafw00f identified ModSecurity (SpiderLabs)	Reveals part of the security architecture	🟢 Low
5	DNS infrastructure information exposed	DNSRecon found DNS, mail and service records, and recursion enabled on the name servers	Helps build an infrastructure profile	🟡 Medium
6	Multiple live hosts visible on the local network	Zenmap found multiple live hosts on 192.168.180.0/24	Unknown or unauthorized devices may be present	🟡 Medium

Note: these are observations from information gathering, not confirmed vulnerabilities. No exploitation or vulnerability validation was performed. Further authorized testing would be needed to confirm any actual vulnerability.

✅ Recommendations
Review publicly exposed technology information (CMS, plugins, versions).
Keep software updated and check it against current security advisories.
Review HTTP headers and remove unnecessary technical information.
Review DNS records regularly, including whether recursion is needed on the name servers.
Keep the WAF (ModSecurity) enabled and tuned.
Scan your own network regularly to identify active devices.
Investigate unknown devices found during scanning.
Maintain network documentation and topology diagrams.
Only test with authorization.
💡 What I Learned
1. Information gathering comes first

Before attempting to exploit anything, a lot can be learned from public information such as domain records, headers and DNS.

2. Each tool shows a different piece

WHOIS shows registration details, WhatWeb the technologies, Nslookup the IP address, Curl the headers, Wafw00f the firewall and DNSRecon the DNS records.

3. Observation is not vulnerability

Seeing a software version, an IP address or a DNS record does not by itself mean a system is vulnerable.

4. Host discovery

Zenmap and Nmap ping scans quickly show which devices are alive on a network, with their IP and MAC addresses.

5. Documentation

A good report explains what was done, what was found, what it means, the risk, and how to reduce it.

6. Authorization

Reconnaissance and scanning must always stay within an authorized scope.

🔐 Security & Ethical Use

I performed these activities only on systems where I had written permission or on devices that I own. This material is for education and research purposes only. Misuse can lead to criminal charges, heavy fines and a permanent record. Every action you take is your own responsibility.

🔗 Tools & Resources
Kali Linux: https://kali.org/get-kali
Nmap / Zenmap: https://nmap.org
WhatWeb: https://github.com/urbanadventurer/WhatWeb
wafw00f: https://github.com/EnableSecurity/wafw00f
DNSRecon: https://github.com/darkoperator/dnsrecon
👤 Author

Liam Ndiva
Cybersecurity Professional B084

LinkedIn: https://www.linkedin.com/feed/

📌 Project Information

Program Name: Cybersecurity at Networkwalks | Week: 02 | Project: Footprinting & Network Scanning | Repository: GitHub
