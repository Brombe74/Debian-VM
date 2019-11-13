---
title: SISTEMI
created: '2019-11-12T09:40:54.373Z'
modified: '2019-11-13T10:41:38.897Z'
---

# SISTEMI

## 13/11/2019
Implementare delle regole di firewall secondo questa tabella 

|da/a|LAN|WAN|DMZ|
| :---: | :---: | :---: | :---: |
|LAN| V| V(dns) | V*2
|WAN| X| V | V*2(dnat)
|DMZ| X| V(dns,http,ntp)| V

<p style="color:red";>**FIREWALL->NAT->Inbound**</p>

External port range: from->SSH
                     to->other
Nat IP: host server, è un alias
Local Port: SSH
Descrizione: Server in SSH
metto la spunta

<p style="color:red";>**Firewall->Rules**</p>

**LAN**
Action: Block
Protocol: TCP/UDP
Source: LAN subnet
Source port range: DNS
Destination: WAN address
Destination port range: DNS
Descrizione: Block: Lan to WAN-DNS

Protocol: TCP/UDP
Source: LAN
Port: any
Destination: DMZ
Port: HTTP
Descrizione: Pass: LAN to DMZ (HTTP)
fare un'altra con https

**WAN**
Protocol: TCP/UDP
Source: WAN
Port: any
Destination: DMZ
Port: HTTP
Descrizione: Pass: WAN to DMZ (HTTP)
fare un'altra con https

**DMZ**
Protocol: ICMP
ICMP type: any
Souce: DMZ
Destination: WAN
Descrizione: DMZ to WAN (ICMP)

Protocol: TCP/UDP
Source: DMZ
Port: any
Destination: WAN
Port: DNS
Descrizione DMZ to WAN (DNS)
