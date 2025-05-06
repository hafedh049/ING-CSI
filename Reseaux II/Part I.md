## 📊 Network Topology Overview

![[Pasted image 20250427054935.png]]
## 📋IP Addressing Table

| **Router** | **Interface** | **IP Address** | **Subnet Mask** | **Description**   |
| ---------- | ------------- | -------------- | --------------- | ----------------- |
| **CE11**   | Loopback0     | 172.16.11.11   | 255.255.255.255 | Customer 1 Site 1 |
|            | Fa1/0         | 192.168.1.2    | 255.255.255.252 | Connect to PE1    |
| **CE12**   | Loopback0     | 172.16.12.12   | 255.255.255.255 | Customer 1 Site 2 |
|            | Fa1/0         | 192.168.1.10   | 255.255.255.252 | Connect to PE2    |
| **CE21**   | Loopback0     | 172.16.21.21   | 255.255.255.255 | Customer 2 Site 1 |
|            | Fa1/0         | 192.168.1.6    | 255.255.255.252 | Connect to PE1    |
| **CE22**   | Loopback0     | 172.16.22.22   | 255.255.255.255 | Customer 2 Site 2 |
|            | Fa1/0         | 192.168.1.14   | 255.255.255.252 | Connect to PE2    |
| **PE1**    | Loopback0     | 1.1.1.1        | 255.255.255.255 | Provider Edge 1   |
|            | Fa1/0         | 10.1.1.1       | 255.255.255.252 | Connect to P1     |
|            | Fa2/0         | 10.1.1.5       | 255.255.255.252 | Connect to P2     |
|            | Fa3/0         | 192.168.1.1    | 255.255.255.252 | Connect to CE11   |
|            | Fa4/0         | 192.168.1.5    | 255.255.255.252 | Connect to CE21   |
| **PE2**    | Loopback0     | 2.2.2.2        | 255.255.255.255 | Provider Edge 2   |
|            | Fa1/0         | 10.1.1.9       | 255.255.255.252 | Connect to P2     |
|            | Fa2/0         | 10.1.1.13      | 255.255.255.252 | Connect to P1     |
|            | Fa3/0         | 192.168.1.9    | 255.255.255.252 | Connect to CE12   |
|            | Fa4/0         | 192.168.1.13   | 255.255.255.252 | Connect to CE22   |
| **P1**     | Loopback0     | 3.3.3.3        | 255.255.255.255 | Provider 1        |
|            | Fa1/0         | 10.1.1.21      | 255.255.255.252 | Connect to P2     |
|            | Fa2/0         | 10.1.1.2       | 255.255.255.252 | Connect to PE1    |
|            | Fa3/0         | 10.1.1.14      | 255.255.255.252 | Connect to PE2    |
| **P2**     | Loopback0     | 4.4.4.4        | 255.255.255.255 | Provider 2        |
|            | Fa1/0         | 10.1.1.22      | 255.255.255.252 | Connect to P1     |
|            | Fa2/0         | 10.1.1.10      | 255.255.255.252 | Connect to PE2    |
|            | Fa3/0         | 10.1.1.6       | 255.255.255.252 | Connect to PE1    |
## 🔄 Network Subnets

| **Subnet** | **Network**     | **Description**        |
| ---------- | --------------- | ---------------------- |
| PE1 – P1   | 10.1.1.0/30     | Backbone Link          |
| PE1 – P2   | 10.1.1.4/30     | Backbone Link          |
| PE2 – P2   | 10.1.1.8/30     | Backbone Link          |
| PE2 – P1   | 10.1.1.12/30    | Backbone Link          |
| P1 – P2    | 10.1.1.20/30    | Backbone Link          |
| CE11 – PE1 | 192.168.1.0/30  | Customer 1 Site 1 Link |
| CE21 – PE1 | 192.168.1.4/30  | Customer 2 Site 1 Link |
| CE12 – PE2 | 192.168.1.8/30  | Customer 1 Site 2 Link |
| CE22 – PE2 | 192.168.1.12/30 | Customer 2 Site 2 Link |
## 🖥️ Router Configurations

