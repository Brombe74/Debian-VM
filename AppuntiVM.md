---
title: SISTEMI
created: '2019-11-12T09:40:54.373Z'
modified: '2020-01-18T08:21:12.968Z'
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

### 29/11/2019

Collaudo DNS
host www.e-fermi.it oppure host 176.15.10.219 (opzionale: 8.8.8.8, server che farà la risoluzione)

Installare elinx su macchina 

Firewall->Rules
Name: apt-cacher
IP: 172.30.1.199
Description: Server updates

LAN->MONOWALL Porta 80  NO RESTRIZIONI
LAN->SERVER Porta 80

Server->MONOWALL Porta 8080 RESTRIZIONI
WAN->MONOWALL Porta 8080

### 5/12/2019
server
ip : 192.1683.112.250
GUARDARE SCREEN 
sudo systemctl status apache2 =vedere se è stato installato con successo 
sudo /var/www/html
nano index.html
modificare mettere il cognome davanti ad "apache2 default *debian si cancella* default page"

dal client andare su 192.168.112.250
apt install apache2 --fix-missing


### 14/12/2019

snake oil
*fare hostname -f*

### 19/12/2019

VPN

C1 S1    C2 S2
\  /      \ /
 0         0
 |         |
    RETE 
        \
        SX

da client 1 pingare il client 2 
VPN IPsec

IPsec nato quando non si usava il NAT

Ai tempi se da C1 dovevo andare ad SX, faceva da semplice *"switch"*
Da C1 mando un qualcosa a C2, R2 vede che hanno mandato un qualcosa, R2 riesce a decrittare l'ipsec e i livelli superiori in modo che C2 non si accorga che ipsec sia stato usato
Una volta nato il NAT questa metodologia non andava più bene, si è iniziato ad usare una ipsec di tipo tunnel.
Crea un livello 3 in più, quello sotto è *"ipsec"* e come indirizzo avrà indirizzo di rete pubblico e destinazione il pubblico del router destinatario, nell'altro ci sarà l'indirizzo privato di C1 e C2
Provare a pingare S2 con C1

### 16/01/2020

openvpn funziona su userspace (5-6-7)

chiave simmetrica 

ip dovranno essere tipo 192.168.200+lato.1
log -append, crea file di log 
tail -f nomefile 

**GUIDA** https://wiki.debian.org/OpenVPN

### 18/01/2020

*dentro il file /etc/openvpn/tun.conf*
dev tunnumerogrande
port 1194
proto udp 
ifcpmfog ip1 ip2
*client*remote ipwan2
secret percorso file chiave */etc/openvpn/chiave.key*
log-append /var/log/openvpn-miavpn.log
comp-lzo //direttiva per comprimere il traffico 
keepalive 10 120


