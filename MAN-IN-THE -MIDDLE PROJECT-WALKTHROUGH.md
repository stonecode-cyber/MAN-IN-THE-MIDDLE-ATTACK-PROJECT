This is a project on MAN-IN-THE-MIDDLE attck where an attacker secretly intercepts and potentially alters communication between two parties, such as a user and a service, without their knowledge. The attacker may eavesdrop to steal sensitive data like credentials and credit card info or inject malicious content. Thi attack revolve around 2 factors which are:

1. :Interception
2. Manipulation/decryption 

Common type of MITM attacks are:

- Packet sniffing: 
- Session hijacking:
- SSL stripping
- DNS spoofing:
- IP spoofing
- Rogue Wi-Fi access point

Address resolution protocol spoofing (Arp spoofing) : attacker sends fake ARP replies to trick devices into associating the attacker’s MAC address with a legitimate IP, usually the default gateway. This allows the attacker to intercept, modify, or redirect traffic.
**ARP spoofimg indicator:**

- Duplicate MAC-to-IP Mappings:
- High number of ARP replies
- Abnormal ARP traffic volume
- Unusual Traffic Routing
- Gateway Redirection Patterns: 
- ARP Probe / Reply Loops
In ARP query:
arp.opcode == 1  mean request
arp.opcode ==2 indicate response

In the task below will be using the Tryhackme  wireshack data for this project:

**TASK 1: How many ARP packets from the gateway MAC Address were observed?**

In this task, firstly  i make use of the responce query command to see the MAC addresses _arp.opcode ==2_
This reveal the gate way MACA address of the device as shown below

<img width="481" height="405" alt="Image" src="https://github.com/user-attachments/assets/e3899f46-045e-4343-acf6-f2dde6cadd75" />
After i found out  the gateway  MAC address to be  **_02:aa:bb:cc:00:01_,** I then make use of the query to filter the packet from these gateway  MAC adrress  _**arp && arp.src.proto_ipv4 == 192.168.10.1 && eth.src == 02:aa:bb:cc:00:01**_
This filter show the total number of packer sent to be 10 as shown below;

<img width="479" height="410" alt="Image" src="https://github.com/user-attachments/assets/f4c5328f-dc8f-48e6-b31d-7ee2bd856762" />

**TASK 2: What MAC address was used by the attacker to impersonate the gateway?**
As shown below  the MAC address that sent the reponse to impersonate the legitimate MAC address is shown below to be _**02:fe:fe:fe:55:55**_

<img width="478" height="320" alt="Image" src="https://github.com/user-attachments/assets/2ef5417e-989e-4375-9a31-186163cc8941" />

**TASK 3: How many Gratuitous ARP replies were observed for 192.168.10.1?**
In this project i will be making us of the arp gratuitous query  and then add a query that filter the arp source  and ipv4 protocol to be 192.168.10.1 as _**arp.isgratuitous && arp.src.proto_ipv4 == 192.168.10.1**_.
This reveal the numbe rof replies to be 2 packet as shown below:

<img width="477" height="400" alt="Image" src="https://github.com/user-attachments/assets/eb7cba47-2746-4c0f-ae60-5f57a8e82bcc" />

**TASK 4:How many ARP spoofing packets were observed in total from the attacker?**
In this task, i make use of a query  to filter the arp response where the source IPv4 is  192.168.10.1 and the ethernet source is 02:fe:fe:fe:55:55 **_arp.opcode == 2 && arp.src.proto_ipv4 == 192.168.10.1 && eth.src == 02:fe:fe:fe:55:55_**
As shown below the total number of ARP spoofimg packet is 14

<img width="485" height="408" alt="Image" src="https://github.com/user-attachments/assets/c0ca4411-5e06-48f7-b3ab-89d0f26b500f" />

**DNS SPOOFING** 
In thi project, let us startby hinting on spomething to look out for during a DNSspoofiing man in thre middle attack  which are:

- Multiple DNS response to the sam query
- DNS responce from unexpected source 
- Suspicious short TTL
- Unsolicited DNS Response

TASK 1 :|How many DNS responses were observed for the domain corp-login.acme-corp.local?

In this task i make use of a query that filter all DNS  where the flag  is 1 (response) and the DNS.query is from corp-login.acme-corp.local?

**_dns.flags.response == 1 &&  dns.qry ==  "corp-login.acme-corp.local"_**

As shown below it display a total of** 211 packets**

<img width="483" height="437" alt="Image" src="https://github.com/user-attachments/assets/3a31dae9-bac7-4805-b423-522d6b4f34bd" />

TASK 2: How many DNS requests were observed from the IPs other than 8.8.8.8?
In this task i make use of the adjusted the first query to filter dns response where the domain is from corp-login.acme-corp.local"and the source ip is not 8.8.8.8.

_**dns.flags.response ==1 && ip.src !=8.8.8.8 && dns.qry =="corp-login.acme-corp.local"**_

<img width="488" height="413" alt="Image" src="https://github.com/user-attachments/assets/f3544419-7e15-4869-9f00-324e2672ac73" />
As shown above there are 2 packet in which the src IP is not from 8.8.8.8

TASK 3: What IP did the attacker’s forged DNS response return for the domain?
As shown  in the screenshot of the task above the response come from an unextected IP instead of 8.8.8.8 the responce came from the attacker IP which is **192.168.10.55**

<img width="486" height="410" alt="Image" src="https://github.com/user-attachments/assets/3bfc1b99-6f3d-4610-bd1e-6c6b630feac4" />

### **SSL STRIPING MITM ATTACK**

TASK 1: How many POST requests were observed for our domain corp-login.acme-corp.local?
in this task i use a query to filter the insecure packet connection with http where traffic was sent to the attacker IP  
_**http && ip.src == 192.168.10.10 && ip.dst == 192.168.10.55**_

<img width="484" height="392" alt="Image" src="https://github.com/user-attachments/assets/3140736a-c24d-45ab-83ee-ddffa5dc329b" />
As shown above we have only 1 POST response

TASK 2: What's the password of the victim found in the plaintext after successful ssl stripping attack. 
In this task , i right click on the POST packet, follow and do the tcp stream to view the detail of the packet as shown below:

<img width="469" height="362" alt="Image" src="https://github.com/user-attachments/assets/6002a165-1fe3-491b-8312-c4c42284b7d1" />

<img width="255" height="146" alt="Image" src="https://github.com/user-attachments/assets/f93e7a05-5692-42af-b915-d17b8522ce0b" />

As revea;led above the Password of the victim is Secret123!

