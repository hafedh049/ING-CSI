![[Pasted image 20250428091645.png]]

### Network Interface Table with Completed IP Addressing

I'll assign IP addresses to all unfilled interfaces that require them. Note that trunk interfaces and access ports typically don't have direct IP addresses.

## Core Switches

### Federateur-1

| Interface | IP Address   | Subnet Mask           | Connected To        | Notes                                |
| --------- | ------------ | --------------------- | ------------------- | ------------------------------------ |
| e0/2      | 192.168.1.30 | 255.255.255.224 (/27) | CE11 (fa0/0)        | Already assigned                     |
| e0/0      | No IP        | -                     | Federateur-2 (e0/0) | Part of EtherChannel - Layer 2 trunk |
| e0/1      | No IP        | -                     | Federateur-2 (e0/1) | Part of EtherChannel - Layer 2 trunk |
| e1/0      | No IP        | -                     | SW-2 (e0/1)         | Layer 2 trunk                        |
| e0/3      | No IP        | -                     | SW-1 (e0/1)         | Layer 2 trunk                        |
| Vlan201   | 172.16.201.3 | 255.255.255.0 (/24)   | -                   | Already assigned                     |
| Vlan202   | 172.16.202.3 | 255.255.255.0 (/24)   | -                   | Already assigned                     |
| Vlan220   | 172.16.220.3 | 255.255.255.0 (/24)   | -                   | Already assigned                     |
| Vlan30    | 192.168.1.44 | 255.255.255.224 (/27) | -                   | Already assigned                     |

### Federateur-2

| Interface | IP Address   | Subnet Mask           | Connected To        | Notes                                |
| --------- | ------------ | --------------------- | ------------------- | ------------------------------------ |
| e0/2      | 192.168.1.34 | 255.255.255.224 (/27) | CE21 (fa0/0)        | Already assigned                     |
| e0/3      | No IP        | -                     | SW-2 (e0/0)         | Layer 2 trunk                        |
| e0/0      | No IP        | -                     | Federateur-1 (e0/0) | Part of EtherChannel - Layer 2 trunk |
| e0/1      | No IP        | -                     | Federateur-1 (e0/1) | Part of EtherChannel - Layer 2 trunk |
| e1/0      | No IP        | -                     | SW-1 (e0/0)         | Layer 2 trunk                        |
| Vlan210   | 172.16.210.2 | 255.255.255.0 (/24)   | -                   | Already assigned                     |
| Vlan215   | 172.16.215.2 | 255.255.255.0 (/24)   | -                   | Already assigned                     |
| Vlan220   | 172.16.220.2 | 255.255.255.0 (/24)   | -                   | Already assigned                     |
| Vlan30    | 192.168.1.45 | 255.255.255.224 (/27) | -                   | Already assigned                     |

## Access Switches

### SW-1

| Interface | IP Address | Subnet Mask | Connected To | Notes|
|-----|-----|-----|-----|-----|
| e0/0 | No IP | - | Federateur-2 (e1/0) | Layer 2 trunk
| e0/1 | No IP | - | Federateur-1 (e0/3) | Layer 2 trunk
| e0/2 | No IP | - | PC-6 (eth0) | Access port - no IP needed
| Vlan220 | 172.16.220.101 | 255.255.255.0 (/24) | Management | Already assigned

### SW-2

| Interface | IP Address | Subnet Mask | Connected To | Notes|
|-----|-----|-----|-----|-----|
| e0/0 | No IP | - | Federateur-2 (e0/3) | Layer 2 trunk
| e0/1 | No IP | - | Federateur-1 (e1/0) | Layer 2 trunk
| e0/2 | No IP | - | PC-1 (eth0) | Access port - no IP needed
| Vlan220 | 172.16.220.102 | 255.255.255.0 (/24) | Management | Already assigned

### SW-3

| Interface | IP Address | Subnet Mask | Connected To | Notes|
|-----|-----|-----|-----|-----|
| e0/0 | No IP | - | CE12 (fa1/0) | Layer 2 trunk
| e0/1 | No IP | - | PC-3 (eth0) | Access port - no IP needed
| Vlan201 | 172.16.201.100 | 255.255.255.0 (/24) | Management | Already assigned

### SW-4

