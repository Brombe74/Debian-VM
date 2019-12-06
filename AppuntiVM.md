---
title: SISTEMI
created: '2019-11-12T09:40:54.373Z'
modified: '2019-12-05T10:59:51.462Z'
---

# SISTEMI

## 13/11/2019
Implementare delle regole di firewall secondo questa tabella 

|da/a|LAN|WAN|DMZ|
| :---: | :---: | :---: | :---: |
|LAN| V| V(dns) | V*2
|WAN| X| V | V*2(dnat)
|DMZ| X| V(dns,http,ntp)| V



(Mettere nella relazione gli alias del quaderno)



<p style="color:red";>**CONFIGURAZIONI DEL ROUTER**</p>

**LAN**

|Action|Proto|Source|Port|Destination|Port|Description|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|Pass|any| Lan net|any|any|any|Default LAN -> any  
|Pass|TCP|LAN net|53(DNS)|host-router-lan|53(DNS)|Pass: LAN to Router-LAN-DNS
|Block|TCP/UDP|LAN net| 53(DNS)|WAN address|53(DNS)|Block:LAN to WAN-DNS
|Pass|TCP/UDP|LAN net|any|DMZ net|80(HTTP)|Pass: LAN to DMZ-HTTP
|Pass|TCP/UDP|LAN net|any|DMZ net|443(HTTPS)|Pass: LAN to DMZ-HTTPS

**WAN**

|Action|Proto|Source|Port|Destination|Port|Description|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|Pass|TCP|host-pcospitante|any|WAN address|80|Allow: accesso web al m0n0wall dal PC ospitante  
|Pass|TCP|any|any|host-server|22| NAT Server in SSH 

**DMZ**

|Action|Proto|Source|Port|Destination|Port|Description|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|Pass|TCP|any|any|host-client|22(SSH)|NAT DMZ to LAN in SSH port 2222
|Block|TCP|any|any|host-client|22(SSH)|Block: DMZ to LAN in SSH
|Pass|TCP/UDP|DMZ net|any|host-client|22(SSH)|Allow: DMZ to Client, normally DISABLED
|Block|any|DMZ net|any|LAN net| any|Block: DMZ to LAN  
|Pass| any|DMZ net|any|any|any|Allow: DMZ to any, normally DISABLED
|Pass|ICMP|DMZ net|any|any|any|Pass: DMZ to WAN-ICMP
|Pass|TCP/UDP|DMZ net|any|any|any|Pass: DMZ to WAN-DNS
|Pass|TCP/UDP|DMZ net|123|any|123|Pass: DMZ to WAN-NTP
|Pass|TCP/UDP|DMZ net|any|apt-cacher|3142|Pass: DMZ to apt-cacher(server aggiornamenti)

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
