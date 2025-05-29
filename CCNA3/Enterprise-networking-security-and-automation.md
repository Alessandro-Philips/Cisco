# 📘 CCNA3 - Enterprise Networking, Security, and Automation

## 🔭 OSPF - Databases en Verificatiecommando's

OSPF gebruikt drie types van databases om correcte routing te garanderen:

### 📂 1. Adjacency Database (Neighbor Table)
```bash
show ip ospf neighbor
```

### 📂 2. Link-State Database (LSDB / Topology Table)
```bash
show ip ospf database
```

### 📂 3. Forwarding Database (Routing Table)
```bash
show ip route
```

---

## 📍 Routing Concepten en Verwerking

### 🧐 Best Path Logic & Forwarding
```bash
show ip route
show ipv6 route
```

### 📦 Cisco Packet Forwarding
- Process switching
- Fast switching
- CEF (Cisco Express Forwarding)

### 🛠️ Basisconfiguratie router
```bash
hostname R1
enable secret class
line console 0
 logging synchronous
 password cisco
 login
line vty 0 4
 password cisco
 login
 transport input ssh telnet
service password-encryption
banner motd # WARNING: Unauthorized access is prohibited! #
```

### 🌐 Interfaceconfiguratie
```bash
interface g0/0/0
 ip address 10.0.1.1 255.255.255.0
 ipv6 address 2001:db8:acad:1::1/64
 ipv6 address fe80::1:a link-local
 no shutdown
```

---

## 📌 Static Routing

### 📘 IPv4 Static Route
```bash
ip route [network-address] [subnet-mask] [ip-address | exit-intf [ip-address]] [distance]
```

### 📘 IPv6 Static Route
```bash
ipv6 route [ipv6-prefix/prefix-length] [ipv6-address | exit-intf [ipv6-address]] [distance]
```

### ✅ IPv4 Examples
```bash
ip route 172.16.1.0 255.255.255.0 172.16.2.2
ip route 192.168.1.0 255.255.255.0 s0/1/0
ip route 192.168.2.0 255.255.255.0 s0/1/0 172.16.2.2
```

### ✅ IPv6 Examples
```bash
ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:2::2
ipv6 route 2001:db8:cafe:1::/64 s0/1/0
ipv6 route 2001:db8:cafe:2::/64 s0/1/0 2001:db8:acad:2::2
```

---

## 🌍 Default Routes (quad-zero & ::/0)

### IPv4
```bash
ip route 0.0.0.0 0.0.0.0 172.16.2.2
```

### IPv6
```bash
ipv6 route ::/0 2001:db8:acad:2::2
```

---

## ↺ Floating Static Routes

Gebruik deze met een hogere administrative distance als back-up:
```bash
ip route 0.0.0.0 0.0.0.0 10.10.10.2 5
ipv6 route ::/0 2001:db8:feed:10::2 5
```

---

## 🧑‍⚖️ Static Host Routes

### IPv4 Host Route
```bash
ip route 209.165.200.238 255.255.255.255 198.51.100.2
```

### IPv6 Host Route
```bash
ipv6 route 2001:db8:acad:2::238/128 2001:db8:acad:1::2
```

### IPv6 Host Route met link-local next-hop
```bash
ipv6 route 2001:db8:acad:2::238/128 fe80::2 link-local-interface
```

---

## 🔎 Verificatie Static Routes
```bash
show ip route static
show ipv6 route static
show ip route | include [network]
show running-config | section ip route
```

---

## ⚠️ Troubleshooting Static & Default Routes

### Belangrijk gedrag
- Packet zonder match in routing table ➔ **drop** + ICMP unreachable
- Static route heeft voorkeur als er geen dynamic route aanwezig is
- Default route vangt alles op dat geen specifieke match vindt

### Packetverwerking over meerdere routers
1. R1 ontvangt pakket van PC1 ➔ controleert routing table
2. Geen match? ➔ gebruik default static route
3. Nieuwe encapsulation + forwarding via S0/1/0
4. R2 ontvangt, herhaalt proces ➔ stuurt door naar R3
5. R3 vindt directe match met PC3 subnet, stuurt ARP request, verzendt naar PC3

### Handige commando’s bij troubleshooting
```bash
ping [ip-adres]
traceroute [ip-adres]
show ip interface brief
show ip route
show running-config
show arp
```

### Fout oplossen voorbeeld
```bash
R2(config)# no ip route 172.16.3.0 255.255.255.0 192.168.1.1
R2(config)# ip route 172.16.3.0 255.255.255.0 172.16.2.1
```

### Routingtable bekijken vanaf bepaald punt
```bash
show ip route | begin Gateway
```