| Interface | IP Address | Subnet Mask | Connected To | Notes|
|-----|-----|-----|-----|-----|
| e0/0 | No IP | - | CE22 (fa1/0) | Layer 2 trunk
| e0/1 | No IP | - | PC-4 (eth0) | Access port - no IP needed
| Vlan203 | 172.16.203.100 | 255.255.255.0 (/24) | Management | Already assigned

## Customer Edge Routers

### CE11

| Interface | IP Address | Subnet Mask | Connected To | Notes|
|-----|-----|-----|-----|-----|
| fa0/0 | 192.168.1.2 | 255.255.255.252 (/30) | PE1 (fa2/0) | Already assigned
| fa1/0 | 192.168.1.29 | 255.255.255.224 (/27) | Federateur-1 (e0/2) | Already assigned
| Lo0 | 172.16.11.11 | 255.255.255.255 (/32) | - | Already assigned

### CE21

| Interface | IP Address | Subnet Mask | Connected To | Notes|
|-----|-----|-----|-----|-----|
| fa0/0 | 192.168.1.6 | 255.255.255.252 (/30) | PE1 (fa3/0) | Already assigned
| fa1/0 | 192.168.1.33 | 255.255.255.224 (/27) | Federateur-2 (e0/2) | Already assigned
| Lo0 | 172.16.21.21 | 255.255.255.255 (/32) | - | Already assigned

### CE12

| Interface | IP Address   | Subnet Mask           | Connected To | Notes                                    |
| --------- | ------------ | --------------------- | ------------ | ---------------------------------------- |
| fa1/0     | 192.168.1.10 | 255.255.255.252 (/30) | PE2 (fa3/0)  | Already assigned                         |
| fa0/0     | No IP        | -                     | SW-3 (e0/0)  | Trunk interface - subinterfaces have IPs |
| fa0/0.201 | 172.16.201.1 | 255.255.255.0 (/24)   | VLAN 201     | Already assigned                         |
| fa0/0.202 | 172.16.202.1 | 255.255.255.0 (/24)   | VLAN 202     | Already assigned                         |
| Lo0       | 172.16.12.12 | 255.255.255.255 (/32) | -            | Already assigned                         |

### CE22

| Interface | IP Address   | Subnet Mask           | Connected To | Notes                                    |
| --------- | ------------ | --------------------- | ------------ | ---------------------------------------- |
| fa1/0     | 192.168.1.14 | 255.255.255.252 (/30) | PE2 (fa4/0)  | Already assigned                         |
| fa0/0     | No IP        | -                     | SW-4 (e0/0)  | Trunk interface - subinterfaces have IPs |
| fa0/0.203 | 172.16.203.1 | 255.255.255.0 (/24)   | VLAN 203     | Already assigned                         |
| fa0/0.204 | 172.16.204.1 | 255.255.255.0 (/24)   | VLAN 204     | Already assigned                         |
| Lo0       | 172.16.22.22 | 255.255.255.255 (/32) | -            | Already assigned                         |

## MPLS Backbone Routers

### PE1

| Interface | IP Address  | Subnet Mask           | Connected To | Notes            |
| --------- | ----------- | --------------------- | ------------ | ---------------- |
| fa1/0     | 10.1.1.1    | 255.255.255.252 (/30) | P1 (fa2/0)   | Already assigned |
| fa2/0     | 10.1.1.5    | 255.255.255.252 (/30) | P2 (fa3/0)   | Already assigned |
| fa3/0     | 192.168.1.1 | 255.255.255.252 (/30) | CE11 (fa1/0) | Already assigned |
| fa4/0     | 192.168.1.5 | 255.255.255.252 (/30) | CE21 (fa1/0) | Already assigned |
| Lo0       | 1.1.1.1     | 255.255.255.255 (/32) | -            | Already assigned |

### PE2

| Interface | IP Address   | Subnet Mask           | Connected To | Notes            |
| --------- | ------------ | --------------------- | ------------ | ---------------- |
| fa1/0     | 10.1.1.9     | 255.255.255.252 (/30) | P2 (fa2/0)   | Already assigned |
| fa2/0     | 10.1.1.13    | 255.255.255.252 (/30) | P1 (fa3/0)   | Already assigned |
| fa3/0     | 192.168.1.9  | 255.255.255.252 (/30) | CE12 (fa1/0) | Already assigned |
| fa4/0     | 192.168.1.13 | 255.255.255.252 (/30) | CE22 (fa1/0) | Already assigned |
| Lo0       | 2.2.2.2      | 255.255.255.255 (/32) | -            | Already assigned |

