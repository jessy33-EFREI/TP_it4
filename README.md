# Ceci est l'endroit ou tu peux retrouver tout les TP du légendaire it4

# TP numéro 1

## 1. LAN
Voici le tableau d'adressage :

node1.tp1.efrei ==>	192.168.1.1/24

node2.tp1.efrei ==>	192.168.1.2/24
### 1.1. Les adresses MAC.
On commence par déterminer les adressesMAC des deux machines.

> Machine 1:

```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 10000
RHOST:PORT  : 127.0.0.1:10001
MTU:        : 1500
```
On a donc `00:50:79:66:68:00` pour la première machine.

> Machine 2:

```
PC2> show ip

NAME        : PC2[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 10008
RHOST:PORT  : 127.0.0.1:10009
MTU:        : 1500
```

Et `00:50:79:66:68:01` pour la seconde machine.

### 1.2. Les adresses IP.

Une IP pour PC1:
```
PC1> ip 192.168.1.1/24
Checking for duplicate address...
PC1 : 10.1.1.1 255.255.255.0

PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.1/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 10000
RHOST:PORT  : 127.0.0
```
(On vérifie bien que l'addresse est bien appliquée avec show ip)

Et une IP pour PC2:
```
PC2> ip 10.1.1.2/24
Checking for duplicate address...
PC1 : 10.1.1.2 255.255.255.0

PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 10008
RHOST:PORT  : 127.0.0.1:10009
MTU:        : 1500
```
### 1.3. Le ping.
On vérifie que les deux machines se ping bien.

**De PC1 à PC2:**
```
PC1> ping 192.168.1.2
84 bytes from 192.168.1.2 icmp_seq=1 ttl=64 time=0.254 ms
84 bytes from 192.168.1.2 icmp_seq=2 ttl=64 time=0.436 ms
84 bytes from 192.168.1.2 icmp_seq=3 ttl=64 time=0.214 ms
```

**De PC2 à PC1 :**
```
PC2> ping 192.168.1.1
84 bytes from 192.168.1.1 icmp_seq=1 ttl=64 time=0.756 ms
84 bytes from 192.168.1.1 icmp_seq=2 ttl=64 time=0.458 ms
84 bytes from 192.168.1.1 icmp_seq=3 ttl=64 time=0.117 ms
```
### 1.4. Wireshark

Les deux machines communiquent.
Quel est le protocole utilisé pour le ping ==> ICMP.

### 1.5 Requêtes ARP

On veut connaître les adresses MAC des deux machines.
==> ping de PC1 vers PC2 // observer les requêtes ARP.

Après ping :
```
PC1> arp

00:50:79:66:68:01  192.168.1.2 expires in 117 seconds
```
==> L'adresse MAC de PC2 est `00:50:79:66:68:01`.

On peut répéter l'opération dans l'autre sens, ici je te fais pas perdre ton temps c'est la même chose.

## 2. Ajoutons un switch

On ajoute un PC3 avec 192.168.1.3/24 :

node3.tp1.efrei	: 192.168.1.3/24

### 2.1 Déterminer les adresses MAC + attribuer les IP

a) Déterminer les adresses MAC des trois machines.
b) On en profite pour vérifier que les IP sont bien attribuées.

> Machine 1:
```
PC1> ip 192.168.1.1/24
Checking for duplicate address...
PC1 : 192.168.1.1 255.255.255.0

PC1> show ip

NAME        : PC1[1]
IP/MASK     : 192.168.1.1/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 10006
RHOST:PORT  : 127.0.0.1:10007
MTU:        : 1500
```

> Machine 2:
```
PC2> ip 192.168.1.2/24
Checking for duplicate address...
PC1 : 192.168.1.2 255.255.255.0

PC2> show ip

NAME        : PC2[1]
IP/MASK     : 192.168.1.2/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 10008
RHOST:PORT  : 127.0.0.1:10009
MTU:        : 1500
```
> Machine 3:
```
PC3> ip 192.168.1.3/24
Checking for duplicate address...
PC3 : 192.168.1.3 255.255.255.0

PC3> show ip

NAME        : PC3[1]
IP/MASK     : 192.168.1.3/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 10009
RHOST:PORT  : 127.0.0.1:10009
MTU:        : 1500
```

### 2.2. Le ping.

On ping toutes les machines entre elles :
```
PC1> ping 192.168.1.2  
84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=0.324 ms  
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=0.485 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=0.362 ms
PC1> ping 192.168.1.3  
84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=0.244 ms  
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=0.264 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=0.224 ms

PC2> ping 10.1.1.1
84 bytes from 10.1.1.1 icmp_seq=1 ttl=64 time=0.154 ms
84 bytes from 10.1.1.1 icmp_seq=2 ttl=64 time=0.365 ms
84 bytes from 10.1.1.1 icmp_seq=3 ttl=64 time=0.254 ms
PC2> ping 10.1.1.3
84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=0.458 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=0.410 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=0.345 ms
```

