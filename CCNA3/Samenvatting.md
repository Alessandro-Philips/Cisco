# Cisco CLI Samenvatting

## 🔑 ENABLE MODUS (zowel op switch als router)
Vanaf hier kun je systeemverificaties uitvoeren, basisstatus controleren, en configuratiemodus betreden.

```bash
enable
disable
show running-config
show startup-config
show ip interface brief
show interfaces
show version
copy running-config startup-config
```

---

## ⚙️ CONFIGURE TERMINAL (globale configuratiemodus)
Vanaf hier configureer je apparaatinstellingen.

```bash
configure terminal
```

---

## 🖧 SWITCH CONFIGURATIE

### 🎛️ Algemene instellingen
```bash
hostname SW1
service password-encryption
enable secret [wachtwoord]
banner motd #Unauthorized access prohibited!#
```

### 🔐 Console- en VTY-toegang
```bash
line console 0
 password [wachtwoord]
 login
 exit

line vty 0 15
 password [wachtwoord]
 login
 transport input ssh
```

### 🔑 SSH
```bash
ip domain-name voorbeeld.local
crypto key generate rsa
ip ssh version 2
username admin secret [wachtwoord]
```

### 🧱 VLAN’s
```bash
vlan 10
 name SALES
interface fastEthernet0/1
 switchport mode access
 switchport access vlan 10
 no shutdown
```

### 🎧 Voice VLAN
```bash
vlan 150
 name VOICE
interface fastEthernet0/2
 switchport mode access
 switchport access vlan 10
 switchport voice vlan 150
 mls qos trust cos
```

### 🌉 Trunk configuratie
```bash
interface gig0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,99
 no shutdown
```

### 🔗 EtherChannel (LACP)
```bash
interface range g0/1 - 2
 channel-group 1 mode active
 no shutdown
interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 10,20
```

### 🧪 STP
```bash
spanning-tree vlan 1 priority 24576
spanning-tree mode pvst
```

---

## 🌐 ROUTER CONFIGURATIE

### 🎛️ Algemene instellingen
```bash
hostname R1
service password-encryption
enable secret [wachtwoord]
```

### 🔐 Console- en VTY-toegang
```bash
line console 0
 password [wachtwoord]
 login
 exit

line vty 0 15
 password [wachtwoord]
 login
 transport input ssh
```

### 🔑 SSH
```bash
ip domain-name voorbeeld.local
crypto key generate rsa
ip ssh version 2
username admin secret [wachtwoord]
```

### 🌐 Interface config (IPv4 & IPv6)
```bash
interface g0/0
 ip address 192.168.1.1 255.255.255.0
 ipv6 address 2001:db8::1/64
 ipv6 address fe80::1 link-local
 no shutdown
```

### 🧪 Router-on-a-Stick
```bash
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

interface g0/0
 no shutdown
```

### 📦 DHCPv4 Server
```bash
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
```

### 📬 DHCP Relay
```bash
interface g0/1
 ip helper-address 192.168.1.1
```

### 🌍 DHCPv6 Stateless
```bash
ipv6 unicast-routing
ipv6 dhcp pool POOL1
 dns-server 2001:db8::1
 domain-name voorbeeld.local

interface g0/0
 ipv6 address 2001:db8:1::1/64
 ipv6 nd other-config-flag
 ipv6 dhcp server POOL1
```

### 🌍 DHCPv6 Stateful (Client)
```bash
interface g0/1
 ipv6 address dhcp
 ipv6 enable
```

### 🚦 Statische routes (IPv4)
```bash
ip route 192.168.2.0 255.255.255.0 10.0.0.2
ip route 0.0.0.0 0.0.0.0 10.0.0.1     # default
ip route 192.168.3.0 255.255.255.0 s0/0/0
ip route 192.168.4.0 255.255.255.0 s0/0/0 10.0.0.2
```

### 🚦 Statische routes (IPv6)
```bash
ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:2::2
ipv6 route ::/0 2001:db8::1
ipv6 route 2001:db8:acad:2::/64 s0/0/0
```

### 🔁 HSRP (First Hop Redundancy Protocol)
#### Actieve router (R1)
```bash
interface g0/0/1
 standby version 2
 standby 1 ip 172.16.10.1
 standby 1 priority 150
 standby 1 preempt
```

#### Standby router (R2)
```bash
interface g0/0/1
 standby version 2
 standby 1 ip 172.16.10.1
```

### 🔎 HSRP Verificatie
```bash
show standby
show standby brief
```

---

## 🧪 VERIFICATIE & STATUSCOMMANDO’S

### 🔎 Interface & IP-status
```bash
show ip interface brief
show ipv6 interface brief
show interfaces
show interfaces trunk
show interfaces switchport
```

### 🧭 Routing
```bash
show ip route
show ipv6 route
show ip protocols
```

### 📦 DHCP
```bash
show ip dhcp binding
show ip dhcp server statistics
show running-config | section dhcp
show ipv6 dhcp interface [interface-id]
```

### 🧾 VLAN & MAC
```bash
show vlan brief
show mac address-table
```

### 🔐 Beveiliging & SSH
```bash
show ip ssh
show users
show line vty 0 4
```

### 🧑‍🤝‍🧑 CDP / LLDP
```bash
show cdp neighbors
show cdp neighbors detail
show lldp neighbors
show lldp neighbors detail
```