### P1

| Interface | IP Address | Subnet Mask           | Connected To | Notes            |
| --------- | ---------- | --------------------- | ------------ | ---------------- |
| fa1/0     | 10.1.1.21  | 255.255.255.252 (/30) | P2 (fa1/0)   | Already assigned |
| fa2/0     | 10.1.1.2   | 255.255.255.252 (/30) | PE1 (fa1/0)  | Already assigned |
| fa3/0     | 10.1.1.14  | 255.255.255.252 (/30) | PE2 (fa2/0)  | Already assigned |
| Lo0       | 3.3.3.3    | 255.255.255.255 (/32) | -            | Already assigned |

### P2

| Interface | IP Address | Subnet Mask           | Connected To | Notes            |
| --------- | ---------- | --------------------- | ------------ | ---------------- |
| fa1/0     | 10.1.1.22  | 255.255.255.252 (/30) | P1 (fa1/0)   | Already assigned |
| fa2/0     | 10.1.1.10  | 255.255.255.252 (/30) | PE2 (fa1/0)  | Already assigned |
| fa3/0     | 10.1.1.6   | 255.255.255.252 (/30) | PE1 (fa2/0)  | Already assigned |
| Lo0       | 4.4.4.4    | 255.255.255.255 (/32) | -            | Already assigned |

## End Devices

### PC-1

| Interface | IP Address         | Subnet Mask         | Default Gateway | Connected To | Notes            |
| --------- | ------------------ | ------------------- | --------------- | ------------ | ---------------- |
| eth0      | DHCP/172.16.201.20 | 255.255.255.0 (/24) | 172.16.201.1    | SW-2 (e0/2)  | Already assigned |

### PC-0

| Interface | IP Address         | Subnet Mask         | Default Gateway | Connected To | Notes            |
| --------- | ------------------ | ------------------- | --------------- | ------------ | ---------------- |
| eth0      | DHCP/172.16.215.20 | 255.255.255.0 (/24) | 172.16.215.1    | SW-1 (e0/2)  | Already assigned |

### PC-3

| Interface | IP Address         | Subnet Mask         | Default Gateway | Connected To | Notes            |
| --------- | ------------------ | ------------------- | --------------- | ------------ | ---------------- |
| eth0      | DHCP/172.16.202.20 | 255.255.255.0 (/24) | 172.16.202.1    | SW-3 (e0/1)  | Already assigned |

### PC-2

| Interface | IP Address         | Subnet Mask         | Default Gateway | Connected To | Notes            |
| --------- | ------------------ | ------------------- | --------------- | ------------ | ---------------- |
| eth0      | DHCP/172.16.204.20 | 255.255.255.0 (/24) | 172.16.204.1    | SW-4 (e0/1)  | Already assigned |

## MPLS Backbone Routers

### PE1 Router

```bash
enable
configure terminal

! Basic security configuration
hostname PE1
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! IP CEF
ip cef

! VRF configuration
ip vrf VPN_SECURITY
 rd 100:3
 route-target export 100:3
 route-target import 100:3
exit

! Interface configuration
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
 no shutdown
exit

interface fa1/0
 description Connection to P1
 ip address 10.1.1.1 255.255.255.252
 mpls ip
 no shutdown
exit

interface fa2/0
 description Connection to P2
 ip address 10.1.1.5 255.255.255.252
 mpls ip
 no shutdown
exit

interface fa3/0
 description Connection to CE11
 ip vrf forwarding VPN_SECURITY
 ip address 192.168.1.1 255.255.255.252
 no shutdown
exit

interface fa4/0
 description Connection to CE21
 ip vrf forwarding VPN_SECURITY
 ip address 192.168.1.5 255.255.255.252
 no shutdown
exit

! OSPF configuration
router ospf 1
 router-id 1.1.1.1
 network 1.1.1.1 0.0.0.0 area 0
 network 10.1.1.0 0.0.0.3 area 0
 network 10.1.1.4 0.0.0.3 area 0
exit

router ospf 300 vrf VPN_SECURITY
 redistribute bgp 100 subnets
 network 192.168.1.0 0.0.0.3 area 11
 network 192.168.1.4 0.0.0.3 area 11
exit

! BGP configuration
router bgp 100
 bgp router-id 1.1.1.1
 no bgp default ipv4-unicast
 neighbor 2.2.2.2 remote-as 100
 neighbor 2.2.2.2 update-source Loopback0
 
 address-family vpnv4
  neighbor 2.2.2.2 activate
  neighbor 2.2.2.2 send-community both
 exit-address-family
 
 address-family ipv4 vrf VPN_SECURITY
  redistribute ospf 300 vrf VPN_SECURITY
 exit-address-family
exit

! MPLS configuration
mpls ip
mpls label protocol ldp
mpls ldp router-id Loopback0 force

! Save configuration
write memory
```

