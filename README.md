# Enterprise LAN Infrastructure & Inter-VLAN Routing

A production-grade Layer 2 corporate network architecture designed, implemented, and validated using Cisco Packet Tracer. This project simulates a secure, highly-available corporate local area network (LAN) featuring logical department segmentation, link aggregation, and high-speed core routing.

---

## 🗺️ Network Architecture Overview

<a href="https://ibb.co.com/yBNh8nB6"><img src="https://i.ibb.co.com/yBNh8nB6/Screenshot-2026-05-19-143603.png" alt="Enterprise Network Topology" width="100%"></a>

The topology implements a collapsed core-distribution block model alongside access layer switching to deliver optimal throughput, failure isolation, and seamless scalability.

### Core Components & Layer Roles
1. **Core Layer (Core_SW_01):** Acts as the high-speed routing backbone of the enterprise. Utilizing a Layer 3 Multilayer Switch (Cisco 3560/3650), it handles all inter-VLAN data transfers locally via Switch Virtual Interfaces (SVIs), eliminating physical router bottlenecks.
2. **Distribution Layer (Dist_SW_01, Dist_SW_02):** Aggregates connections coming from the access floor and establishes high-speed uplink pathways to the Core Switch.
3. **Access Layer (Access_SW_01 to Access_SW_04):** Connects the end-user workstations directly to the infrastructure and enforces strict network boundary classification.

---

## 🚀 Technical Features Implemented

### 1. VLAN Segmentation (Broadcast Isolation)
To control broadcast domains and enhance data security across departments, the network is segmented into four distinct Virtual Local Area Networks (VLANs):
* **VLAN 10 (Admin):** Dedicated infrastructure for corporate management.
* **VLAN 20 (HR):** Isolated space for sensitive personnel operations.
* **VLAN 30 (Marketing):** Segment for high-bandwidth creative assets and operations.
* **VLAN 40 (Employee):** Main standard workforce operational domain.

### 2. High Availability Link Aggregation (EtherChannel via LACP)
To avoid single points of failure and increase total trunk bandwidth, multi-link logical bundles are configured using **Link Aggregation Control Protocol (LACP - Active Mode)**. 
* **Bundle 5 (Left Side):** Redundant link bundling between `Dist_SW_01` and `Core_SW_01`.
* **Bundle 6 (Right Side):** Redundant link bundling between `Dist_SW_02` and `Core_SW_01`.
* **Access Bundles:** Multi-link active channels connecting access nodes to their respective distribution nodes.

### 3. Layer 3 SVI Inter-VLAN Routing
Traditional "Router-on-a-Stick" pipelines are bypassed in favor of hardware-driven switching fabrics. The Core Switch runs global `ip routing`, hosting virtual default gateways for all subnets, minimizing packet latency while forwarding traffic between segments.

---

## 📊 IP Addressing & Allocation Subnets

| VLAN ID | Department Name | IP Range |
| :--- | :--- | :--- |
| **VLAN 10** | Admin | `192.168.10.0/24` |
| **VLAN 20** | HR | `192.168.20.0/24` |
| **VLAN 30** | Marketing | `192.168.30.0/24` |
| **VLAN 40** | Employee | `192.168.40.0/24` |

---

## 🛠️ Key CLI Configuration Snapshots

### Core Switch (Core_SW_01) Global Routing & SVI
```text
Core_SW_01# configure terminal
Core_SW_01(config)# ip routing

Core_SW_01(config)# interface vlan 10
Core_SW_01(config-if)# ip address 192.168.10.1 255.255.255.0
Core_SW_01(config-if)# no shutdown

Core_SW_01(config)# interface vlan 20
Core_SW_01(config-if)# ip address 192.168.20.1 255.255.255.0
Core_SW_01(config-if)# no shutdown

Core_SW_01(config)# interface vlan 30
Core_SW_01(config-if)# ip address 192.168.30.1 255.255.255.0
Core_SW_01(config-if)# no shutdown

Core_SW_01(config)# interface vlan 40
Core_SW_01(config-if)# ip address 192.168.40.1 255.255.255.0
Core_SW_01(config-if)# no shutdown
