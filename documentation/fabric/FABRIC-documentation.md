# FABRIC

## Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [ISIS CLNS interfaces](#isis-clns-interfaces)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| FABRIC | l3leaf | dc1-leaf1a | 192.168.0.13/24 | cEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc1-leaf1b | 192.168.0.14/24 | cEOS-lab | Provisioned | - |
| FABRIC | spine | dc1-spine1 | 192.168.0.11/24 | cEOS-lab | Provisioned | - |
| FABRIC | l3leaf | dc2-leaf2a | 192.168.0.15/24 | cEOS | Provisioned | - |
| FABRIC | l3leaf | dc2-leaf2b | 192.168.0.16/24 | cEOS | Provisioned | - |
| FABRIC | spine | dc2-spine1 | 192.168.0.12/24 | cEOS | Provisioned | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | --------- | -------------- |
| l3leaf | dc1-leaf1a | Ethernet1 | spine | dc1-spine1 | Ethernet1 |
| l3leaf | dc1-leaf1a | Ethernet3 | mlag_peer | dc1-leaf1b | Ethernet3 |
| l3leaf | dc1-leaf1a | Ethernet4 | mlag_peer | dc1-leaf1b | Ethernet4 |
| l3leaf | dc1-leaf1b | Ethernet1 | spine | dc1-spine1 | Ethernet2 |
| l3leaf | dc2-leaf2a | Ethernet1 | spine | dc2-spine1 | Ethernet1 |
| l3leaf | dc2-leaf2a | Ethernet3 | mlag_peer | dc2-leaf2b | Ethernet3 |
| l3leaf | dc2-leaf2a | Ethernet4 | mlag_peer | dc2-leaf2b | Ethernet4 |
| l3leaf | dc2-leaf2b | Ethernet1 | spine | dc2-spine1 | Ethernet2 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 192.168.103.0/24 | 256 | 4 | 1.57 % |
| 192.168.203.0/24 | 256 | 4 | 1.57 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| dc1-leaf1a | Ethernet1 | 192.168.103.1/31 | dc1-spine1 | Ethernet1 | 192.168.103.0/31 |
| dc1-leaf1b | Ethernet1 | 192.168.103.3/31 | dc1-spine1 | Ethernet2 | 192.168.103.2/31 |
| dc2-leaf2a | Ethernet1 | 192.168.203.5/31 | dc2-spine1 | Ethernet1 | 192.168.203.4/31 |
| dc2-leaf2b | Ethernet1 | 192.168.203.7/31 | dc2-spine1 | Ethernet2 | 192.168.203.6/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 192.168.101.0/24 | 256 | 3 | 1.18 % |
| 192.168.201.0/24 | 256 | 3 | 1.18 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| FABRIC | dc1-leaf1a | 192.168.101.1/32 |
| FABRIC | dc1-leaf1b | 192.168.101.2/32 |
| FABRIC | dc1-spine1 | 192.168.101.11/32 |
| FABRIC | dc2-leaf2a | 192.168.201.3/32 |
| FABRIC | dc2-leaf2b | 192.168.201.4/32 |
| FABRIC | dc2-spine1 | 192.168.201.13/32 |

### ISIS CLNS interfaces

| POD | Node | CLNS Address |
| --- | ---- | ------------ |
| FABRIC | dc1-leaf1a | 49.0001.0000.0000.0011.00 |
| FABRIC | dc1-leaf1b | 49.0001.0000.0000.0012.00 |
| FABRIC | dc2-leaf2a | 49.0001.0000.0000.0023.00 |
| FABRIC | dc2-leaf2b | 49.0001.0000.0000.0024.00 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------------ | ------------------- | ------------------ | ------------------ |
| 192.168.102.0/24 | 256 | 2 | 0.79 % |
| 192.168.202.0/24 | 256 | 2 | 0.79 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| FABRIC | dc1-leaf1a | 192.168.102.1/32 |
| FABRIC | dc1-leaf1b | 192.168.102.1/32 |
| FABRIC | dc2-leaf2a | 192.168.202.3/32 |
| FABRIC | dc2-leaf2b | 192.168.202.3/32 |