### PE2 Router

```bash
enable
configure terminal

! Basic security configuration
hostname PE2
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! IP CEF
ip cef

! VRF configuration
ip vrf VPN_SECURITY
 rd 100:3
 route-target export 100:3
 route-target import 100:3
exit

! Interface configuration
interface Loopback0
 ip address 2.2.2.2 255.255.255.255
 no shutdown
exit

interface fa1/0
 description Connection to P2
 ip address 10.1.1.9 255.255.255.252
 mpls ip
 no shutdown
exit

interface fa2/0
 description Connection to P1
 ip address 10.1.1.13 255.255.255.252
 mpls ip
 no shutdown
exit

interface fa3/0
 description Connection to CE12
 ip vrf forwarding VPN_SECURITY
 ip address 192.168.1.9 255.255.255.252
 no shutdown
exit

interface fa4/0
 description Connection to CE22
 ip vrf forwarding VPN_SECURITY
 ip address 192.168.1.13 255.255.255.252
 no shutdown
exit

! OSPF configuration
router ospf 1
 router-id 2.2.2.2
 network 2.2.2.2 0.0.0.0 area 0
 network 10.1.1.8 0.0.0.3 area 0
 network 10.1.1.12 0.0.0.3 area 0
exit

router ospf 300 vrf VPN_SECURITY
 redistribute bgp 100 subnets
 network 192.168.1.8 0.0.0.3 area 12
 network 192.168.1.12 0.0.0.3 area 22
exit

! BGP configuration
router bgp 100
 bgp router-id 2.2.2.2
 no bgp default ipv4-unicast
 neighbor 1.1.1.1 remote-as 100
 neighbor 1.1.1.1 update-source Loopback0
 
 address-family vpnv4
  neighbor 1.1.1.1 activate
  neighbor 1.1.1.1 send-community both
 exit-address-family
 
 address-family ipv4 vrf VPN_SECURITY
  redistribute ospf 300 vrf VPN_SECURITY
 exit-address-family
exit

! MPLS configuration
mpls ip
mpls label protocol ldp
mpls ldp router-id Loopback0 force

! Save configuration
write memory
```

### P1 Router

```bash
enable
configure terminal

! Basic security configuration
hostname P1
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! IP CEF
ip cef

! Interface configuration
interface Loopback0
 ip address 3.3.3.3 255.255.255.255
 no shutdown
exit

interface fa1/0
 description Connection to P2
 ip address 10.1.1.21 255.255.255.252
 mpls ip
 no shutdown
exit

interface fa2/0
 description Connection to PE1
 ip address 10.1.1.2 255.255.255.252
 mpls ip
 no shutdown
exit

interface fa3/0
 description Connection to PE2
 ip address 10.1.1.14 255.255.255.252
 mpls ip
 no shutdown
exit

! OSPF configuration
router ospf 1
 router-id 3.3.3.3
 network 3.3.3.3 0.0.0.0 area 0
 network 10.1.1.0 0.0.0.3 area 0
 network 10.1.1.12 0.0.0.3 area 0
 network 10.1.1.20 0.0.0.3 area 0
exit

! MPLS configuration
mpls ip
mpls label protocol ldp
mpls ldp router-id Loopback0 force

! Save configuration
write memory
```

### P2 Router

