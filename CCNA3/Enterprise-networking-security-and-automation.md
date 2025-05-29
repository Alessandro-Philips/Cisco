# 📘 CCNA3 - Enterprise Networking, Security, and Automation

## 🛠️ OSPF - Open Shortest Path First

OSPF is een dynamisch link-state routing protocol dat gebruik maakt van gebieden om routing informatie efficiënt te verspreiden. Elke router bouwt een gedetailleerde kaart van het netwerk (LSDB) en gebruikt Dijkstra’s algoritme om de beste paden te berekenen.

### 🔹 Belangrijke OSPF componenten:

* **Router ID**: Unieke identificatie per router, meestal hoogste IP of handmatig ingesteld.
* **Area**: Logische verdeling van het OSPF-netwerk. Area 0 is de backbone.
* **Cost**: Routing metric gebaseerd op bandbreedte; lagere cost = betere route.
* **LSDB**: Link State Database, toont het volledige topologische overzicht.
* **DR/BDR**: Designated Router / Backup Designated Router in multi-access netwerken.

### 🧾 Basisconfiguratie (Single-Area)

```bash
router ospf 1
 router-id 1.1.1.1
 network 10.0.0.0 0.255.255.255 area 0
```

### Alternatief via interfaces

```bash
interface g0/0
 ip ospf 1 area 0
```

### Default Routes in OSPF

```bash
ip route 0.0.0.0 0.0.0.0 192.0.2.1
router ospf 1
 default-information originate
```

### Metrics & Cost

```bash
interface g0/0/0
 ip ospf cost 10
router ospf 1
 auto-cost reference-bandwidth 10000
```

### Passive Interfaces

```bash
router ospf 1
 passive-interface default
 no passive-interface g0/0
```

### Multiarea Configuratie

```bash
interface g0/1
 ip ospf 1 area 23
```

### Verificatiecommando’s

```bash
show ip ospf
show ip ospf neighbor
show ip ospf database
show ip ospf interface brief
```

---

## 🔧 Basisconfiguratie (Router/Switch)

### Vanaf `enable` mode:

```bash
show running-config
show startup-config
show version
show interfaces
```

### Vanaf `configure terminal`:

```bash
hostname R1
no ip domain-lookup
enable secret class
service password-encryption
banner motd # Unauthorized access is prohibited! #
```

### Console en VTY lijnen:

```bash
line console 0
 password cisco
 login
 logging synchronous
line vty 0 4
 password cisco
 login
 transport input ssh telnet
```

### Interface instellen:

```bash
interface g0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
```

## 🛰️ Statische Routing

### IPv4:

```bash
ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

### IPv6:

```bash
ipv6 route 2001:db8::/64 2001:db8::2
```

### Default route (quad-zero):

```bash
ip route 0.0.0.0 0.0.0.0 10.0.0.2
ipv6 route ::/0 2001:db8::2
```

### Floating static route:

```bash
ip route 192.168.3.0 255.255.255.0 10.0.0.2 5
```

## 🧪 Samenvattende show-commando’s

### Configuratie:

```bash
show running-config
show startup-config
show ip protocols
```

### Interface en route:

```bash
show ip interface brief
show ip route
show ip route ospf
```

### OSPF specifiek:

```bash
show ip ospf
show ip ospf interface
show ip ospf interface brief
show ip ospf interface [type number]
show ip ospf database
show ip ospf neighbor
```

### OSPF resetten:

```bash
clear ip ospf process
```

---

> Deze samenvatting combineert jouw oorspronkelijke overzicht en alle uitgebreide informatie uit het handboek en dia’s. Laat weten of je dit wil opdelen per module of onderwerp!
