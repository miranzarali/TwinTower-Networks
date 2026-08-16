# TwinTower Networks — Configuration & Verification Commands

Cisco Packet Tracer enterprise campus network implementing a redundant two-building network.

---

## 1. Network Addressing

### Building 1 — Headquarters

| VLAN | Department | Network | HSRP VIP |
|---|---|---|---|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Sales | 192.168.20.0/24 | 192.168.20.1 |
| 30 | Finance | 192.168.30.0/24 | 192.168.30.1 |

### Building 2 — Branch Office

| VLAN | Department | Network | HSRP VIP |
|---|---|---|---|
| 40 | HR | 192.168.40.0/24 | 192.168.40.1 |
| 50 | Sales | 192.168.50.0/24 | 192.168.50.1 |
| 60 | Finance | 192.168.60.0/24 | 192.168.60.1 |

### Server Network

| VLAN | Purpose | Network | Gateway |
|---|---|---|---|
| 100 | Servers | 192.168.100.0/24 | 192.168.100.1 |

**Server**

```text
IP Address: 192.168.100.10
Subnet Mask: 255.255.255.0
Gateway: 192.168.100.1
Services: DHCP, DNS, HTTP
```

---

## 2. VLAN Configuration

VLANs provide logical Layer-2 segmentation between departments.

### Building 1

```text
vlan 10
 name HR

vlan 20
 name SALES

vlan 30
 name FINANCE
```

### Building 2

```text
vlan 40
 name HR

vlan 50
 name SALES

vlan 60
 name FINANCE
```

### Server VLAN

```text
vlan 100
 name SERVERS
```

---

## 3. Access Port Configuration

Access ports connect end devices to a single VLAN.

### Building 1

```text
interface range fa0/1-8
 switchport mode access
 switchport access vlan 10

interface range fa0/9-16
 switchport mode access
 switchport access vlan 20

interface range fa0/17-23
 switchport mode access
 switchport access vlan 30
```

### Building 2

```text
interface range fa0/1-8
 switchport mode access
 switchport access vlan 40

interface range fa0/9-16
 switchport mode access
 switchport access vlan 50

interface range fa0/17-23
 switchport mode access
 switchport access vlan 60
```

### Server Port

The centralized server is physically located in Building 1.

```text
interface fa0/24
 switchport mode access
 switchport access vlan 100
```

---

## 4. Trunk Configuration

Trunks carry multiple VLANs between network devices using IEEE 802.1Q.

### Building 1

```text
interface range fa0/1-3,g0/2
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
```

### Building 2

```text
interface range fa0/1-3,g0/2
 switchport mode trunk
 switchport trunk allowed vlan 40,50,60
```

### Adding Server VLAN 100

VLAN 100 was added to existing trunks without replacing the existing VLAN list.

```text
interface range fa0/1-3,g0/2
 switchport mode trunk
 switchport trunk allowed vlan add 100
```

### Why `add` is used

Using:

```text
switchport trunk allowed vlan add 100
```

adds VLAN 100 to the existing allowed VLAN list.

Using:

```text
switchport trunk allowed vlan 100
```

would replace the existing allowed VLAN list and could remove VLANs already configured on the trunk.

---

## 5. Layer-3 Switching and SVIs

Multilayer switches perform inter-VLAN routing using Switched Virtual Interfaces (SVIs).

```text
ip routing
```

### Building 1 — Primary MLS

```text
interface vlan 10
 ip address 192.168.10.2 255.255.255.0
 no shutdown

interface vlan 20
 ip address 192.168.20.2 255.255.255.0
 no shutdown

interface vlan 30
 ip address 192.168.30.2 255.255.255.0
 no shutdown
```

### Building 1 — Secondary MLS

```text
interface vlan 10
 ip address 192.168.10.3 255.255.255.0
 no shutdown

interface vlan 20
 ip address 192.168.20.3 255.255.255.0
 no shutdown

interface vlan 30
 ip address 192.168.30.3 255.255.255.0
 no shutdown
```

### Building 2 — Primary MLS

```text
interface vlan 40
 ip address 192.168.40.2 255.255.255.0
 no shutdown

interface vlan 50
 ip address 192.168.50.2 255.255.255.0
 no shutdown

interface vlan 60
 ip address 192.168.60.2 255.255.255.0
 no shutdown
```

### Building 2 — Secondary MLS