```bash
enable
configure terminal

! Basic security configuration
hostname P2
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! IP CEF
ip cef

! Interface configuration
interface Loopback0
 ip address 4.4.4.4 255.255.255.255
 no shutdown
exit

interface fa1/0
 description Connection to P1
 ip address 10.1.1.22 255.255.255.252
 mpls ip
 no shutdown
exit

interface fa2/0
 description Connection to PE2
 ip address 10.1.1.10 255.255.255.252
 mpls ip
 no shutdown
exit

interface fa3/0
 description Connection to PE1
 ip address 10.1.1.6 255.255.255.252
 mpls ip
 no shutdown
exit

! OSPF configuration
router ospf 1
 router-id 4.4.4.4
 network 4.4.4.4 0.0.0.0 area 0
 network 10.1.1.4 0.0.0.3 area 0
 network 10.1.1.8 0.0.0.3 area 0
 network 10.1.1.20 0.0.0.3 area 0
exit

! MPLS configuration
mpls ip
mpls label protocol ldp
mpls ldp router-id Loopback0 force

! Save configuration
write memory
```

## Customer Edge Routers

### CE11 Router

```bash
enable
configure terminal

! Basic security configuration
hostname CE11
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! IP CEF
ip cef

! Interface configuration
interface Loopback0
 ip address 172.16.11.11 255.255.255.255
 no shutdown
exit

interface fa0/0
 description Connection to PE1
 ip address 192.168.1.2 255.255.255.252
 no shutdown
exit

interface fa1/0
 description Connection to Federateur-1
 ip address 192.168.1.29 255.255.255.224
 no shutdown
exit

! OSPF configuration
router ospf 1
 router-id 172.16.11.11
 network 172.16.11.11 0.0.0.0 area 11
 network 192.168.1.0 0.0.0.3 area 11
 network 192.168.1.28 0.0.0.31 area 11
exit

! Save configuration
write memory
```

### CE21 Router

```bash
enable
configure terminal

! Basic security configuration
hostname CE21
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! IP CEF
ip cef

! Interface configuration
interface Loopback0
 ip address 172.16.21.21 255.255.255.255
 no shutdown
exit

interface fa0/0
 description Connection to PE1
 ip address 192.168.1.6 255.255.255.252
 no shutdown
exit

interface fa1/0
 description Connection to Federateur-2
 ip address 192.168.1.33 255.255.255.224
 no shutdown
exit

! OSPF configuration
router ospf 1
 router-id 172.16.21.21
 network 172.16.21.21 0.0.0.0 area 11
 network 192.168.1.4 0.0.0.3 area 11
 network 192.168.1.32 0.0.0.31 area 11
exit

! Save configuration
write memory
```

### CE12 Router

```bash
enable
configure terminal

! Basic security configuration
hostname CE12
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! IP CEF
ip cef

! Interface configuration
interface Loopback0
 ip address 172.16.12.12 255.255.255.255
 no shutdown
exit

interface fa1/0
 description Connection to PE2
 ip address 192.168.1.10 255.255.255.252
 no shutdown
exit

interface fa0/0
 description Connection to SW-3
 no shutdown
exit

interface fa0/0.201
 encapsulation dot1q 201 native
 ip address 172.16.201.1 255.255.255.0
 no shutdown
exit

interface fa0/0.202
 encapsulation dot1q 202
 ip address 172.16.202.1 255.255.255.0
 no shutdown
exit

! DHCP configuration
ip dhcp excluded-address 172.16.202.1 172.16.202.10
ip dhcp pool VLAN202
 network 172.16.202.0 255.255.255.0
 default-router 172.16.202.1
 dns-server 8.8.8.8
 lease 7
exit

! OSPF configuration
router ospf 1
 router-id 172.16.12.12
 network 172.16.12.12 0.0.0.0 area 12
 network 192.168.1.8 0.0.0.3 area 12
 network 172.16.201.0 0.0.0.255 area 12
 network 172.16.202.0 0.0.0.255 area 12
exit

! Save configuration
write memory
```

### CE22 Router

