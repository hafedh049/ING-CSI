## 📊 Network Topology Overview

![[Pasted image 20250330121707.png]]

## 📋 Comprehensive IP Addressing Table

| **Router** | **Interface** | **IP Address** | **Subnet Mask** | **Description**   |
| ---------- | ------------- | -------------- | --------------- | ----------------- |
| **CE11**   | Loopback0     | 172.16.11.11   | 255.255.255.255 | Customer 1 Site 1 |
|            | G1/0          | 192.168.1.2    | 255.255.255.252 | Connect to PE1    |
| **CE12**   | Loopback0     | 172.16.12.12   | 255.255.255.255 | Customer 1 Site 2 |
|            | G1/0          | 192.168.1.10   | 255.255.255.252 | Connect to PE2    |
| **CE21**   | Loopback0     | 172.16.21.21   | 255.255.255.255 | Customer 2 Site 1 |
|            | G1/0          | 192.168.1.6    | 255.255.255.252 | Connect to PE1    |
| **CE22**   | Loopback0     | 172.16.22.22   | 255.255.255.255 | Customer 2 Site 2 |
|            | G1/0          | 192.168.1.14   | 255.255.255.252 | Connect to PE2    |
| **PE1**    | Loopback0     | 1.1.1.1        | 255.255.255.255 | Provider Edge 1   |
|            | G1/0          | 10.1.1.1       | 255.255.255.252 | Connect to P1     |
|            | G2/0          | 10.1.1.5       | 255.255.255.252 | Connect to P2     |
|            | G3/0          | 192.168.1.1    | 255.255.255.252 | Connect to CE11   |
|            | G4/0          | 192.168.1.5    | 255.255.255.252 | Connect to CE21   |
| **PE2**    | Loopback0     | 2.2.2.2        | 255.255.255.255 | Provider Edge 2   |
|            | G1/0          | 10.1.1.9       | 255.255.255.252 | Connect to P2     |
|            | G2/0          | 10.1.1.13      | 255.255.255.252 | Connect to P1     |
|            | G3/0          | 192.168.1.9    | 255.255.255.252 | Connect to CE12   |
|            | G4/0          | 192.168.1.13   | 255.255.255.252 | Connect to CE22   |
| **P1**     | Loopback0     | 3.3.3.3        | 255.255.255.255 | Provider 1        |
|            | G1/0          | 10.1.1.21      | 255.255.255.252 | Connect to P2     |
|            | G2/0          | 10.1.1.2       | 255.255.255.252 | Connect to PE1    |
|            | G3/0          | 10.1.1.14      | 255.255.255.252 | Connect to PE2    |
| **P2**     | Loopback0     | 4.4.4.4        | 255.255.255.255 | Provider 2        |
|            | G1/0          | 10.1.1.22      | 255.255.255.252 | Connect to P1     |
|            | G2/0          | 10.1.1.10      | 255.255.255.252 | Connect to PE2    |
|            | G3/0          | 10.1.1.6       | 255.255.255.252 | Connect to PE1    |

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

### 🔹 P1 Router Configuration

```bash
! Basic Configuration
hostname P1
!
! Interface Configuration
interface Loopback0
 ip address 3.3.3.3 255.255.255.255
!
interface GigabitEthernet1/0
 description Connect to P2
 ip address 10.1.1.21 255.255.255.252
 no shutdown
!
interface GigabitEthernet2/0
 description Connect to PE1
 ip address 10.1.1.2 255.255.255.252
 no shutdown
!
interface GigabitEthernet3/0
 description Connect to PE2
 ip address 10.1.1.14 255.255.255.252
 no shutdown
!
! OSPF Configuration
router ospf 1
 network 3.3.3.3 0.0.0.0 area 0
 network 10.1.1.0 0.0.0.3 area 0
 network 10.1.1.12 0.0.0.3 area 0
 network 10.1.1.20 0.0.0.3 area 0
!
! MPLS Configuration
ip cef
mpls ip
mpls label protocol ldp
mpls ldp router-id Loopback0 force
!
interface GigabitEthernet1/0
 mpls ip
!
interface GigabitEthernet2/0
 mpls ip
!
interface GigabitEthernet3/0
 mpls ip
```

### 🔹 P2 Router Configuration