### CE11 Configuration

```bash
en
conf t
hostname CE11
!
ip cef
!
interface Loopback0
 ip address 172.16.11.11 255.255.255.255
!
interface FastEthernet1/0
 description Connection to PE1
 ip address 192.168.1.2 255.255.255.252
 no shutdown
!
router ospf 1
 network 172.16.11.11 0.0.0.0 area 11
 network 192.168.1.0 0.0.0.3 area 11
!
end
write memory
```

### CE12 Configuration

```bash
en
conf t
hostname CE12
!
ip cef
!
interface Loopback0
 ip address 172.16.12.12 255.255.255.255
!
interface FastEthernet1/0
 description Connection to PE2
 ip address 192.168.1.10 255.255.255.252
 no shutdown
!
router ospf 1
 network 172.16.12.12 0.0.0.0 area 12
 network 192.168.1.8 0.0.0.3 area 12
!
end
write memory
```

### CE21 Configuration

```bash
en
conf t
hostname CE21
!
ip cef
!
interface Loopback0
 ip address 172.16.21.21 255.255.255.255
!
interface FastEthernet1/0
 description Connection to PE1
 ip address 192.168.1.6 255.255.255.252
 no shutdown
!
router ospf 1
 network 172.16.21.21 0.0.0.0 area 21
 network 192.168.1.4 0.0.0.3 area 21
!
end
write memory
```

### CE22 Configuration

```bash
en
conf t
hostname CE22
!
ip cef
!
interface Loopback0
 ip address 172.16.22.22 255.255.255.255
!
interface FastEthernet1/0
 description Connection to PE2
 ip address 192.168.1.14 255.255.255.252
 no shutdown
!
router ospf 1
 network 172.16.22.22 0.0.0.0 area 22
 network 192.168.1.12 0.0.0.3 area 22
!
end
write memory
```

### PE1 Configuration

```bash
en
conf t
hostname PE1
!
ip cef
!
ip vrf VPN_Customer1
 rd 100:1
 route-target export 100:1
 route-target import 100:1
!
ip vrf VPN_Customer2
 rd 100:2
 route-target export 100:2
 route-target import 100:2
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
!
interface FastEthernet1/0
 description Connection to P1
 ip address 10.1.1.1 255.255.255.252
 mpls ip
 no shutdown
!
interface FastEthernet2/0
 description Connection to P2
 ip address 10.1.1.5 255.255.255.252
 mpls ip
 no shutdown
!
interface FastEthernet3/0
 description Connection to CE11
 ip vrf forwarding VPN_Customer1
 ip address 192.168.1.1 255.255.255.252
 no shutdown
!
interface FastEthernet4/0
 description Connection to CE21
 ip vrf forwarding VPN_Customer2
 ip address 192.168.1.5 255.255.255.252
 no shutdown
!
router ospf 1
 network 1.1.1.1 0.0.0.0 area 0
 network 10.1.1.0 0.0.0.3 area 0
 network 10.1.1.4 0.0.0.3 area 0
!
router ospf 100 vrf VPN_Customer1
 redistribute bgp 100 subnets
 network 192.168.1.0 0.0.0.3 area 11
!
router ospf 200 vrf VPN_Customer2
 redistribute bgp 100 subnets
 network 192.168.1.4 0.0.0.3 area 21
!
router bgp 100
 no bgp default ipv4-unicast
 neighbor 2.2.2.2 remote-as 100
 neighbor 2.2.2.2 update-source Loopback0
 !
 address-family vpnv4
  neighbor 2.2.2.2 activate
  neighbor 2.2.2.2 send-community both
 exit-address-family
 !
 address-family ipv4 vrf VPN_Customer1
  redistribute ospf 100 vrf VPN_Customer1
 exit-address-family
 !
 address-family ipv4 vrf VPN_Customer2
  redistribute ospf 200 vrf VPN_Customer2
 exit-address-family
!
mpls ip
mpls label protocol ldp
mpls ldp router-id Loopback0 force
!
end
write memory
```

### PE2 Configuration