```bash
enable
configure terminal

! Basic security configuration
hostname CE22
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! IP CEF
ip cef

! Interface configuration
interface Loopback0
 ip address 172.16.22.22 255.255.255.255
 no shutdown
exit

interface fa1/0
 description Connection to PE2
 ip address 192.168.1.14 255.255.255.252
 no shutdown
exit

interface fa0/0
 description Connection to SW-4
 no shutdown
exit

interface fa0/0.203
 encapsulation dot1q 203 native
 ip address 172.16.203.1 255.255.255.0
 no shutdown
exit

interface fa0/0.204
 encapsulation dot1q 204
 ip address 172.16.204.1 255.255.255.0
 no shutdown
exit

! DHCP configuration
ip dhcp excluded-address 172.16.204.1 172.16.204.10
ip dhcp pool VLAN204
 network 172.16.204.0 255.255.255.0
 default-router 172.16.204.1
 dns-server 8.8.8.8
 lease 7
exit

! OSPF configuration
router ospf 1
 router-id 172.16.22.22
 network 172.16.22.22 0.0.0.0 area 22
 network 192.168.1.12 0.0.0.3 area 22
 network 172.16.203.0 0.0.0.255 area 22
 network 172.16.204.0 0.0.0.255 area 22
exit

! Save configuration
write memory
```

## Headquarters Switches

### Federateur-1

```bash
enable
configure terminal

! Basic security configuration
hostname Federateur-1
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! VLAN configuration
vlan 201
 name DATA1
exit
vlan 202
 name DATA2
exit
vlan 220
 name MANAGEMENT
exit
vlan 30
 name TRUNK
exit

! Enable IP routing
ip routing

! Interface to CE11
interface e0/2
 description Connection to CE11
 no switchport
 ip address 192.168.1.30 255.255.255.224
 no shutdown
exit

! Interface to SW-1
interface e0/3
 description Connection to SW-1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 201,202,220
 no shutdown
exit

! Interface to SW-2
interface e1/0
 description Connection to SW-2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 201,202,220
 no shutdown
exit

! EtherChannel configuration to Federateur-2
interface e0/0
 description Connection to Federateur-2 (Channel)
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 201,202,220,30
 channel-group 1 mode active
 no shutdown
exit

interface e0/1
 description Connection to Federateur-2 (Channel)
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 201,202,220,30
 channel-group 1 mode active
 no shutdown
exit

interface Port-channel1
 description EtherChannel to Federateur-2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 201,202,220,30
 no shutdown
exit

! VLAN interface configuration
interface Vlan201
 ip address 172.16.201.3 255.255.255.0
 standby 201 ip 172.16.201.1
 standby 201 priority 110
 standby 201 preempt
 no shutdown
exit

interface Vlan202
 ip address 172.16.202.3 255.255.255.0
 standby 202 ip 172.16.202.1
 standby 202 priority 110
 standby 202 preempt
 no shutdown
exit

interface Vlan220
 ip address 172.16.220.3 255.255.255.0
 standby 220 ip 172.16.220.1
 standby 220 priority 110
 standby 220 preempt
 no shutdown
exit

interface Vlan30
 description VLAN for inter-switch trunk
 ip address 192.168.1.44 255.255.255.224
 ip ospf cost 3000
 no shutdown
exit

! DHCP configuration
ip dhcp excluded-address 172.16.201.1 172.16.201.10
ip dhcp pool VLAN201
 network 172.16.201.0 255.255.255.0
 default-router 172.16.201.1
 dns-server 8.8.8.8
 lease 7
exit

ip dhcp excluded-address 172.16.202.1 172.16.202.10
ip dhcp pool VLAN202
 network 172.16.202.0 255.255.255.0
 default-router 172.16.202.1
 dns-server 8.8.8.8
 lease 7
exit

! OSPF configuration
router ospf 1
 router-id 172.16.220.3
 network 172.16.201.0 0.0.0.255 area 11
 network 172.16.202.0 0.0.0.255 area 11
 network 172.16.220.0 0.0.0.255 area 11
 network 192.168.1.28 0.0.0.31 area 11
 passive-interface default
 no passive-interface e0/2
 no passive-interface Vlan30
exit

! Save configuration
write memory
```

### Federateur-2