```bash
! Basic Configuration
hostname P2
!
! Interface Configuration
interface Loopback0
 ip address 4.4.4.4 255.255.255.255
!
interface GigabitEthernet1/0
 description Connect to P1
 ip address 10.1.1.22 255.255.255.252
 no shutdown
!
interface GigabitEthernet2/0
 description Connect to PE2
 ip address 10.1.1.10 255.255.255.252
 no shutdown
!
interface GigabitEthernet3/0
 description Connect to PE1
 ip address 10.1.1.6 255.255.255.252
 no shutdown
!
! OSPF Configuration
router ospf 1
 network 4.4.4.4 0.0.0.0 area 0
 network 10.1.1.4 0.0.0.3 area 0
 network 10.1.1.8 0.0.0.3 area 0
 network 10.1.1.20 0.0.0.3 area 0
!
! MPLS Configuration
ip cef
mpls ip
mpls label protocol ldp
mpls ldp router-id Loopback0 force
!
interface GigabitEthernet1/0
 mpls ip
!
interface GigabitEthernet2/0
 mpls ip
!
interface GigabitEthernet3/0
 mpls ip
```

### 🔹 PE1 Router Configuration

```bash
! Basic Configuration
hostname PE1
!
! Interface Configuration
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
!
interface GigabitEthernet1/0
 description Connect to P1
 ip address 10.1.1.1 255.255.255.252
 no shutdown
!
interface GigabitEthernet2/0
 description Connect to P2
 ip address 10.1.1.5 255.255.255.252
 no shutdown
!
! OSPF Configuration for Backbone
router ospf 1
 network 1.1.1.1 0.0.0.0 area 0
 network 10.1.1.0 0.0.0.3 area 0
 network 10.1.1.4 0.0.0.3 area 0
!
! MPLS Configuration
ip cef
mpls ip
mpls label protocol ldp
mpls ldp router-id Loopback0 force
!
interface GigabitEthernet1/0
 mpls ip
!
interface GigabitEthernet2/0
 mpls ip
!
! VRF Configuration
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
interface GigabitEthernet3/0
 ip vrf forwarding VPN_Customer1
 ip address 192.168.1.1 255.255.255.252
 no shutdown
!
interface GigabitEthernet4/0
 ip vrf forwarding VPN_Customer2
 ip address 192.168.1.5 255.255.255.252
 no shutdown
!
! MP-BGP Configuration
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
! OSPF for VRF Customer1
router ospf 100 vrf VPN_Customer1
 redistribute bgp 100 subnets
 network 192.168.1.0 0.0.0.3 area 11
!
! OSPF for VRF Customer2
router ospf 200 vrf VPN_Customer2
 redistribute bgp 100 subnets
 network 192.168.1.4 0.0.0.3 area 21
```

### 🔹 PE2 Router Configuration

```bash
! Basic Configuration
hostname PE2
!
! Interface Configuration
interface Loopback0
 ip address 2.2.2.2 255.255.255.255
!
interface GigabitEthernet1/0
 description Connect to P2
 ip address 10.1.1.9 255.255.255.252
 no shutdown
!
interface GigabitEthernet2/0
 description Connect to P1
 ip address 10.1.1.13 255.255.255.252
 no shutdown
!
! OSPF Configuration for Backbone
router ospf 1
 network 2.2.2.2 0.0.0.0 area 0
 network 10.1.1.8 0.0.0.3 area 0
 network 10.1.1.12 0.0.0.3 area 0
!
! MPLS Configuration
ip cef
mpls ip
mpls label protocol ldp
mpls ldp router-id Loopback0 force
!
interface GigabitEthernet1/0
 mpls ip
!
interface GigabitEthernet2/0
 mpls ip
!
! VRF Configuration
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
interface GigabitEthernet3/0
 ip vrf forwarding VPN_Customer1
 ip address 192.168.1.9 255.255.255.252
 no shutdown
!
interface GigabitEthernet4/0
 ip vrf forwarding VPN_Customer2
 ip address 192.168.1.13 255.255.255.252
 no shutdown
!
! MP-BGP Configuration
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
! OSPF for VRF Customer1
router ospf 100 vrf VPN_Customer1
 redistribute bgp 100 subnets
 network 192.168.1.8 0.0.0.3 area 12
!
! OSPF for VRF Customer2
router ospf 200 vrf VPN_Customer2
 redistribute bgp 100 subnets
 network 192.168.1.12 0.0.0.3 area 22
```

