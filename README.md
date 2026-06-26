# automated-vlan-dhcp-network
An enterprise-grade network design featuring multi-VLAN segmentation, Layer 3 Inter-VLAN routing, and centralized dynamic DHCP configuration using Cisco Packet Tracer.
# Automated Multi-VLAN Corporate Network with Layer 3 Inter-VLAN Routing & DHCP

A Cisco Packet Tracer laboratory demonstrating a scalable enterprise network built around a **Cisco Catalyst 3650 Multilayer Switch**. The project showcases VLAN segmentation, centralized DHCP services, Layer 3 Inter-VLAN Routing, and IEEE 802.1Q trunking between the core and access switches.

---

# 📌 Project Overview

This lab simulates a small enterprise network divided into three departments:

- 🟢 Management (VLAN 10)
- 🔴 Finance (VLAN 20)
- 🟡 IT (VLAN 30)

Instead of using an external router, a **Layer 3 Multilayer Switch** performs both routing and DHCP services, allowing hosts in different VLANs to communicate while automatically obtaining IP configuration.

---

# 🎯 Learning Objectives

By completing this lab, you will learn how to:

- Create and manage VLANs
- Configure IEEE 802.1Q trunk links
- Configure Switched Virtual Interfaces (SVIs)
- Enable Layer 3 Inter-VLAN Routing
- Configure DHCP pools on a Layer 3 switch
- Verify VLANs, routing, and DHCP operation
- Troubleshoot Layer 2 and Layer 3 connectivity

---

# 🏗️ Network Topology

![Network Topology](topology.png)

---

# 🌐 Network Architecture

The network consists of one Cisco Catalyst 3650 Multilayer Switch connected to three Cisco 2960 access switches.

Each access switch serves one department and connects to the core switch through an IEEE 802.1Q trunk link.

The multilayer switch performs:

- Layer 2 Switching
- Layer 3 Routing
- Default Gateway functionality
- DHCP Server functionality

---

# 📋 Network Addressing

| Department | VLAN | Network | Gateway |
|------------|------|----------------|----------------|
| Management | VLAN 10 | 192.168.10.0/24 | 192.168.10.1 |
| Finance | VLAN 20 | 192.168.20.0/24 | 192.168.20.1 |
| IT | VLAN 30 | 192.168.30.0/24 | 192.168.30.1 |

---

# 🖥️ Devices

| Device | Purpose |
|---------|---------|
| Catalyst 3650 | Core Multilayer Switch |
| Switch0 | Management Access Switch |
| Switch1 | Finance Access Switch |
| Switch2 | IT Access Switch |
| 6 PCs | DHCP Clients |
| 3 Servers | Department Servers |

---

# 🛠️ Core Switch Configuration

The Catalyst 3650 performs four major functions:

- VLAN Database
- DHCP Server
- Inter-VLAN Routing
- Trunk Aggregation

Configuration includes:

- VLAN Creation
- SVIs
- DHCP Pools
- IP Routing
- Trunk Ports

---

# 🛠️ Access Switch Configuration

Each access switch is configured to:

- Create VLANs
- Assign end devices to Access Ports
- Configure a single Trunk uplink to the Core Switch

Example:

```ios
interface range fa0/2-24
 switchport mode access
 switchport access vlan 10

interface fa0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
```

The same configuration pattern is used for Finance and IT switches by changing the assigned VLAN.

---

# 🔄 Traffic Flow Analysis

When a host in **Management (VLAN 10)** communicates with a host in **Finance (VLAN 20)**, the traffic follows this sequence:

1. The source PC determines that the destination belongs to another subnet.

2. The frame is sent to the default gateway (SVI VLAN 10).

3. The access switch forwards the tagged frame over the trunk link.

4. The Catalyst 3650 receives the frame on the trunk port.

5. The Layer 3 engine routes the packet internally between VLAN interfaces.

6. A new Layer 2 frame is created for VLAN 20.

7. The frame leaves through the trunk toward the Finance switch.

8. The access switch removes the VLAN tag.