```text
interface vlan 40
 ip address 192.168.40.3 255.255.255.0
 no shutdown

interface vlan 50
 ip address 192.168.50.3 255.255.255.0
 no shutdown

interface vlan 60
 ip address 192.168.60.3 255.255.255.0
 no shutdown
```

---

## 6. HSRP Gateway Redundancy

HSRP provides a virtual default gateway so that the failure of one multilayer switch does not immediately break client connectivity.

The virtual gateway uses `.1`.

The primary MLS uses `.2` and the secondary MLS uses `.3`.

### Building 1 — Primary MLS

```text
interface vlan 10
 standby 10 ip 192.168.10.1
 standby 10 priority 110
 standby 10 preempt

interface vlan 20
 standby 20 ip 192.168.20.1
 standby 20 priority 110
 standby 20 preempt

interface vlan 30
 standby 30 ip 192.168.30.1
 standby 30 priority 110
 standby 30 preempt
```

### Building 1 — Secondary MLS

```text
interface vlan 10
 standby 10 ip 192.168.10.1
 standby 10 priority 100
 standby 10 preempt

interface vlan 20
 standby 20 ip 192.168.20.1
 standby 20 priority 100
 standby 20 preempt

interface vlan 30
 standby 30 ip 192.168.30.1
 standby 30 priority 100
 standby 30 preempt
```

### Building 2 — Primary MLS

```text
interface vlan 40
 standby 40 ip 192.168.40.1
 standby 40 priority 210
 standby 40 preempt

interface vlan 50
 standby 50 ip 192.168.50.1
 standby 50 priority 210
 standby 50 preempt

interface vlan 60
 standby 60 ip 192.168.60.1
 standby 60 priority 210
 standby 60 preempt
```

### Building 2 — Secondary MLS

```text
interface vlan 40
 standby 40 ip 192.168.40.1
 standby 40 priority 100
 standby 40 preempt

interface vlan 50
 standby 50 ip 192.168.50.1
 standby 50 priority 100
 standby 50 preempt

interface vlan 60
 standby 60 ip 192.168.60.1
 standby 60 priority 100
 standby 60 preempt
```

---

## 7. Layer-3 Transit Interfaces

The interfaces connecting the multilayer switches to the routers are configured as routed ports.

```text
no switchport
```

This removes Layer-2 switching functionality from the interface and allows it to operate as a Layer-3 routed interface.

### Building 1 Transit Networks

```text
10.10.1.0/30
10.10.1.4/30
```

Example:

```text
interface gigabitEthernet 0/1
 no switchport
 ip address 10.10.1.2 255.255.255.252
 no shutdown
```

```text
interface gigabitEthernet 0/1
 no switchport
 ip address 10.10.1.6 255.255.255.252
 no shutdown
```

### Building 2 Transit Networks

```text
10.10.2.0/30
10.10.2.4/30
```

Example:

```text
interface gigabitEthernet 0/1
 no switchport
 ip address 10.10.2.2 255.255.255.252
 no shutdown
```

```text
interface gigabitEthernet 0/1
 no switchport
 ip address 10.10.2.6 255.255.255.252
 no shutdown
```

---

## 8. OSPF Dynamic Routing

OSPF is used to dynamically exchange routes between the multilayer switches and routers.

All participating devices use:

```text
router ospf 1
```

and operate in:

```text
area 0
```

### Building 1 — MLS

```text
router ospf 1

network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
network 192.168.30.0 0.0.0.255 area 0
network 10.10.1.0 0.0.0.3 area 0
```

### Second Building 1 MLS

```text
router ospf 1

network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
network 192.168.30.0 0.0.0.255 area 0
network 10.10.1.4 0.0.0.3 area 0
```

### Building 2 — MLS

```text
router ospf 1

network 192.168.40.0 0.0.0.255 area 0
network 192.168.50.0 0.0.0.255 area 0
network 192.168.60.0 0.0.0.255 area 0
network 10.10.2.0 0.0.0.3 area 0
```

### Second Building 2 MLS

```text
router ospf 1

network 192.168.40.0 0.0.0.255 area 0
network 192.168.50.0 0.0.0.255 area 0
network 192.168.60.0 0.0.0.255 area 0
network 10.10.2.4 0.0.0.3 area 0
```

### Passive Interfaces

User VLAN interfaces do not form OSPF neighbor relationships with end devices.

```text
passive-interface vlan 40
passive-interface vlan 50
passive-interface vlan 60
```

The VLAN networks can still be advertised into OSPF while preventing unnecessary OSPF hello packets from being sent toward end-user networks.

