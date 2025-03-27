# 🌐 BGP At Doors of Autonomous Systems is Simple (BADASS) 🚀

## 📝 Project Overview

This project implements a **VXLAN network with BGP EVPN** using **GNS3 and Docker**. It explores:
- **VXLAN (RFC 7348)**: Virtualized Layer 2 networks over Layer 3.
- **BGP EVPN (RFC 7432)**: Ethernet VPN using BGP for MAC address distribution.
- **OSPF and BGP**: Routing protocols used for dynamic network discovery.

The project is divided into **three parts**:
1. **GNS3 Configuration with Docker** 🛠️ 
2. **VXLAN Implementation** 🌐 
3. **BGP EVPN Integration** 🔀 


### 🔧 Key Technologies
- 🐳 Docker
- 🌐 GNS3
- 🌈 VXLAN
- 🔗 BGP EVPN
- 🌳 OSPF
- 🔄 Route Reflection


## 🏗️ **Installation & Setup**
Ensure you have the following installed on your virtual machine:
- **GNS3**
- **Docker**
- **FRRouting (FRR)**
- **BusyBox** (for lightweight network hosts)


### 🔹 Part 1: Basic Docker Network
```
[host_wil-1] --- [routeur_wil]
```

- Minimal setup with a host and router
- Purpose: Establish basic network connectivity
- Devices:
  - 1x Host
  - 1x Router



### 🔹 Part 2: VXLAN Network
```
        [Switch_wil]
       /             \
[routeur_wil-1]   [routeur_wil-2]
       \             /
    [host_wil-1]  [host_wil-2]
```
- VXLAN Configuration Details:
  - VXLAN ID: 10
  - Multicast Group: 239.1.1.1
  - Destination Port: 4789
- Key Components:
  - 2x Routers
  - 1x Switch
  - 2x Hosts
- Network Type: Layer 2 Overlay


- VXLAN with Unicast (Static Peering)
```
ip addr add dev eth0 10.0.0.1/30
ip link add vxlan10 type vxlan id 10 dev eth0 local 10.0.0.1 remote 10.0.0.2 dstport 4789
ip link add br0 type bridge
ip link set eth1 master br0
ip link set vxlan10 master br0
ip link set br0 up
ip link set vxlan10 up
```
- VXLAN with Multicast (Dynamic Peering)
```
ip link add vxlan10 type vxlan id 10 dev eth0 group 239.1.1.1 dstport 4789
```
Unicast VXLAN is used for point-to-point tunnels.

Multicast VXLAN allows multiple routers to join dynamically.



### 🔹 Part 3: BGP EVPN Datacenter
```
         [_wil-1 (RR)]
        /   |   \
 [_wil-2] [_wil-3] [_wil-4]
    |       |       |
[host_1] [host_2] [host_3]
```
- Advanced Routing Scenario
- Routing Protocol: OSPF + BGP
- Route Reflection Topology
- Key Features:
  - Autonomous System: 1
  - Loopback Interfaces
  - EVPN Address Family
- Devices:
  - 1x Route Reflector
  - 3x Leaf Routers
  - 3x Hosts

## 🛠️ Configuration Highlights

### Route Reflector
```
router bgp 1
 neighbor ibgp peer-group
 neighbor ibgp remote-as 1
 neighbor ibgp update-source lo
 bgp listen range 1.1.1.0/29 peer-group ibgp
 address-family l2vpn evpn
  neighbor ibgp activate
  neighbor ibgp route-reflector-client
 exit-address-family

```

### Router VTEP Configuration (VXLAN Tunnel End Point)
```
router bgp 1
 neighbor 1.1.1.1 remote-as 1
 neighbor 1.1.1.1 update-source lo
 address-family l2vpn evpn
  neighbor 1.1.1.1 activate
  advertise-all-vni
 exit-address-family

```

## 🎓 Learning Objectives

- 🌐 Network virtualization techniques
- 🔗 VXLAN and BGP EVPN principles
- 🚦 Advanced routing protocols
- 🐳 Docker and GNS3 integration
- 🛠️ Datacenter network design

## 🛠️ Environment Setup

1. Prerequisites
   - Virtual Machine
   - Docker
   - GNS3
   - FRRouting

## 📚 Recommended Resources

- [RFC 4271 - BGP](https://tools.ietf.org/html/rfc4271)
- [RFC 7348 - VXLAN](https://tools.ietf.org/html/rfc7348)
- [RFC 7432 - BGP EVPN](https://tools.ietf.org/html/rfc7432)
- [Autonomous System (AS)](https://www.cloudflare.com/learning/network-layer/what-is-an-autonomous-system/)
- [Border Gateway Protocol (BGP)](https://www.cloudflare.com/learning/security/glossary/what-is-bgp/)
- [Route Reflector (RR)](https://www.youtube.com/watch?v=j8t2uHhl5ZA&t=3s)

---

🌟 **Networking Journey Begins!** 🌟
