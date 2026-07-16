# Core2

## Table of Contents

- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
- [Routing](#routing)
  - [Router ISIS](#router-isis)
  - [Router BGP](#router-bgp)
- [MPLS](#mpls)
  - [MPLS and LDP](#mpls-and-ldp)
  - [MPLS Device Configuration](#mpls-device-configuration)

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF | MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | --- | --- | -------- | ------ | ------- |
| Ethernet1 | P2P_Core1_Ethernet1 | - | 192.168.98.1/31 | default | 1500 | - | - | - |
| Ethernet11 | P2P_dc1-leaf1a_Ethernet11 | - | 192.168.98.4/31 | default | 1500 | - | - | - |
| Ethernet12 | P2P_dc1-leaf1b_Ethernet12 | - | 192.168.98.8/31 | default | 1500 | - | - | - |
| Ethernet13 | P2P_dc2-leaf2a_Ethernet13 | - | 192.168.98.12/31 | default | 1500 | - | - | - |
| Ethernet14 | P2P_dc2-leaf2b_Ethernet14 | - | 192.168.98.16/31 | default | 1500 | - | - | - |

##### ISIS

| Interface | Channel Group | ISIS Instance | ISIS BFD | ISIS Metric | Mode | ISIS Circuit Type | Hello Padding | ISIS Authentication Mode |
| --------- | ------------- | ------------- | -------- | ----------- | ---- | ----------------- | ------------- | ------------------------ |
| Ethernet1 | - | CORE | - | - | point-to-point | - | - | - |
| Ethernet11 | - | CORE | - | - | point-to-point | - | - | - |
| Ethernet12 | - | CORE | - | - | point-to-point | - | - | - |
| Ethernet13 | - | CORE | - | - | point-to-point | - | - | - |
| Ethernet14 | - | CORE | - | - | point-to-point | - | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_Core1_Ethernet1
   mtu 1500
   ip address 192.168.98.1/31
   isis enable CORE
   isis network point-to-point
!
interface Ethernet11
   description P2P_dc1-leaf1a_Ethernet11
   mtu 1500
   ip address 192.168.98.4/31
   isis enable CORE
   isis network point-to-point
!
interface Ethernet12
   description P2P_dc1-leaf1b_Ethernet12
   mtu 1500
   ip address 192.168.98.8/31
   isis enable CORE
   isis network point-to-point
!
interface Ethernet13
   description P2P_dc2-leaf2a_Ethernet13
   mtu 1500
   ip address 192.168.98.12/31
   isis enable CORE
   isis network point-to-point
!
interface Ethernet14
   description P2P_dc2-leaf2b_Ethernet14
   mtu 1500
   ip address 192.168.98.16/31
   isis enable CORE
   isis network point-to-point
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | SR-ISIS Router ID | default | 192.168.99.2/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Addresses |
| --------- | ----------- | --- | -------------- |
| Loopback0 | SR-ISIS Router ID | default | - |

##### ISIS

| Interface | ISIS instance | ISIS metric | Interface mode |
| --------- | ------------- | ----------- | -------------- |
| Loopback0 | CORE | - | passive |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description SR-ISIS Router ID
   ip address 192.168.99.2/32
   isis enable CORE
   isis passive
```

## Routing

### Router ISIS

#### Router ISIS Summary

| Settings | Value |
| -------- | ----- |
| Instance | CORE |
| Net-ID | 49.0001.0000.0000.0002.00 |
| Type | level-2 |
| Log Adjacency Changes | True |
| SR MPLS Enabled | True |

#### ISIS Interfaces Summary

| Interface | ISIS Instance | ISIS Metric | Interface Mode |
| --------- | ------------- | ----------- | -------------- |
| Ethernet1 | CORE | - | point-to-point |
| Ethernet11 | CORE | - | point-to-point |
| Ethernet12 | CORE | - | point-to-point |
| Ethernet13 | CORE | - | point-to-point |
| Ethernet14 | CORE | - | point-to-point |
| Loopback0 | CORE | - | passive |

#### Prefix Segments

| Prefix Segment | Index |
| -------------- | ----- |
| 192.168.99.2/32 | 2 |

#### ISIS IPv4 Address Family Summary

| Settings | Value |
| -------- | ----- |
| IPv4 Address-family Enabled | True |

#### Router ISIS Device Configuration

```eos
!
router isis CORE
   net 49.0001.0000.0000.0002.00
   is-type level-2
   log-adjacency-changes
   !
   address-family ipv4 unicast
   !
   segment-routing mpls
      no shutdown
      prefix-segment 192.168.99.2/32 index 2
```

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65298 | 192.168.99.2 |

#### Router BGP Peer Groups

##### EVPN-CORE

| Settings | Value |
| -------- | ----- |
| Next-hop self | True |
| Source | Loopback0 |
| Send community | extended |

##### EVPN-LEAFS

| Settings | Value |
| -------- | ----- |
| Route Reflector Client | Yes |
| Source | Loopback0 |
| Ebgp multihop | 5 |
| Send community | extended |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 192.168.99.1 | 65298 | default | - | Inherited from peer group EVPN-CORE | - | - | - | - | - | - | - |
| 192.168.99.11 | 65100 | default | - | Inherited from peer group EVPN-LEAFS | - | - | - | - | Inherited from peer group EVPN-LEAFS | - | - |
| 192.168.99.12 | 65100 | default | - | Inherited from peer group EVPN-LEAFS | - | - | - | - | Inherited from peer group EVPN-LEAFS | - | - |
| 192.168.99.23 | 65299 | default | - | Inherited from peer group EVPN-LEAFS | - | - | - | - | Inherited from peer group EVPN-LEAFS | - | - |
| 192.168.99.24 | 65299 | default | - | Inherited from peer group EVPN-LEAFS | - | - | - | - | Inherited from peer group EVPN-LEAFS | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Route-map In | Route-map Out | Peer-tag In | Peer-tag Out | Encapsulation | Next-hop-self Source Interface |
| ---------- | -------- | ------------ | ------------- | ----------- | ------------ | ------------- | ------------------------------ |
| EVPN-CORE | True | - | - | - | - | default | - |
| EVPN-LEAFS | True | - | - | - | - | default | - |

#### Router BGP Device Configuration

```eos
!
router bgp 65298
   router-id 192.168.99.2
   neighbor EVPN-CORE peer group
   neighbor EVPN-CORE next-hop-self
   neighbor EVPN-CORE update-source Loopback0
   neighbor EVPN-CORE send-community extended
   neighbor EVPN-LEAFS peer group
   neighbor EVPN-LEAFS update-source Loopback0
   neighbor EVPN-LEAFS ebgp-multihop 5
   neighbor EVPN-LEAFS route-reflector-client
   neighbor EVPN-LEAFS send-community extended
   neighbor 192.168.99.1 peer group EVPN-CORE
   neighbor 192.168.99.1 remote-as 65298
   neighbor 192.168.99.11 peer group EVPN-LEAFS
   neighbor 192.168.99.11 remote-as 65100
   neighbor 192.168.99.12 peer group EVPN-LEAFS
   neighbor 192.168.99.12 remote-as 65100
   neighbor 192.168.99.23 peer group EVPN-LEAFS
   neighbor 192.168.99.23 remote-as 65299
   neighbor 192.168.99.24 peer group EVPN-LEAFS
   neighbor 192.168.99.24 remote-as 65299
   !
   address-family evpn
      neighbor EVPN-CORE activate
      neighbor EVPN-LEAFS activate
```

## MPLS

### MPLS and LDP

#### MPLS and LDP Summary

| Setting | Value |
| -------- | ---- |
| MPLS IP Enabled | True |
| LDP Enabled | False |
| LDP Router ID | - |
| LDP Interface Disabled Default | - |
| LDP Transport-Address Interface | - |

### MPLS Device Configuration

```eos
!
mpls ip
!
mpls ldp
   shutdown
```