---

## 9. Server VLAN

The server is isolated into a dedicated VLAN.

```text
VLAN 100
Network: 192.168.100.0/24
Server: 192.168.100.10
Gateway: 192.168.100.1
```

The server provides:

```text
DHCP
DNS
HTTP
```

---

## 10. DHCP

A centralized Packet Tracer Server provides DHCP services for all department VLANs.

The DHCP pools correspond to:

```text
VLAN 10 → 192.168.10.0/24
VLAN 20 → 192.168.20.0/24
VLAN 30 → 192.168.30.0/24
VLAN 40 → 192.168.40.0/24
VLAN 50 → 192.168.50.0/24
VLAN 60 → 192.168.60.0/24
```

The DHCP server provides clients with:

```text
IP address
Subnet mask
Default gateway
DNS server
```

The HSRP virtual IP is used as the default gateway.

Example:

```text
VLAN 50
Gateway: 192.168.50.1
DNS: 192.168.100.10
```

---

## 11. DHCP Relay

DHCP broadcasts normally do not cross Layer-3 boundaries.

The multilayer switch uses DHCP relay to forward client DHCP requests to the centralized server.

Example:

```text
interface vlan 40
 ip helper-address 192.168.100.10

interface vlan 50
 ip helper-address 192.168.100.10

interface vlan 60
 ip helper-address 192.168.100.10
```

The same principle is applied to the Building 1 client VLANs.

### Traffic Flow

```text
Client
   ↓
DHCP Broadcast
   ↓
SVI
   ↓
ip helper-address
   ↓
192.168.100.10
   ↓
DHCP Server
```

---

## 12. DNS

The centralized server also provides internal DNS.

DNS record:

```text
mycompany.com → 192.168.100.10
```

The server is configured as the DNS server for clients through DHCP.

### Test

```text
ping mycompany.com
```

Expected result:

```text
mycompany.com → 192.168.100.10
```

---

## 13. HTTP Internal Web Server

HTTP is enabled on the centralized server.

The internal company portal is accessed through:

```text
http://mycompany.com
```

The page represents the TwinTower Networks enterprise internal portal.

---

## 14. Connectivity Testing

### Test Default Gateway

```text
ping 192.168.50.1
```

### Test Another VLAN

```text
ping <VLAN-40-PC-IP>
```

### Test Building 1 from Building 2

```text
ping <VLAN-20-PC-IP>
```

### Test Server VLAN

```text
ping 192.168.100.10
```

### Test DNS

```text
ping mycompany.com
```

### Test HTTP

Open:

```text
http://mycompany.com
```

---

## 15. Verification Commands

### VLANs

```text
show vlan brief
```

### Trunks

```text
show interfaces trunk
```

### SVI Status

```text
show ip interface brief
```

### HSRP

```text
show standby brief
```

Detailed HSRP information:

```text
show standby
```

### OSPF Neighbors

```text
show ip ospf neighbor
```

### OSPF Configuration

```text
show ip protocols
```

### Routing Table

```text
show ip route
```

### OSPF Routes

```text
show ip route ospf
```

### Interface Configuration

```text
show running-config interface vlan 40
```

### DHCP Relay Verification

```text
show running-config | include helper
```

### MAC Address Table

```text
show mac address-table
```

---

## 16. End-to-End Validation

The completed network was tested across both buildings.

Validation included:

```text
DHCP address assignment
Inter-VLAN connectivity
Inter-building connectivity
HSRP gateway reachability
OSPF route propagation
Server reachability
DNS name resolution
HTTP access
```

### Example Successful Connectivity Test

```text
ping 192.168.20.101

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

The internal web portal was successfully accessed using:

```text
http://mycompany.com
```

---

## 17. Technologies Used

- Cisco Packet Tracer
- VLANs
- IEEE 802.1Q Trunking
- Layer-3 Switching
- SVIs
- Inter-VLAN Routing
- HSRP
- OSPF
- DHCP
- DHCP Relay
- DNS
- HTTP
- IPv4 Subnetting

---

## 18. Project Architecture

```text
Building 1 — Headquarters
        │
        │ OSPF / Routed Links
        │
Building 2 — Branch Office
        │
        └── Centralized Services in Building 1


Server VLAN 100
        │
        ├── DHCP
        ├── DNS
        └── HTTP
```

The design combines network segmentation, Layer-3 routing, gateway redundancy, dynamic routing, and centralized network services into a two-building enterprise topology.
