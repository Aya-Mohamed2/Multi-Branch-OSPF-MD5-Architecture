# Multi-Branch Enterprise Network Architecture (OSPF & MD5 Security)

## Project Overview
This project demonstrates the design and deployment of a multi-branch enterprise network topology. It implements dynamic interior routing via OSPF (Open Shortest Path First) across disparate subnets and hardens the routing protocol exchanges using cryptographic MD5 Authentication to prevent unauthorized routing updates and route poisoning.

---

## Network Architecture & Topology
The topology consists of two distinct branch environments connected across a WAN serial link, dynamically exchanging routing updates through a single OSPF Area 0 backbone.

![OSPF Network Topology](01_ospf_topology.png)

### Addressing & Routing Schema:
| Network Segment | Device / Interface | Subnet / IP Address | Routing Protocol | Security / Authentication |
| :--- | :--- | :--- | :--- | :--- |
| HQ LAN (Subnet A) | HQ-Router (Gi0/0) | 192.168.1.0/24 | OSPF Area 0 | None (Internal LAN) |
| WAN Serial Link | Inter-Router (S0/3/0) | 10.0.0.0/30 | OSPF Area 0 | MD5 Authentication (Cisco@123) |
| Branch LAN (Subnet B) | Branch-Router (Gi0/0) | 192.168.2.0/24 | OSPF Area 0 | None (Internal LAN) |

---

## Core Technical Implementations

### 1. Dynamic Routing Configuration (OSPF)
- Configured OSPF Process 1 with explicit Router-IDs (1.1.1.1 for HQ, 2.2.2.2 for Branch) to ensure stable router identification.
- Advertised both local LAN interfaces and the point-to-point WAN serial link into Area 0 (Backbone).

### 2. OSPF MD5 Authentication Hardening
- Applied message-digest authentication on the serial interfaces connecting the two routers.
- Configured a shared cryptographic key (Key ID 1) utilizing MD5 hashing to ensure that only trusted routers exchanging valid keys can form adjacencies and inject routing table updates.

---

## Verification & Testing

### 1. OSPF Neighbor Adjacency:
Verification on HQ-Router confirming neighbor 2.2.2.2 reaches the FULL state over MD5-authenticated link:

![OSPF Neighbor Verification](02_ospf_neighbor_verification.png)

### 2. Routing Table Inspection:
Verifying the routing table on HQ-Router showing routes learned via OSPF (marked with the O flag):

![OSPF Routing Table](03_routing_table_ospf.png)

### 3. End-to-End Ping Verification:
Successful ICMP reachability test from HQ workstation (PC0) to Branch workstation (PC2):

![End-to-End Ping Test](04_ping_test_hq_to_branch.png)

---

## Project Lab File
Download and inspect the complete Cisco Packet Tracer simulation topology:

[Download MultiBranch_OSPF_MD5_Architecture.pkt](./MultiBranch_OSPF_MD5_Architecture.pkt)