# 3. Serveur DHCP

On va maintenant ajouter un serveur DHCP pour attribuer les IP Dynamiquement.

# 3.1. Shéma

La table d'adressage

Machines | IP 
node1.tp1.efrei | pas encore d'ip 
node2.tp1.efrei	| pas encore d'ip 
node3.tp1.efrei	| pas encore d'ip 
dhcp.tp1.efrei | 10.1.1.253/24

## 3.1.1. Configuration du serveur DHCP.

On commence par fournir un accès internet au serveur DHCP.
```
PING www.google.fr(fra16s56-in-x03.1e100.net (2a00:1450:4001:82f::2003)) 56 data bytes
64 bytes from fra16s56-in-x03.1e100.net (2a00:1450:4001:82f::2003): icmp_seq=1 ttl=111 time=5.29 ms
64 bytes from fra16s56-in-x03.1e100.net (2a00:1450:4001:82f::2003): icmp_seq=2 ttl=111 time=3.23 ms
64 bytes from fra16s56-in-x03.1e100.net (2a00:1450:4001:82f::2003): icmp_seq=3 ttl=111 time=3.24 ms
64 bytes from 2a00:1450:4001:82f::2003: icmp_seq=4 ttl=111 time=3.29 ms

--- www.google.fr ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3003ms
rtt min/avg/max/mdev = 3.225/3.761/5.290/0.882 ms
```

**L'accès internet fonctionne**

On met une IP fixe à notre DHCP

Ensuite, on installe le serveur DHCP.

`dnf-y install dhcp-server`
`nano /etc/dhcp/dhcpd.conf`
`cat /etc/dhcp/dhcpd.conf` :
```
authoritative;
subnet 192.168.1.0 netmask 255.255.255.0 {
	range 192.168.1.10 192.168.1.100;
	option broadcast-address 192.168.1.1;
	option routers 192.168.1.1;
}
```
On active le service DHCP:

`systemctl enable --now dhcpd`

### 3.1.2. Récupérer une IP pour un client

> Machine 1:
```
PC1> dhcp   
  
PC1> show ip  
  
NAME        : PC1\[1\]  
IP/MASK     : 192.168.1.10/24  
GATEWAY     : 192.168.1.1  
DNS         :  
DHCP SERVER : 192.168.1.5  
DHCP LEASE  : 42892, 42898/21449/37535  
MAC         : 00:50:79:66:68:02  
LPORT       : 10008  
RHOST:PORT  : 127.0.0.1:10009  
MTU:        : 1500  
```
**Même principe pour les deux autres machines gars.**

### 3.1.3. DHCP spoofing :

On observere les échanges entre le serveur DHCP et une machine.

On va tenter de faire du DHCP spoofing ==> on essayera de récupérer les requêtes DHCP d'une machine cliente, et de lui attribuer une IP différente.
On part d'une VM rocky Linux, comme conseillé dans le cours.

### 3.2.1 Configuration du serveur

Premièrement, on installe dnsmasq.

`dnf install -y dnsmasq`
`cat /etc/dnsmasq.conf` :
```
port=0
dhcp-range=192.168.1.10,192.168.1.100,255.255.255.0,12h
dhcp-authoritative
interface=enp0s3
```
`systemctl enable --now dnsmasq`

Petit test :
```
PC3> dhcp

PC3> show ip

NAME        : PC3[1]
IP/MASK     : 255.255.255.0
GATEWAY     : 192.168.1.250
DNS         :
DHCP SERVER : 192.168.1.6
DHCP LEASE  : 43049, 43200/21600/37800
MAC         : 00:50:79:66:68:00
LPORT       : 10012
RHOST:PORT  : 127.0.0.1:10013
MTU:        : 1500
```
On va connecter les deux serveurs DHCP sur le même switch pour voir si on peut récupérer des ip différentes.

> Machine 4:
```
PC4> dhcp
IP 192.168.1.13/24 GW 192.168.1.250
```
On voit que PC4 récupere son ip du second dhcp, le notre.

On va observer le serveur **légitime** est enregistré avec wireshark.

On y verra que le dhcp **illégitime** ne récupère qu'une seule requête, et le dhcp **légitime** récupère le reste... 
La solution serait de ralentir le légitime pour que l'illégitime intercepte plus de requetes.

________________________________________________________________________________________________________________________

©  2024. PERTUS Jessy. Tous droits réservés.