9. The destination PC receives a standard Ethernet frame.

---

# 🌐 DHCP Operation

The Core Switch also functions as a DHCP server.

Each VLAN has its own DHCP pool:

| Pool | Network |
|-------|----------------|
| VLAN10_Pool | 192.168.10.0/24 |
| VLAN20_Pool | 192.168.20.0/24 |
| VLAN30_Pool | 192.168.30.0/24 |

When a client boots:

1. DHCP Discover
2. DHCP Offer
3. DHCP Request
4. DHCP ACK

After the process completes, the PC automatically receives:

- IP Address
- Subnet Mask
- Default Gateway
- DNS Server

---

# 🏷️ Understanding Tagged Traffic

Hosts never generate VLAN tags.

When traffic enters an Access Port:

- The switch associates it with the configured VLAN.

When traffic leaves through a Trunk Port:

- The switch inserts an IEEE 802.1Q VLAN tag.

The receiving multilayer switch reads this tag and forwards the traffic to the appropriate VLAN interface before routing.

---

# 🔍 Verification Commands

## Verify VLAN Database

```ios
show vlan brief
```

---

## Verify Trunk Links

```ios
show interfaces trunk
```

---

## Verify DHCP Bindings

```ios
show ip dhcp binding
```

---

## Verify Routing Table

```ios
show ip route
```

---

## Verify SVIs

```ios
show ip interface brief
```

---

# 🧪 Connectivity Tests

Example:

```bash
C:\> ping 192.168.20.11
```

Expected output:

```text
Request timed out.

Reply from 192.168.20.11: bytes=32 time<1ms TTL=127
Reply from 192.168.20.11: bytes=32 time<1ms TTL=127
Reply from 192.168.20.11: bytes=32 time<1ms TTL=127
```

The initial timeout occurs because the source device must first resolve the destination MAC address using ARP.

Once the ARP cache is populated, all subsequent packets are delivered successfully.

---

# 📸 Screenshots

This repository includes screenshots demonstrating:

- Network Topology
- VLAN Database
- Trunk Interfaces
- DHCP Bindings
- Routing Table
- Successful Ping Test
- Packet Tracer Simulation Mode

---

# ⚠️ Design Considerations

Using a Layer 3 switch provides several advantages over Router-on-a-Stick:

- Hardware-based routing
- Lower latency
- Higher throughput
- Better scalability
- Reduced router CPU utilization
- Simplified enterprise design

However, large enterprise environments commonly introduce additional technologies such as:

- Rapid Spanning Tree Protocol (RSTP)
- EtherChannel
- HSRP/VRRP
- Dynamic Routing Protocols
- ACLs
- Redundant Core Switches

to improve redundancy, availability, and scalability.

---

# 📂 Repository Structure

```text
Layer3-InterVLAN-DHCP-Lab/
│
├── README.md
├── Layer3-InterVLAN-DHCP.pkt
├── topology.png
└── screenshots/
    ├── topology.png
    ├── show-vlan-brief.png
    ├── show-interfaces-trunk.png
    ├── show-ip-route.png
    ├── show-ip-dhcp-binding.png
    ├── ping-success.png
    └── simulation-mode.png
```

---

# 🚀 Technologies Used

- Cisco Packet Tracer
- Cisco Catalyst 3650 Multilayer Switch
- Cisco Catalyst 2960 Switches
- VLANs
- IEEE 802.1Q
- Layer 3 Switching
- Switched Virtual Interfaces (SVIs)
- DHCP
- ICMP
- ARP

---

# 👩‍💻 About This Project

This project was developed as part of my CCNA learning journey and networking portfolio.

It demonstrates practical experience with enterprise VLAN segmentation, IEEE 802.1Q trunking, Layer 3 switching, centralized DHCP services, Switched Virtual Interfaces (SVIs), and Inter-VLAN Routing using Cisco Catalyst Multilayer Switches.

It also serves as a foundation for more advanced enterprise networking topics such as High Availability, Redundancy, Dynamic Routing, and Campus Network Design.