### 🔹 CE11 Router Configuration

```bash
! Basic Configuration
hostname CE11
!
! Interface Configuration
interface Loopback0
 ip address 172.16.11.11 255.255.255.255
!
interface GigabitEthernet1/0
 description Connect to PE1
 ip address 192.168.1.2 255.255.255.252
 no shutdown
!
! OSPF Configuration
router ospf 1
 network 172.16.11.11 0.0.0.0 area 11
 network 192.168.1.0 0.0.0.3 area 11
```

### 🔹 CE12 Router Configuration

```bash
! Basic Configuration
hostname CE12
!
! Interface Configuration
interface Loopback0
 ip address 172.16.12.12 255.255.255.255
!
interface GigabitEthernet1/0
 description Connect to PE2
 ip address 192.168.1.10 255.255.255.252
 no shutdown
!
! OSPF Configuration
router ospf 1
 network 172.16.12.12 0.0.0.0 area 12
 network 192.168.1.8 0.0.0.3 area 12
```

### 🔹 CE21 Router Configuration

```bash
! Basic Configuration
hostname CE21
!
! Interface Configuration
interface Loopback0
 ip address 172.16.21.21 255.255.255.255
!
interface GigabitEthernet1/0
 description Connect to PE1
 ip address 192.168.1.6 255.255.255.252
 no shutdown
!
! OSPF Configuration
router ospf 1
 network 172.16.21.21 0.0.0.0 area 21
 network 192.168.1.4 0.0.0.3 area 21
```

### 🔹 CE22 Router Configuration

```bash
! Basic Configuration
hostname CE22
!
! Interface Configuration
interface Loopback0
 ip address 172.16.22.22 255.255.255.255
!
interface GigabitEthernet1/0
 description Connect to PE2
 ip address 192.168.1.14 255.255.255.252
 no shutdown
!
! OSPF Configuration
router ospf 1
 network 172.16.22.22 0.0.0.0 area 22
 network 192.168.1.12 0.0.0.3 area 22
```

## 🔄 OSPF Area Structure

![[Pasted image 20250330121613.png]]

## 🔍 Verification Commands

### 🔹 OSPF Verification

```bash
! Display OSPF neighbors
show ip ospf neighbor

! Example output:
Neighbor ID     Pri   State           Dead Time   Address         Interface
4.4.4.4           1   FULL/DR         00:00:33    10.1.1.22       GigabitEthernet1/0
1.1.1.1           1   FULL/BDR        00:00:39    10.1.1.1        GigabitEthernet2/0
2.2.2.2           1   FULL/BDR        00:00:33    10.1.1.13       GigabitEthernet3/0

! Display OSPF routes
show ip route ospf

! Example output:
O    1.1.1.1/32 [110/11] via 10.1.1.1, 00:18:35, GigabitEthernet2/0
O    2.2.2.2/32 [110/11] via 10.1.1.13, 00:18:35, GigabitEthernet3/0
O    4.4.4.4/32 [110/11] via 10.1.1.22, 00:18:35, GigabitEthernet1/0
O    10.1.1.4/30 [110/20] via 10.1.1.1, 00:18:35, GigabitEthernet2/0
                  [110/20] via 10.1.1.22, 00:18:35, GigabitEthernet1/0
O    10.1.1.8/30 [110/20] via 10.1.1.13, 00:18:35, GigabitEthernet3/0
                  [110/20] via 10.1.1.22, 00:18:35, GigabitEthernet1/0
```

### 🔹 MPLS Verification