```bash
en
conf t
hostname PE2
!
ip cef
!
ip vrf VPN_Customer1
 rd 100:1
 route-target export 100:1
 route-target import 100:1
!
ip vrf VPN_Customer2
 rd 100:2
 route-target export 100:2
 route-target import 100:2
!
interface Loopback0
 ip address 2.2.2.2 255.255.255.255
!
interface FastEthernet1/0
 description Connection to P2
 ip address 10.1.1.9 255.255.255.252
 mpls ip
 no shutdown
!
interface FastEthernet2/0
 description Connection to P1
 ip address 10.1.1.13 255.255.255.252
 mpls ip
 no shutdown
!
interface FastEthernet3/0
 description Connection to CE12
 ip vrf forwarding VPN_Customer1
 ip address 192.168.1.9 255.255.255.252
 no shutdown
!
interface FastEthernet4/0
 description Connection to CE22
 ip vrf forwarding VPN_Customer2
 ip address 192.168.1.13 255.255.255.252
 no shutdown
!
router ospf 1
 network 2.2.2.2 0.0.0.0 area 0
 network 10.1.1.8 0.0.0.3 area 0
 network 10.1.1.12 0.0.0.3 area 0
!
router ospf 100 vrf VPN_Customer1
 redistribute bgp 100 subnets
 network 192.168.1.8 0.0.0.3 area 12
!
router ospf 200 vrf VPN_Customer2
 redistribute bgp 100 subnets
 network 192.168.1.12 0.0.0.3 area 22
!
router bgp 100
 no bgp default ipv4-unicast
 neighbor 1.1.1.1 remote-as 100
 neighbor 1.1.1.1 update-source Loopback0
 !
 address-family vpnv4
  neighbor 1.1.1.1 activate
  neighbor 1.1.1.1 send-community both
 exit-address-family
 !
 address-family ipv4 vrf VPN_Customer1
  redistribute ospf 100 vrf VPN_Customer1
 exit-address-family
 !
 address-family ipv4 vrf VPN_Customer2
  redistribute ospf 200 vrf VPN_Customer2
 exit-address-family
!
mpls ip
mpls label protocol ldp
mpls ldp router-id Loopback0 force
!
end
write memory
```

### P1 Configuration

```bash
en
conf t
hostname P1
!
ip cef
!
interface Loopback0
 ip address 3.3.3.3 255.255.255.255
!
interface FastEthernet1/0
 description Connection to P2
 ip address 10.1.1.21 255.255.255.252
 mpls ip
 no shutdown
!
interface FastEthernet2/0
 description Connection to PE1
 ip address 10.1.1.2 255.255.255.252
 mpls ip
 no shutdown
!
interface FastEthernet3/0
 description Connection to PE2
 ip address 10.1.1.14 255.255.255.252
 mpls ip
 no shutdown
!
router ospf 1
 network 3.3.3.3 0.0.0.0 area 0
 network 10.1.1.0 0.0.0.3 area 0
 network 10.1.1.12 0.0.0.3 area 0
 network 10.1.1.20 0.0.0.3 area 0
!
mpls ip
mpls label protocol ldp
mpls ldp router-id Loopback0 force
!
end
write memory
```

### P2 Configuration

```bash
en
conf t
hostname P2
!
ip cef
!
interface Loopback0
 ip address 4.4.4.4 255.255.255.255
!
interface FastEthernet1/0
 description Connection to P1
 ip address 10.1.1.22 255.255.255.252
 mpls ip
 no shutdown
!
interface FastEthernet2/0
 description Connection to PE2
 ip address 10.1.1.10 255.255.255.252
 mpls ip
 no shutdown
!
interface FastEthernet3/0
 description Connection to PE1
 ip address 10.1.1.6 255.255.255.252
 mpls ip
 no shutdown
!
router ospf 1
 network 4.4.4.4 0.0.0.0 area 0
 network 10.1.1.4 0.0.0.3 area 0
 network 10.1.1.8 0.0.0.3 area 0
 network 10.1.1.20 0.0.0.3 area 0
!
mpls ip
mpls label protocol ldp
mpls ldp router-id Loopback0 force
!
end
write memory
```