```bash
enable
configure terminal

! Basic security configuration
hostname Federateur-2
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! VLAN configuration
vlan 210
 name DATA1
exit
vlan 215
 name DATA2
exit
vlan 220
 name MANAGEMENT
exit
vlan 30
 name TRUNK
exit

! Enable IP routing
ip routing

! Interface to CE21
interface e0/2
 description Connection to CE21
 no switchport
 ip address 192.168.1.34 255.255.255.224
 no shutdown
exit

! Interface to SW-2
interface e0/3
 description Connection to SW-2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 210,215,220
 no shutdown
exit

! Interface to SW-1
interface e1/0
 description Connection to SW-1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 210,215,220
 no shutdown
exit

! EtherChannel configuration to Federateur-1
interface e0/0
 description Connection to Federateur-1 (Channel)
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 210,215,220,30
 channel-group 1 mode active
 no shutdown
exit

interface e0/1
 description Connection to Federateur-1 (Channel)
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 210,215,220,30
 channel-group 1 mode active
 no shutdown
exit

interface Port-channel1
 description EtherChannel to Federateur-1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 210,215,220,30
 no shutdown
exit

! VLAN interface configuration
interface Vlan210
 ip address 172.16.210.2 255.255.255.0
 standby 210 ip 172.16.210.1
 standby 210 priority 100
 no shutdown
exit

interface Vlan215
 ip address 172.16.215.2 255.255.255.0
 standby 215 ip 172.16.215.1
 standby 215 priority 100
 no shutdown
exit

interface Vlan220
 ip address 172.16.220.2 255.255.255.0
 standby 220 ip 172.16.220.1
 standby 220 priority 100
 no shutdown
exit

interface Vlan30
 description VLAN for inter-switch trunk
 ip address 192.168.1.45 255.255.255.224
 ip ospf cost 3000
 no shutdown
exit

! DHCP configuration
ip dhcp excluded-address 172.16.210.1 172.16.210.10
ip dhcp pool VLAN210
 network 172.16.210.0 255.255.255.0
 default-router 172.16.210.1
 dns-server 8.8.8.8
 lease 7
exit

ip dhcp excluded-address 172.16.215.1 172.16.215.10
ip dhcp pool VLAN215
 network 172.16.215.0 255.255.255.0
 default-router 172.16.215.1
 dns-server 8.8.8.8
 lease 7
exit

! OSPF configuration
router ospf 1
 router-id 172.16.220.2
 network 172.16.210.0 0.0.0.255 area 11
 network 172.16.215.0 0.0.0.255 area 11
 network 172.16.220.0 0.0.0.255 area 11
 network 192.168.1.32 0.0.0.31 area 11
 passive-interface default
 no passive-interface e0/2
 no passive-interface Vlan30
exit

! Save configuration
write memory
```

## Branch Switches

### SW-1

```bash
enable
configure terminal

! Basic security configuration
hostname SW-1
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! VLAN configuration
vlan 201
 name DATA1
exit
vlan 215
 name DATA2
exit
vlan 220
 name MANAGEMENT
exit

! Trunk port configuration to Federateur-2
interface e0/0
 description Connection to Federateur-2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 201,215,220
 no shutdown
exit

! Trunk port configuration to Federateur-1
interface e0/1
 description Connection to Federateur-1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 201,215,220
 no shutdown
exit

! Access port configuration for PC-6
interface e0/2
 description Connection to PC-6
 switchport mode access
 switchport access vlan 215
 spanning-tree portfast
 no shutdown
exit

! Management VLAN configuration
interface Vlan220
 ip address 172.16.220.101 255.255.255.0
 no shutdown
exit

! Default gateway
ip default-gateway 172.16.220.1

! Save configuration
write memory
```

### SW-2

```bash
enable
configure terminal

! Basic security configuration
hostname SW-2
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! VLAN configuration
vlan 201
 name DATA1
exit
vlan 210
 name DATA2
exit
vlan 220
 name MANAGEMENT
exit

! Trunk port configuration to Federateur-2
interface e0/0
 description Connection to Federateur-2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 201,210,220
 no shutdown
exit

! Trunk port configuration to Federateur-1
interface e0/1
 description Connection to Federateur-1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 220
 switchport trunk allowed vlan 201,210,220
 no shutdown
exit

! Access port configuration for PC-1
interface e0/2
 description Connection to PC-1
 switchport mode access
 switchport access vlan 201
 spanning-tree portfast
 no shutdown
exit

! Management VLAN configuration
interface Vlan220
 ip address 172.16.220.102 255.255.255.0
 no shutdown
exit

! Default gateway
ip default-gateway 172.16.220.1

! Save configuration
write memory
```

