## OSPFv2 – Single Area Configuratie (Volledig Overzicht)

### 🔧 OSPF Activeren

```bash
en
conf t
router ospf [process-id]  # 1–65535
```

> Kies overal dezelfde process-id voor consistentie.

### 🆔 Router-ID instellen

**a) Via loopback-interface (wordt niet geadverteerd tenzij toegevoegd):**

```bash
en
conf t
interface loopback1
ip address 1.1.1.1 255.255.255.255
```

**b) Expliciet instellen:**

```bash
en
conf t
router ospf 10
router-id 1.1.1.1
end
```

**c) Router-ID wijzigen (herstart vereist):**

```bash
en
conf t
router ospf 10
router-id 1.1.1.1
end
clear ip ospf process
```

**Verificatie:**

```bash
show ip protocols | include router id
```

### 🌐 Netwerken toevoegen aan OSPF

**1. Netwerk met wildcard mask:**

```bash
router ospf 10
network 10.10.10.0 0.0.0.255 area 0
```

**2. Interface-gebaseerd:**

```bash
interface g0/0/0
ip ospf 10 area 0
```

**3. Exact IP match:**

```bash
router ospf 10
network 10.10.1.1 0.0.0.0 area 0
```

> `area 0` is vereist (backbone). Wildcard mask = omgekeerd subnetmasker.

### 🚫 Passive Interface & Netwerktype instellen

```bash
router ospf 10
passive-interface loopback0
```

**Point-to-point netwerktype (geen DR/BDR):**

```bash
ip ospf network point-to-point  # Alleen tussen 2 routers
```

### 📡 DR/BDR Verkiezing

**Verificatie:**

```bash
show ip ospf neighbor
```

**Statussen:**

* * **FULL/DR**: Volledige burrelatie met de Designated Router.
* **FULL/BDR**: Volledige burrelatie met de Backup Designated Router.
* **FULL/DROTHER**: Volledige burrelatie met een router die geen DR of BDR is.
* **2WAY/DROTHER**: OSPF heeft een tweezijdige relatie met een router die geen DR of BDR is — geen burrelatie opgebouwd.

**Standaard verkiezing:**

* Hoogste interface priority (0–255)
* Bij gelijke priority: hoogste router-id

**Instellen:**

```bash
interface g0/0/0
ip ospf priority 255
end
clear ip ospf process
```

### 🧮 Kosten & Bandbreedte

**Interfacekost instellen:**

```bash
interface g0/0/0
ip ospf cost 100  # Voor FastEthernet (100 Mbps)

# Andere standaardwaarden:
# ip ospf cost 1     # 10 Gbps
# ip ospf cost 10    # 1 Gbps
# ip ospf cost 1000  # 10 Mbps
```

**Referentiebandbreedte aanpassen:**

```bash
router ospf 10
auto-cost reference-bandwidth 10000   # 10 Gbps
auto-cost reference-bandwidth 1000    # 1 Gbps
auto-cost reference-bandwidth 100     # FastEthernet
auto-cost reference-bandwidth 10      # 10 Mbps
```

> Interface-costs: 1 (10Gb), 10 (1Gb), 100 (FastEth), 1000 (10Mb)

**Controle:**

```bash
show ip ospf interface
```

### 🔁 Hello & Dead Interval

**Instellen:**

```bash
interface g0/0
ip ospf hello-interval 5
ip ospf dead-interval 15
```

**Terugzetten naar standaard:**

```bash
no ip ospf hello-interval
no ip ospf dead-interval
```

### 🌍 Default Route Propageren

**Statische default route maken:**

```bash
ip route 0.0.0.0 0.0.0.0 loopback1
```

**Adverteren via OSPF:**

```bash
router ospf 10
default-information originate
```

### 🔍 Verificatiecommando’s

```bash
show interfaces brief
show ip route
show ip ospf neighbor
show ip protocols
show ip ospf
show ip ospf interface
```

---