## 🔄 VPN_SECURITY Configuration

Here are the commands to remove the existing customer VRFs and create the VPN_SECURITY VRF on both PE1 and PE2:

### PE1 VPN_SECURITY Configuration

```bash
en
conf t

! First, remove the interfaces from the existing VRFs
interface FastEthernet3/0
 no ip vrf forwarding
 no ip address
 shutdown

interface FastEthernet4/0
 no ip vrf forwarding
 no ip address
 shutdown

! Remove the BGP address families for the existing VRFs
router bgp 100
 no address-family ipv4 vrf VPN_Customer1
 no address-family ipv4 vrf VPN_Customer2

! Remove the OSPF processes for the existing VRFs
no router ospf 100 vrf VPN_Customer1
no router ospf 200 vrf VPN_Customer2

! Remove the existing VRFs
no ip vrf VPN_Customer1
no ip vrf VPN_Customer2

! Create the new VPN_SECURITY VRF
ip vrf VPN_SECURITY
 rd 100:3
 route-target both 100:3

! Configure the interfaces with the new VRF
interface FastEthernet3/0
 ip vrf forwarding VPN_SECURITY
 ip address 192.168.1.1 255.255.255.252
 no shutdown

interface FastEthernet4/0
 ip vrf forwarding VPN_SECURITY
 ip address 192.168.1.5 255.255.255.252
 no shutdown

! Configure OSPF for the new VRF
router ospf 300 vrf VPN_SECURITY
 redistribute bgp 100 subnets
 network 192.168.1.0 0.0.0.3 area 11
 network 192.168.1.4 0.0.0.3 area 21

! Configure BGP for the new VRF
router bgp 100
 address-family ipv4 vrf VPN_SECURITY
  redistribute ospf 300 vrf VPN_SECURITY
 exit-address-family

end
write memory
```

### PE2 VPN_SECURITY Configuration

```bash
en
conf t

! First, remove the interfaces from the existing VRFs
interface FastEthernet3/0
 no ip vrf forwarding
 no ip address
 shutdown

interface FastEthernet4/0
 no ip vrf forwarding
 no ip address
 shutdown

! Remove the BGP address families for the existing VRFs
router bgp 100
 no address-family ipv4 vrf VPN_Customer1
 no address-family ipv4 vrf VPN_Customer2

! Remove the OSPF processes for the existing VRFs
no router ospf 100 vrf VPN_Customer1
no router ospf 200 vrf VPN_Customer2

! Remove the existing VRFs
no ip vrf VPN_Customer1
no ip vrf VPN_Customer2

! Create the new VPN_SECURITY VRF
ip vrf VPN_SECURITY
 rd 100:3
 route-target both 100:3

! Configure the interfaces with the new VRF
interface FastEthernet3/0
 ip vrf forwarding VPN_SECURITY
 ip address 192.168.1.9 255.255.255.252
 no shutdown

interface FastEthernet4/0
 ip vrf forwarding VPN_SECURITY
 ip address 192.168.1.13 255.255.255.252
 no shutdown

! Configure OSPF for the new VRF
router ospf 300 vrf VPN_SECURITY
 redistribute bgp 100 subnets
 network 192.168.1.8 0.0.0.3 area 12
 network 192.168.1.12 0.0.0.3 area 22

! Configure BGP for the new VRF
router bgp 100
 address-family ipv4 vrf VPN_SECURITY
  redistribute ospf 300 vrf VPN_SECURITY
 exit-address-family

end
write memory
```

## 🔍 Verification Commands

```bash
! OSPF Verification
show ip ospf neighbor
show ip route ospf

! MPLS Verification
show mpls ldp neighbor
show mpls forwarding-table
show mpls ip binding

! VRF Verification (on PE routers)
show ip vrf
show ip vrf interfaces
show ip bgp vpnv4 all
show ip bgp vpnv4 vrf VPN_SECURITY
show ip route vrf VPN_SECURITY

! Connectivity Testing (from CE routers)
! From CE11:
ping 172.16.12.12    ! Should reach CE12
! From CE21:
ping 172.16.22.22    ! Should reach CE22
```