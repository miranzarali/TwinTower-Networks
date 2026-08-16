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
