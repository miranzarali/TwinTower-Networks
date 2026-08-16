# TwinTower Networks


> A fully simulated two-building enterprise campus network designed and implemented in Cisco Packet Tracer.


## Project Overview


**TwinTower Networks** is a redundant enterprise network connecting a **Headquarters (Building 1)** and a **Branch Office (Building 2)**.


The project demonstrates enterprise network segmentation, Layer-3 routing, gateway redundancy, dynamic routing, centralized network services, and end-to-end connectivity.


## Network Architecture


```text
                    TWIN TOWER NETWORKS


        ┌──────────── BUILDING 1 ────────────┐
        │           HEADQUARTERS             │
        │                                    │
        │   HR    SALES    FINANCE           │
        │    │      │         │              │
        │   VLAN   VLAN      VLAN             │
        │  10/20/30 + HSRP                    │
        │          │                          │
        │     Layer-3 Switching              │
        │          │                          │
        │    Server VLAN 100                 │
        │    DHCP | DNS | HTTP               │
        └──────────────┬─────────────────────┘
                       │
                  OSPF Area 0
                Routed WAN Links
                       │
        ┌──────────────┴─────────────────────┐
        │           BUILDING 2               │
        │          BRANCH OFFICE             │
        │                                    │
        │   HR    SALES    FINANCE           │
        │    │      │         │              │
        │   VLAN   VLAN      VLAN             │
        │  40/50/60 + HSRP                    │
        │          │                          │
        │     Layer-3 Switching              │
        └────────────────────────────────────┘
Key Features
VLAN segmentation for HR, Sales, and Finance departments
802.1Q trunking between network devices
Layer-3 switching using SVIs
Inter-VLAN routing
HSRP for redundant default gateways
OSPF Area 0 for dynamic inter-building routing
Dedicated Server VLAN 100
Centralized DHCP for department networks
DHCP relay using ip helper-address
Internal DNS with mycompany.com
Internal HTTP web server
End-to-end connectivity testing
Redundant two-building architecture
VLAN & IP Addressing
Building	VLAN	Department	Network	HSRP Gateway
Building 1	10	HR	192.168.10.0/24	192.168.10.1
Building 1	20	Sales	192.168.20.0/24	192.168.20.1
Building 1	30	Finance	192.168.30.0/24	192.168.30.1
Building 2	40	HR	192.168.40.0/24	192.168.40.1
Building 2	50	Sales	192.168.50.0/24	192.168.50.1
Building 2	60	Finance	192.168.60.0/24	192.168.60.1
Both	100	Servers	192.168.100.0/24	192.168.100.1
Central Server
IP Address : 192.168.100.10
Gateway    : 192.168.100.1
Services   : DHCP | DNS | HTTP
DNS Name   : mycompany.com
High Availability

HSRP provides a virtual gateway address for each user VLAN.

The multilayer switches use:

Primary MLS   → .2
Secondary MLS → .3
Virtual IP    → .1

Building 1 uses HSRP priority 110 on the primary MLS, while Building 2 uses priority 210.

preempt is configured so the higher-priority device can resume the active role when appropriate.

Routing

The network uses OSPF Area 0 across routed /30 transit links.

Building 1
    │
    ├── 10.10.1.0/30
    ├── 10.10.1.4/30
    │
    │ OSPF Area 0
    │
    ├── 10.10.2.0/30
    └── 10.10.2.4/30
    │
Building 2
Centralized Services

The Server VLAN provides:

Service	Purpose
DHCP	Automatic client addressing
DNS	Internal name resolution
HTTP	Internal company web portal

The DNS record:

mycompany.com → 192.168.100.10

allows clients to access the internal web server using:

http://mycompany.com
Verification

The implementation was tested for:

DHCP address assignment
Inter-VLAN connectivity
Inter-building connectivity
HSRP gateway reachability
OSPF route propagation
Server reachability
DNS name resolution
HTTP access

Example connectivity result:

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
Evidence
Network Topology

Internal Web Portal

Connectivity Test

Project Files
File	Description
TwinTower-Networks.pkt	Complete Cisco Packet Tracer project
COMMANDS.md	Configuration and verification commands
screenshots/	Implementation and testing evidence
Technologies

Cisco Packet Tracer · VLAN · 802.1Q · SVI · HSRP · OSPF · DHCP · DHCP Relay · DNS · HTTP · IPv4 Subnetting

Project Goal

To design and implement a realistic, redundant enterprise campus network that integrates network segmentation, high availability, dynamic routing, centralized services, and end-to-end validation within a two-building architecture.
