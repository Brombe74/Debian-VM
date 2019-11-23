---
title: SISTEMI
created: '2019-11-12T09:40:54.373Z'
modified: '2019-11-23T08:22:14.217Z'
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


(Mettere nella relazione gli alias del quaderno)

<p style="color:red";>**Firewall->Rules**</p>

**LAN**
Action: Block
Protocol: TCP/UDP
Source: any
Source port range: any
Destination: NOT  Single host or alias "host-router-lan"
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

### 21/11/2019

<p style="color:red";>**Firewall->Rules**</p>


**LAN**

Protocol: TCP/UDP
Source: LAN
Source port: any
Destination:NOT WAN address
Destination port: any
Descrizione: Pass: LAN to WAN any

Protocol: TCP
Source: DMZ subnet
Port: 2222 2222 
Destination: LAN
Port: other SSH
Descrizione:Pass: Pass: DMZ to LAN client- SSH port 2222 (normally disabled)

Action: Block
Protocol: TCP
Source: LAN
Port: any
Destination: DMZ 
Port: any
Descrizione: Block: LAN to DMZ

**WAN**

Protocol: 
Source: 
Port: 
Destination:
Port:
Descrizione: 

**DMZ**

Protocol: TCP
Source: LAN subnet
Port: SSH
Destination: DMZ
Port: other 2222
Descrizione:Pass: DMZ to LAN client- SSH port 2222 (normally disabled) 

Protocol: UDP
Source: DMZ 
Port: any
Destination: WAN 
port: 123
Descrizione: DMZ in WAN-NTP

Protocol: TCP/UDP
Source: DMZ 
Port: any
Destination: WAN 
port: other 3142
Descrizione: DMZ to WAN-Updates

Action: Block
Protocol: TCP/UDP
Source: DMZ 
Port: any
Destination: WAN 
port: other any
Descrizione: Block: DMZ to WAN

### 23/11/2019

<p style="color:red";>**CONFIGURAZIONI PRIMA DI AVER FATTO IL RESTORE**</p>

**LAN**

|Action|Proto|Source|Port|Destination|Port|Description|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|Block|TCP/UDP|any|any|!host-router-lan|port 53| Block:LAN to WAN-DNS 
|Pass|TCP/UDP|LAN net| any|DMZ net| 80| Pass: LAN to DMZ (HTTP)
|Pass|TCP/UDP|LAN net| any|DMZ net| 443| Pass: LAN to DMZ (HTTPS)
|Pass|TCP/UDP|DMZ net| 2222|LAN net| 22| Pass: DMZ to LAN client- SSH port 2222 (normally disabled)  
|Pass|any|LAN net| any|any|any| Default LAN -> any
|Block|TCP|LAN net| any|DMZ net|any| Default LAN -> any| Block: LAN to DMZ 
|Pass|TCP/UDP|LAN net| any|!WAN address|any| Pass: LAN to WAN any  

**WAN**

|Action|Proto|Source|Port|Destination|Port|Description|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|Pass|TCP|host-pcospitante| any|WAN address| 80| PAllow: accesso web al m0n0wall dal PC ospitante  
|Pass|TCP|any|any|host-server|22|NAT Server in SSH  
|Pass|TCP/UDP|WAN address|any|DMZ net|80|Pass: WAN to DMZ (HTTP)
|Pass|TCP/UDP|WAN address|any|host-server|443|Pass: WAN to DMZ (HTTPS)   

**DMZ**

|Action|Proto|Source|Port|Destination|Port|Description|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|Block|any|DMZ net|any|LAN net|any|Block: DMZ to LAN 
|Pass|any|DMZ net|any|WAN address|any|Allow: DMZ to any (normally disabled) 
|Pass|ICMP|DMZ net|any|WAN address|any|Pass: DMZ to WAN (ICMP)
|Pass|TCP|LAN net|22|DMZ net|2222|Pass: DMZ to LAN client- SSH port 2222 (normally disabled) 
|Pass|TCP/UDP|DMZ net|any|WAN address|53|Pass: DMZ to WAN (DNS) 
|Pass|UDP|DMZ net|any|WAN address|123|DMZ to WAN-NTP
|Pass|TCP/UDP|DMZ net|any|WAN address|3142|DMZ to WAN-Updates
|Block|TCP/UDP|any|WAN address|any|Block: DMZ to WAN

<p style="color:red";>**CONFIGURAZIONI DOPO AVER FATTO IL RESTORE**</p>

**LAN**

|Action|Proto|Source|Port|Destination|Port|Description|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|Pass|any| Lan net|any|any|any|Default LAN -> any  

**WAN**
|Action|Proto|Source|Port|Destination|Port|Description|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|Pass|TCP|host-pcospitante|any|WAN address|80|Allow: accesso web al m0n0wall dal PC ospitante  
|Pass|TCP|any|any|host-server|22| NAT Server in SSH 

**DMZ**
|Action|Proto|Source|Port|Destination|Port|Description|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|Block|any|DMZ net|any|LAN net| any|Block: DMZ to LAN  
|Pass| any|DMZ net|any|any|any|Allow: DMZ to any  