```bash
! Display MPLS LDP neighbors
show mpls ldp neighbors

! Example output:
Peer LDP Ident: 1.1.1.1:0; Local LDP Ident 3.3.3.3:0
    TCP connection: 1.1.1.1.646 - 3.3.3.3.11001
    State: Oper; Msgs sent/rcvd: 61/59; Downstream
    Up time: 00:52:19
    LDP discovery sources:
      GigabitEthernet2/0, Src IP addr: 10.1.1.1
    Addresses bound to peer LDP Ident:
      10.1.1.1       10.1.1.5       1.1.1.1        

! Display MPLS forwarding table
show mpls forwarding-table

! Example output:
Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop    
Label      Label      or Tunnel Id     Switched      interface              
16         Pop Label  1.1.1.1/32       0             Gi2/0      10.1.1.1    
17         Pop Label  2.2.2.2/32       0             Gi3/0      10.1.1.13   
18         Pop Label  4.4.4.4/32       0             Gi1/0      10.1.1.22   
19         17         10.1.1.4/30      0             Gi2/0      10.1.1.1    
20         18         10.1.1.8/30      0             Gi3/0      10.1.1.13   

! Display MPLS LDP bindings
show mpls ip binding

! Example output:
  1.1.1.1/32
        in label:     16
        out label:    imp-null        lsr: 1.1.1.1:0
  2.2.2.2/32
        in label:     17
        out label:    16              lsr: 1.1.1.1:0
        out label:    imp-null        lsr: 2.2.2.2:0
  3.3.3.3/32
        in label:     imp-null
        out label:    16              lsr: 1.1.1.1:0
        out label:    16              lsr: 2.2.2.2:0
```

### 🔹 VRF and VPN Verification

```css
! Display VRF information
show ip vrf

! Example output:
  Name                             Default RD          Interfaces
  VPN_Customer1                    100:1               Gi3/0
  VPN_Customer2                    100:2               Gi4/0

! Display VRF routing table for Customer1
show ip route vrf VPN_Customer1

! Output:
Routing Table: VPN_Customer1
Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      172.16.11.0/32 is subnetted, 1 subnets
O        172.16.11.11 [110/2] via 192.168.1.2, 00:25:32, GigabitEthernet3/0
      172.16.12.0/32 is subnetted, 1 subnets
B        172.16.12.12 [200/0] via 2.2.2.2, 00:25:32
      192.168.1.0/30 is subnetted, 2 subnets
C        192.168.1.0 is directly connected, GigabitEthernet3/0
B        192.168.1.8 [200/0] via 2.2.2.2, 00:25:32

! Display BGP VPNv4 routes for Customer1
show ip bgp vpnv4 vrf VPN_Customer1

! Output:
BGP table version is 5, local router ID is 1.1.1.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
              t secondary path, L long-lived-stale,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
Route Distinguisher: 100:1 (default for vrf VPN_Customer1)
 *>i172.16.12.12/32   2.2.2.2                  0    100      0 ?
 *>i192.168.1.8/30    2.2.2.2                  0    100      0 ?
 *> 172.16.11.11/32   192.168.1.2              2         32768 ?
 *> 192.168.1.0/30    0.0.0.0                  0         32768 ?
```

### 1. Testing from CE11 (Customer 1, Site 1)

```bash
CE11# ping 172.16.12.12
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.12.12, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 4/6/8 ms

CE11# ping 172.16.21.21
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.21.21, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
```

### 2. Testing from CE12 (Customer 1, Site 2)

```bash
CE12# ping 172.16.11.11
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.11.11, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 4/7/9 ms

CE12# ping 172.16.22.22
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.22.22, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
```

### 3. Testing from CE21 (Customer 2, Site 1)

```bash
CE21# ping 172.16.22.22
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.22.22, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 5/7/10 ms

CE21# ping 172.16.11.11
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.11.11, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
```

### 4. Testing from CE22 (Customer 2, Site 2)

```bash
CE22# ping 172.16.21.21
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.21.21, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 4/6/9 ms

CE22# ping 172.16.12.12
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.12.12, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
```

### 5. Testing Traceroute from CE11 to CE12

```bash
CE11# traceroute 172.16.12.12
Type escape sequence to abort.
Tracing the route to 172.16.12.12

  1 192.168.1.1 [MPLS: Label 21 Exp 0] 4 msec 4 msec 4 msec
  2 10.1.1.2 [MPLS: Label 19 Exp 0] 8 msec 8 msec 8 msec
  3 10.1.1.14 [MPLS: Label 16 Exp 0] 12 msec 12 msec 12 msec
  4 192.168.1.10 16 msec 16 msec 16 msec
```

These test results confirm that:

1. ✅ Sites within the same VPN can communicate with each other (100% success rate)
2. ❌ Sites in different VPNs cannot communicate (0% success rate)
3. 🔄 The traceroute shows the MPLS label switching path through the provider network