### SW-3

```bash
enable
configure terminal

! Basic security configuration
hostname SW-3
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! VLAN configuration
vlan 201
 name MANAGEMENT
exit
vlan 202
 name DATA
exit

! Trunk port configuration to CE12
interface e0/0
 description Connection to CE12
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 201
 switchport trunk allowed vlan 201,202
 no shutdown
exit

! Access port configuration for PC-3
interface e0/1
 description Connection to PC-3
 switchport mode access
 switchport access vlan 202
 spanning-tree portfast
 no shutdown
exit

! Management VLAN configuration
interface Vlan201
 ip address 172.16.201.100 255.255.255.0
 no shutdown
exit

! Default gateway
ip default-gateway 172.16.201.1

! Save configuration
write memory
```

### SW-4

```bash
enable
configure terminal

! Basic security configuration
hostname SW-4
username hafedh secret hafedh
enable secret hafedh
service password-encryption
no ip domain-lookup

! Console and VTY configuration
line console 0
 login local
 exec-timeout 5 0
exit
line vty 0 4
 login local
 transport input ssh
exit

! VLAN configuration
vlan 203
 name MANAGEMENT
exit
vlan 204
 name DATA
exit

! Trunk port configuration to CE22
interface e0/0
 description Connection to CE22
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 203
 switchport trunk allowed vlan 203,204
 no shutdown
exit

! Access port configuration for PC-4
interface e0/1
 description Connection to PC-4
 switchport mode access
 switchport access vlan 204
 spanning-tree portfast
 no shutdown
exit

! Management VLAN configuration
interface Vlan203
 ip address 172.16.203.100 255.255.255.0
 no shutdown
exit

! Default gateway
ip default-gateway 172.16.203.1

! Save configuration
write memory
```

## PC Configurations

### PC-1 Configuration

```bash
# PC-1 Configuration
# DHCP Configuration
ip dhcp
# Or static configuration:
# ip 172.16.201.20 255.255.255.0 172.16.201.1
# Set DNS server
ip name-server 8.8.8.8
```

### PC-0 Configuration

```bash
# PC-6 Configuration
# DHCP Configuration
ip dhcp
# Or static configuration:
# ip 172.16.215.20 255.255.255.0 172.16.215.1
# Set DNS server
ip name-server 8.8.8.8
```

### PC-3 Configuration

```bash
# PC-3 Configuration
# DHCP Configuration
ip dhcp
# Or static configuration:
# ip 172.16.202.20 255.255.255.0 172.16.202.1
# Set DNS server
ip name-server 8.8.8.8
```

### PC-2 Configuration

```bash
# PC-4 Configuration
# DHCP Configuration
ip dhcp
# Or static configuration:
# ip 172.16.204.20 255.255.255.0 172.16.204.1
# Set DNS server
ip name-server 8.8.8.8
```

## Verification Commands

```bash
! MPLS Backbone Verification
show mpls ldp neighbor
show mpls forwarding-table
show mpls ldp binding
show mpls interfaces
show ip ospf neighbor
show ip route ospf
show ip bgp summary
show ip bgp vpnv4 all summary

! VRF Verification
show ip vrf
show ip vrf interfaces
show ip route vrf VPN_SECURITY
show ip bgp vpnv4 vrf VPN_SECURITY

! Switch Verification
show vlan brief
show interface trunk
show etherchannel summary
show spanning-tree
show standby brief
show standby

! Connectivity Testing
ping vrf VPN_SECURITY 192.168.1.2
ping vrf VPN_SECURITY 192.168.1.6
ping vrf VPN_SECURITY 192.168.1.10
ping vrf VPN_SECURITY 192.168.1.14
ping 172.16.12.12 source 172.16.11.11
ping 172.16.22.22 source 172.16.21.21
traceroute 172.16.12.12 source 172.16.11.11
traceroute 172.16.22.22 source 172.16.21.21

! DHCP Verification
show ip dhcp binding
show ip dhcp pool

! Interface Status
show ip interface brief
show interfaces status
```
