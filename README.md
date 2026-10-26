# MAN-IN-THE-MIDDLE-ATTACK-PROJECT
MAN-IN-THE-MIDDLE-ANALYSIS-PROJECT

THIS IS MY MAN-IN-THE-MIDDLE ATTACK ANALYSIS HANDS ON PRACTICAL PROJECT WITH TRYHACKME

Man-in-the-Middle (MITM) Forensics & Protocol Dissection Lab: ARP Spoofing, DNS Poisoning & SSL Stripping Analysis. A hands-on network forensic project focusing on detecting active interception vectors, identifying forged ARP/DNS responses, and capturing plaintext credentials from downgraded HTTP streams using Wireshark.

Table of Contents

Project Overview
MITM Attack Concepts & Indicators
Tools & Technologies Used
Key Investigation Areas
Conclusion

📌 Project Overview

A Man-in-the-Middle (MITM) attack occurs when an adversary secretly intercepts and potentially alters communications between two trusting parties without their knowledge. Attackers leverage MITM vectors for eavesdropping, credential theft, and session hijacking. This project investigates real-world PCAP data to isolate three core MITM attack stages: ARP Cache Poisoning, DNS Spoofing, and SSL Stripping.

⚙️ MITM Attack Concepts & Indicators

Primary Attack Factors:
Interception: Routing victim traffic through an attacker-controlled host via layer-2 or layer-3 spoofing.

Manipulation/Decryption: Modifying network payloads or downgrading encrypted connections to plaintext.

Attack Vectors & Forensics Indicators Analyzed:
ARP Spoofing: Attacker sends unsolicited or forged ARP replies (arp.opcode == 2) associating their MAC address with the default gateway IP (192.168.10.1), leading to duplicate MAC-to-IP mappings and abnormal ARP traffic volume.

DNS Spoofing: Injecting forged DNS responses from an unauthorized source IP containing malicious A-records to redirect victim traffic away from legitimate domains (e.g., corp-login.acme-corp.local).

SSL Stripping: Intercepting secure HTTPS requests and downgrading them to unencrypted HTTP connections, allowing the attacker to harvest plaintext form data and credentials via HTTP POST streams.

🛠️ Tools & Technologies Used
Wireshark: Deep packet analysis, protocol filtering, stream follow/reassembly, and MAC/IP mapping verification.

Display Filters Used:
arp.opcode == 2 (ARP Replies)
arp.isgratuitous (Gratuitous ARP Announcements)
dns.flags.response == 1 && dns.qry == "..." (DNS Query Responses)
http && ip.src == ... && ip.dst == ... (HTTP Session Streams)

🔍 Key Investigation Areas

ARP Spoofing & Cache Poisoning Detection:

Identified the legitimate gateway MAC address (02:aa:bb:cc:00:01) vs. the attacker's impersonating MAC address (02:fe:fe:fe:55:55).
Isolated 2 gratuitous ARP replies and identified a total of 14 spoofed ARP packets originating from the attacker.

DNS Spoofing & Traffic Redirection:
Analyzed 211 total DNS responses for corp-login.acme-corp.local.
Filtered responses originating from unauthorized resolver IPs (excluding 8.8.8.8) to uncover the attacker's forged DNS response returning destination IP 192.168.10.55.

SSL Stripping & Credential Harvesting:
Isolated HTTP POST requests transmitted over unencrypted sessions redirected to the attacker's host (192.168.10.55).
Reconstructed the underlying TCP stream to extract victim credentials in plain text (Secret123!).

🛡️ Conclusion

Detecting MITM attacks requires continuous monitoring of Layer 2 ARP broadcasts, DNS resolution paths, and encryption downgrades. Implementing Static ARP tables, Dynamic ARP Inspection (DAI), DNSSEC, and strict HTTP Strict Transport Security (HSTS) headers is essential to defend against interception and credential harvesting attacks.
