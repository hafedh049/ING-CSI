## 📋 Network Overview

- **Backbone**: MPLS network with VRF SECURITY
- **Headquarters (Siège)**: CE11 (Siège1) and CE21 (Siège2) with redundant Layer 3 switches
- **Branch Offices**: CE12 (Branch1) and CE22 (Branch2) with Layer 2 switches
- **VLANs**: Multiple VLANs for data and management at each site
- **Redundancy**: HSRP/VRRP for gateway redundancy at headquarters
## 🔄 MP-BGP Configuration for VRF SECURITY

### PE1 Configuration

```c
PE1#conf t
! Create VRF SECURITY
PE1(config)# ip vrf VPN_SECURITY
PE1(config-vrf)# rd 100:3
PE1(config-vrf)# route-target export 100:3
PE1(config-vrf)# route-target import 100:3
PE1(config-vrf)# exit

! Remove existing VRF configurations
PE1(config)# no router ospf 100 vrf VPN_Customer1
PE1(config)# no router ospf 200 vrf VPN_Customer2

! Configure BGP for VRF SECURITY
PE1(config)# router bgp 100
PE1(config-router)# no bgp default ipv4-unicast
PE1(config-router)# neighbor 2.2.2.2 remote-as 100
PE1(config-router)# neighbor 2.2.2.2 update-source loopback 0
PE1(config-router)# address-family vpnv4 unicast
PE1(config-router-af)# neighbor 2.2.2.2 activate
PE1(config-router-af)# neighbor 2.2.2.2 send-community both
PE1(config-router-af)# exit-address-family

! Configure VRF address families
PE1(config-router)# address-family ipv4 vrf VPN_Customer1
PE1(config-router-af)# redistribute ospf 100 vrf VPN_Customer1
PE1(config-router-af)# exit-address-family

PE1(config-router)# address-family ipv4 vrf VPN_Customer2
PE1(config-router-af)# redistribute ospf 200 vrf VPN_Customer2
PE1(config-router-af)# exit-address-family

PE1(config-router)# address-family ipv4 vrf VPN_SECURITY
PE1(config-router-af)# redistribute ospf 300 vrf VPN_SECURITY
PE1(config-router-af)# exit-address-family
PE1(config-router)# exit

! Configure OSPF for VRF SECURITY
PE1(config)# router ospf 300 vrf VPN_SECURITY
PE1(config-router)# redistribute bgp 100 subnets
PE1(config-router)# network 192.168.1.0 0.0.0.3 area 11
PE1(config-router)# network 192.168.1.4 0.0.0.3 area 11
PE1(config-router)# exit
```

### PE2 Configuration

```c
PE2#conf t
! Create VRF SECURITY
PE2(config)# ip vrf VPN_SECURITY
PE2(config-vrf)# rd 100:3
PE2(config-vrf)# route-target export 100:3
PE2(config-vrf)# route-target import 100:3
PE2(config-vrf)# exit

! Remove existing VRF configurations
PE2(config)# no router ospf 100 vrf VPN_Customer1
PE2(config)# no router ospf 200 vrf VPN_Customer2

! Configure BGP for VRF SECURITY
PE2(config)# router bgp 100
PE2(config-router)# no bgp default ipv4-unicast
PE2(config-router)# neighbor 1.1.1.1 remote-as 100
PE2(config-router)# neighbor 1.1.1.1 update-source loopback 0
PE2(config-router)# address-family vpnv4 unicast
PE2(config-router-af)# neighbor 1.1.1.1 activate
PE2(config-router-af)# neighbor 1.1.1.1 send-community both
PE2(config-router-af)# exit-address-family

! Configure VRF address families
PE2(config-router)# address-family ipv4 vrf VPN_Customer1
PE2(config-router-af)# redistribute ospf 100 vrf VPN_Customer1
PE2(config-router-af)# exit-address-family

PE2(config-router)# address-family ipv4 vrf VPN_Customer2
PE2(config-router-af)# redistribute ospf 200 vrf VPN_Customer2
PE2(config-router-af)# exit-address-family

PE2(config-router)# address-family ipv4 vrf VPN_SECURITY
PE2(config-router-af)# redistribute ospf 300 vrf VPN_SECURITY
PE2(config-router-af)# exit-address-family
PE2(config-router)# exit

! Configure OSPF for VRF SECURITY
PE2(config)# router ospf 300 vrf VPN_SECURITY
PE2(config-router)# redistribute bgp 100 subnets
PE2(config-router)# network 192.168.1.8 0.0.0.3 area 12
PE2(config-router)# network 192.168.1.12 0.0.0.3 area 22
PE2(config-router)# exit
```

## 🏢 Headquarters (Siège) Configuration

### CE11 (Siège1) Configuration

```c
! Rename router
Router# configure terminal
Router(config)# hostname Siege1
Siege1(config)# enable secret Hafedh
Siege1(config)# line console 0
Siege1(config-line)# password Ibtihel
Siege1(config-line)# login
Siege1(config-line)# line vty 0 4
Siege1(config-line)# password Ibtihel
Siege1(config-line)# login
Siege1(config-line)# exit

! Configure interface to PE1
Siege1(config)# interface GigabitEthernet0/0/0
Siege1(config-if)# description Connection to PE1
Siege1(config-if)# ip address 192.168.1.2 255.255.255.252
Siege1(config-if)# no shutdown
Siege1(config-if)# exit

! Configure interface to Federateur1
Siege1(config)# interface GigabitEthernet1/0/1
Siege1(config-if)# description Connection to Federateur1
Siege1(config-if)# no shutdown
Siege1(config-if)# exit

! Configure OSPF
Siege1(config)# router ospf 1
Siege1(config-router)# network 192.168.1.0 0.0.0.3 area 11
Siege1(config-router)# network 172.16.210.0 0.0.0.255 area 11
Siege1(config-router)# network 172.16.215.0 0.0.0.255 area 11
Siege1(config-router)# network 172.16.220.0 0.0.0.255 area 11
Siege1(config-router)# exit
```

### CE21 (Siège2) Configuration

```c
! Rename router
Router# configure terminal
Router(config)# hostname Siege2
Siege2(config)# enable secret Hafedh
Siege2(config)# line console 0
Siege2(config-line)# password Ibtihel
Siege2(config-line)# login
Siege2(config-line)# line vty 0 4
Siege2(config-line)# password Ibtihel
Siege2(config-line)# login
Siege2(config-line)# exit

! Configure interface to PE1
Siege2(config)# interface GigabitEthernet0/0/0
Siege2(config-if)# description Connection to PE1
Siege2(config-if)# ip address 192.168.1.6 255.255.255.252
Siege2(config-if)# no shutdown
Siege2(config-if)# exit

! Configure interface to Federateur2
Siege2(config)# interface GigabitEthernet1/0/1
Siege2(config-if)# description Connection to Federateur2
Siege2(config-if)# no shutdown
Siege2(config-if)# exit

! Configure OSPF
Siege2(config)# router ospf 1
Siege2(config-router)# network 192.168.1.4 0.0.0.3 area 11
Siege2(config-router)# network 172.16.210.0 0.0.0.255 area 11
Siege2(config-router)# network 172.16.215.0 0.0.0.255 area 11
Siege2(config-router)# network 172.16.220.0 0.0.0.255 area 11
Siege2(config-router)# exit
```

### Federateur1 (Layer 3 Switch) Configuration

```c
! Basic configuration
Switch# configure terminal
Switch(config)# hostname Federateur1
Federateur1(config)# enable secret Hafedh
Federateur1(config)# line console 0
Federateur1(config-line)# password Ibtihel
Federateur1(config-line)# login
Federateur1(config-line)# line vty 0 15
Federateur1(config-line)# password Ibtihel
Federateur1(config-line)# login
Federateur1(config-line)# exit

! Enable IP routing
Federateur1(config)# ip routing

! Create VLANs
Federateur1(config)# vlan 210
Federateur1(config-vlan)# name DATA1
Federateur1(config-vlan)# exit
Federateur1(config)# vlan 215
Federateur1(config-vlan)# name DATA2
Federateur1(config-vlan)# exit
Federateur1(config)# vlan 220
Federateur1(config-vlan)# name Management
Federateur1(config-vlan)# exit
Federateur1(config)# vlan 300
Federateur1(config-vlan)# name InterSwitch
Federateur1(config-vlan)# exit

! Configure VLAN interfaces
Federateur1(config)# interface Vlan210
Federateur1(config-if)# ip address 172.16.210.2 255.255.255.0
Federateur1(config-if)# standby 210 ip 172.16.210.1
Federateur1(config-if)# standby 210 priority 110
Federateur1(config-if)# standby 210 preempt
Federateur1(config-if)# no shutdown
Federateur1(config-if)# exit

Federateur1(config)# interface Vlan215
Federateur1(config-if)# ip address 172.16.215.2 255.255.255.0
Federateur1(config-if)# standby 215 ip 172.16.215.1
Federateur1(config-if)# standby 215 priority 110
Federateur1(config-if)# standby 215 preempt
Federateur1(config-if)# no shutdown
Federateur1(config-if)# exit

Federateur1(config)# interface Vlan220
Federateur1(config-if)# ip address 172.16.220.2 255.255.255.0
Federateur1(config-if)# standby 220 ip 172.16.220.1
Federateur1(config-if)# standby 220 priority 110
Federateur1(config-if)# standby 220 preempt
Federateur1(config-if)# no shutdown
Federateur1(config-if)# exit

Federateur1(config)# interface Vlan300
Federateur1(config-if)# ip address 192.168.1.45 255.255.255.252
Federateur1(config-if)# ip ospf cost 3000
Federateur1(config-if)# no shutdown
Federateur1(config-if)# exit

! Configure interface to Siege1
Federateur1(config)# interface GigabitEthernet1/0/1
Federateur1(config-if)# description Connection to Siege1
Federateur1(config-if)# no switchport
Federateur1(config-if)# ip address 192.168.1.29 255.255.255.252
Federateur1(config-if)# no shutdown
Federateur1(config-if)# exit

! Configure EtherChannel to Federateur2
Federateur1(config)# interface range GigabitEthernet1/0/4-5
Federateur1(config-if-range)# description EtherChannel to Federateur2
Federateur1(config-if-range)# channel-group 1 mode on
Federateur1(config-if-range)# exit

Federateur1(config)# interface Port-channel1
Federateur1(config-if)# switchport mode trunk
Federateur1(config-if)# switchport trunk native vlan 220
Federateur1(config-if)# switchport trunk allowed vlan 210,215,220,300
Federateur1(config-if)# no shutdown
Federateur1(config-if)# exit

! Configure interfaces to SW1
Federateur1(config)# interface GigabitEthernet1/0/2
Federateur1(config-if)# description Connection to SW1
Federateur1(config-if)# switchport mode trunk
Federateur1(config-if)# switchport trunk native vlan 220
Federateur1(config-if)# switchport trunk allowed vlan 210,215,220
Federateur1(config-if)# no shutdown
Federateur1(config-if)# exit

! Configure DHCP service
Federateur1(config)# ip dhcp excluded-address 172.16.210.1 172.16.210.10
Federateur1(config)# ip dhcp excluded-address 172.16.215.1 172.16.215.10

Federateur1(config)# ip dhcp pool VLAN210
Federateur1(dhcp-config)# network 172.16.210.0 255.255.255.0
Federateur1(dhcp-config)# default-router 172.16.210.1
Federateur1(dhcp-config)# dns-server 8.8.8.8
Federateur1(dhcp-config)# exit

Federateur1(config)# ip dhcp pool VLAN215
Federateur1(dhcp-config)# network 172.16.215.0 255.255.255.0
Federateur1(dhcp-config)# default-router 172.16.215.1
Federateur1(dhcp-config)# dns-server 8.8.8.8
Federateur1(dhcp-config)# exit

! Configure OSPF
Federateur1(config)# router ospf 1
Federateur1(config-router)# passive-interface default
Federateur1(config-router)# no passive-interface Vlan300
Federateur1(config-router)# no passive-interface GigabitEthernet1/0/1
Federateur1(config-router)# network 172.16.210.0 0.0.0.255 area 11
Federateur1(config-router)# network 172.16.215.0 0.0.0.255 area 11
Federateur1(config-router)# network 172.16.220.0 0.0.0.255 area 11
Federateur1(config-router)# network 192.168.1.28 0.0.0.3 area 11
Federateur1(config-router)# network 192.168.1.44 0.0.0.3 area 11
Federateur1(config-router)# exit
```

### Federateur2 (Layer 3 Switch) Configuration

```c
! Basic configuration
Switch# configure terminal
Switch(config)# hostname Federateur2
Federateur2(config)# enable secret Hafedh
Federateur2(config)# line console 0
Federateur2(config-line)# password Ibtihel
Federateur2(config-line)# login
Federateur2(config-line)# line vty 0 15
Federateur2(config-line)# password Ibtihel
Federateur2(config-line)# login
Federateur2(config-line)# exit

! Enable IP routing
Federateur2(config)# ip routing

! Create VLANs
Federateur2(config)# vlan 210
Federateur2(config-vlan)# name DATA1
Federateur2(config-vlan)# exit
Federateur2(config)# vlan 215
Federateur2(config-vlan)# name DATA2
Federateur2(config-vlan)# exit
Federateur2(config)# vlan 220
Federateur2(config-vlan)# name Management
Federateur2(config-vlan)# exit
Federateur2(config)# vlan 300
Federateur2(config-vlan)# name InterSwitch
Federateur2(config-vlan)# exit

! Configure VLAN interfaces
Federateur2(config)# interface Vlan210
Federateur2(config-if)# ip address 172.16.210.3 255.255.255.0
Federateur2(config-if)# standby 210 ip 172.16.210.1
Federateur2(config-if)# standby 210 priority 100
Federateur2(config-if)# no shutdown
Federateur2(config-if)# exit

Federateur2(config)# interface Vlan215
Federateur2(config-if)# ip address 172.16.215.3 255.255.255.0
Federateur2(config-if)# standby 215 ip 172.16.215.1
Federateur2(config-if)# standby 215 priority 100
Federateur2(config-if)# no shutdown
Federateur2(config-if)# exit

Federateur2(config)# interface Vlan220
Federateur2(config-if)# ip address 172.16.220.3 255.255.255.0
Federateur2(config-if)# standby 220 ip 172.16.220.1
Federateur2(config-if)# standby 220 priority 100
Federateur2(config-if)# no shutdown
Federateur2(config-if)# exit

Federateur2(config)# interface Vlan300
Federateur2(config-if)# ip address 192.168.1.46 255.255.255.252
Federateur2(config-if)# ip ospf cost 3000
Federateur2(config-if)# no shutdown
Federateur2(config-if)# exit

! Configure interface to Siege2
Federateur2(config)# interface GigabitEthernet1/0/1
Federateur2(config-if)# description Connection to Siege2
Federateur2(config-if)# no switchport
Federateur2(config-if)# ip address 192.168.1.33 255.255.255.252
Federateur2(config-if)# no shutdown
Federateur2(config-if)# exit

! Configure EtherChannel to Federateur1
Federateur2(config)# interface range GigabitEthernet1/0/4-5
Federateur2(config-if-range)# description EtherChannel to Federateur1
Federateur2(config-if-range)# channel-group 1 mode on
Federateur2(config-if-range)# exit

Federateur2(config)# interface Port-channel1
Federateur2(config-if)# switchport mode trunk
Federateur2(config-if)# switchport trunk native vlan 220
Federateur2(config-if)# switchport trunk allowed vlan 210,215,220,300
Federateur2(config-if)# no shutdown
Federateur2(config-if)# exit

! Configure interfaces to SW2
Federateur2(config)# interface GigabitEthernet1/0/2
Federateur2(config-if)# description Connection to SW2
Federateur2(config-if)# switchport mode trunk
Federateur2(config-if)# switchport trunk native vlan 220
Federateur2(config-if)# switchport trunk allowed vlan 210,215,220
Federateur2(config-if)# no shutdown
Federateur2(config-if)# exit

! Configure DHCP service
Federateur2(config)# ip dhcp excluded-address 172.16.210.1 172.16.210.10
Federateur2(config)# ip dhcp excluded-address 172.16.215.1 172.16.215.10

Federateur2(config)# ip dhcp pool VLAN210
Federateur2(dhcp-config)# network 172.16.210.0 255.255.255.0
Federateur2(dhcp-config)# default-router 172.16.210.1
Federateur2(dhcp-config)# dns-server 8.8.8.8
Federateur2(dhcp-config)# exit

Federateur2(config)# ip dhcp pool VLAN215
Federateur2(dhcp-config)# network 172.16.215.0 255.255.255.0
Federateur2(dhcp-config)# default-router 172.16.215.1
Federateur2(dhcp-config)# dns-server 8.8.8.8
Federateur2(dhcp-config)# exit

! Configure OSPF
Federateur2(config)# router ospf 1
Federateur2(config-router)# passive-interface default
Federateur2(config-router)# no passive-interface Vlan300
Federateur2(config-router)# no passive-interface GigabitEthernet1/0/1
Federateur2(config-router)# network 172.16.210.0 0.0.0.255 area 11
Federateur2(config-router)# network 172.16.215.0 0.0.0.255 area 11
Federateur2(config-router)# network 172.16.220.0 0.0.0.255 area 11
Federateur2(config-router)# network 192.168.1.32 0.0.0.3 area 11
Federateur2(config-router)# network 192.168.1.44 0.0.0.3 area 11
Federateur2(config-router)# exit
```

### SW1 (Layer 2 Switch) Configuration

```c
! Basic configuration
Switch# configure terminal
Switch(config)# hostname SW1
SW1(config)# enable secret Hafedh
SW1(config)# line console 0
SW1(config-line)# password Ibtihel
SW1(config-line)# login
SW1(config-line)# line vty 0 15
SW1(config-line)# password Ibtihel
SW1(config-line)# login
SW1(config-line)# exit

! Create VLANs
SW1(config)# vlan 210
SW1(config-vlan)# name DATA1
SW1(config-vlan)# exit
SW1(config)# vlan 215
SW1(config-vlan)# name DATA2
SW1(config-vlan)# exit
SW1(config)# vlan 220
SW1(config-vlan)# name Management
SW1(config-vlan)# exit

! Configure management interface
SW1(config)# interface Vlan220
SW1(config-if)# ip address 172.16.220.101 255.255.255.0
SW1(config-if)# no shutdown
SW1(config-if)# exit
SW1(config)# ip default-gateway 172.16.220.1

! Configure trunk to Federateur1
SW1(config)# interface GigabitEthernet1/0/1
SW1(config-if)# description Uplink to Federateur1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk native vlan 220
SW1(config-if)# switchport trunk allowed vlan 210,215,220
SW1(config-if)# no shutdown
SW1(config-if)# exit

! Configure access ports for PCs
SW1(config)# interface GigabitEthernet1/0/2
SW1(config-if)# description PC0 - VLAN 210
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 210
SW1(config-if)# no shutdown
SW1(config-if)# exit

SW1(config)# interface GigabitEthernet1/0/3
SW1(config-if)# description PC1 - VLAN 215
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 215
SW1(config-if)# no shutdown
SW1(config-if)# exit
```

### SW2 (Layer 2 Switch) Configuration

```c
! Basic configuration
Switch# configure terminal
Switch(config)# hostname SW2
SW2(config)# enable secret Hafedh
SW2(config)# line console 0
SW2(config-line)# password Ibtihel
SW2(config-line)# login
SW2(config-line)# line vty 0 15
SW2(config-line)# password Ibtihel
SW2(config-line)# login
SW2(config-line)# exit

! Create VLANs
SW2(config)# vlan 210
SW2(config-vlan)# name DATA1
SW2(config-vlan)# exit
SW2(config)# vlan 215
SW2(config-vlan)# name DATA2
SW2(config-vlan)# exit
SW2(config)# vlan 220
SW2(config-vlan)# name Management
SW2(config-vlan)# exit

! Configure management interface
SW2(config)# interface Vlan220
SW2(config-if)# ip address 172.16.220.102 255.255.255.0
SW2(config-if)# no shutdown
SW2(config-if)# exit
SW2(config)# ip default-gateway 172.16.220.1

! Configure trunk to Federateur2
SW2(config)# interface GigabitEthernet1/0/1
SW2(config-if)# description Uplink to Federateur2
SW2(config-if)# switchport mode trunk
SW2(config-if)# switchport trunk native vlan 220
SW2(config-if)# switchport trunk allowed vlan 210,215,220
SW2(config-if)# no shutdown
SW2(config-if)# exit

! Configure access ports for PCs
SW2(config)# interface GigabitEthernet1/0/2
SW2(config-if)# description PC0 - VLAN 210
SW2(config-if)# switchport mode access
SW2(config-if)# switchport access vlan 210
SW2(config-if)# no shutdown
SW2(config-if)# exit

SW2(config)# interface GigabitEthernet1/0/3
SW2(config-if)# description PC1 - VLAN 215
SW2(config-if)# switchport mode access
SW2(config-if)# switchport access vlan 215
SW2(config-if)# no shutdown
SW2(config-if)# exit
```

## 🏢 Branch Offices Configuration

### CE12 (Branch1) Configuration

```c
! Rename router
Router# configure terminal
Router(config)# hostname Branch1
Branch1(config)# enable secret Hafedh
Branch1(config)# line console 0
Branch1(config-line)# password Ibtihel
Branch1(config-line)# login
Branch1(config-line)# line vty 0 4
Branch1(config-line)# password Ibtihel
Branch1(config-line)# login
Branch1(config-line)# exit

! Configure interface to PE2
Branch1(config)# interface GigabitEthernet0/0/0
Branch1(config-if)# description Connection to PE2
Branch1(config-if)# ip address 192.168.1.10 255.255.255.252
Branch1(config-if)# no shutdown
Branch1(config-if)# exit

! Configure subinterfaces for VLANs
Branch1(config)# interface GigabitEthernet0/0/1
Branch1(config-if)# description Connection to SW3
Branch1(config-if)# no ip address
Branch1(config-if)# no shutdown
Branch1(config-if)# exit

Branch1(config)# interface GigabitEthernet0/0/1.201
Branch1(config-subif)# description Management VLAN
Branch1(config-subif)# encapsulation dot1q 201 native
Branch1(config-subif)# ip address 172.16.201.1 255.255.255.0
Branch1(config-subif)# exit

Branch1(config)# interface GigabitEthernet0/0/1.202
Branch1(config-subif)# description Data VLAN
Branch1(config-subif)# encapsulation dot1q 202
Branch1(config-subif)# ip address 172.16.202.1 255.255.255.0
Branch1(config-subif)# exit

! Configure DHCP service
Branch1(config)# ip dhcp excluded-address 172.16.202.1 172.16.202.10

Branch1(config)# ip dhcp pool VLAN202
Branch1(dhcp-config)# network 172.16.202.0 255.255.255.0
Branch1(dhcp-config)# default-router 172.16.202.1
Branch1(dhcp-config)# dns-server 8.8.8.8
Branch1(dhcp-config)# exit

! Configure OSPF
Branch1(config)# router ospf 1
Branch1(config-router)# network 192.168.1.8 0.0.0.3 area 12
Branch1(config-router)# network 172.16.201.0 0.0.0.255 area 12
Branch1(config-router)# network 172.16.202.0 0.0.0.255 area 12
Branch1(config-router)# exit
```

### CE22 (Branch2) Configuration

```c
! Rename router
Router# configure terminal
Router(config)# hostname Branch2
Branch2(config)# enable secret Hafedh
Branch2(config)# line console 0
Branch2(config-line)# password Ibtihel
Branch2(config-line)# login
Branch2(config-line)# line vty 0 4
Branch2(config-line)# password Ibtihel
Branch2(config-line)# login
Branch2(config-line)# exit

! Configure interface to PE2
Branch2(config)# interface GigabitEthernet0/0/0
Branch2(config-if)# description Connection to PE2
Branch2(config-if)# ip address 192.168.1.14 255.255.255.252
Branch2(config-if)# no shutdown
Branch2(config-if)# exit

! Configure subinterfaces for VLANs
Branch2(config)# interface GigabitEthernet0/0/1
Branch2(config-if)# description Connection to SW4
Branch2(config-if)# no ip address
Branch2(config-if)# no shutdown
Branch2(config-if)# exit

Branch2(config)# interface GigabitEthernet0/0/1.203
Branch2(config-subif)# description Management VLAN
Branch2(config-subif)# encapsulation dot1q 203 native
Branch2(config-subif)# ip address 172.16.203.1 255.255.255.0
Branch2(config-subif)# exit

Branch2(config)# interface GigabitEthernet0/0/1.204
Branch2(config-subif)# description Data VLAN
Branch2(config-subif)# encapsulation dot1q 204
Branch2(config-subif)# ip address 172.16.204.1 255.255.255.0
Branch2(config-subif)# exit

! Configure DHCP service
Branch2(config)# ip dhcp excluded-address 172.16.204.1 172.16.204.10

Branch2(config)# ip dhcp pool VLAN204
Branch2(dhcp-config)# network 172.16.204.0 255.255.255.0
Branch2(dhcp-config)# default-router 172.16.204.1
Branch2(dhcp-config)# dns-server 8.8.8.8
Branch2(dhcp-config)# exit

! Configure OSPF
Branch2(config)# router ospf 1
Branch2(config-router)# network 192.168.1.12 0.0.0.3 area 22
Branch2(config-router)# network 172.16.203.0 0.0.0.255 area 22
Branch2(config-router)# network 172.16.204.0 0.0.0.255 area 22
Branch2(config-router)# exit
```

### SW3 (Layer 2 Switch at Branch1) Configuration

```c
! Basic configuration
Switch# configure terminal
Switch(config)# hostname SW3
SW3(config)# enable secret Hafedh
SW3(config)# line console 0
SW3(config-line)# password Ibtihel
SW3(config-line)# login
SW3(config-line)# line vty 0 15
SW3(config-line)# password Ibtihel
SW3(config-line)# login
SW3(config-line)# exit

! Create VLANs
SW3(config)# vlan 201
SW3(config-vlan)# name Management
SW3(config-vlan)# exit
SW3(config)# vlan 202
SW3(config-vlan)# name DATA
SW3(config-vlan)# exit

! Configure management interface
SW3(config)# interface Vlan201
SW3(config-if)# ip address 172.16.201.100 255.255.255.0
SW3(config-if)# no shutdown
SW3(config-if)# exit
SW3(config)# ip default-gateway 172.16.201.1

! Configure trunk to Branch1
SW3(config)# interface GigabitEthernet1/0/1
SW3(config-if)# description Uplink to Branch1
SW3(config-if)# switchport mode trunk
SW3(config-if)# switchport trunk native vlan 201
SW3(config-if)# switchport trunk allowed vlan 201,202
SW3(config-if)# no shutdown
SW3(config-if)# exit

! Configure access port for PC2
SW3(config)# interface GigabitEthernet1/0/2
SW3(config-if)# description PC2 - VLAN 202
SW3(config-if)# switchport mode access
SW3(config-if)# switchport access vlan 202
SW3(config-if)# no shutdown
SW3(config-if)# exit
```

### SW4 (Layer 2 Switch at Branch2) Configuration

```c
! Basic configuration
Switch# configure terminal
Switch(config)# hostname SW4
SW4(config)# enable secret Hafedh
SW4(config)# line console 0
SW4(config-line)# password Ibtihel
SW4(config-line)# login
SW4(config-line)# line vty 0 15
SW4(config-line)# password Ibtihel
SW4(config-line)# login
SW4(config-line)# exit

! Create VLANs
SW4(config)# vlan 203
SW4(config-vlan)# name Management
SW4(config-vlan)# exit
SW4(config)# vlan 204
SW4(config-vlan)# name DATA
SW4(config-vlan)# exit

! Configure management interface
SW4(config)# interface Vlan203
SW4(config-if)# ip address 172.16.203.100 255.255.255.0
SW4(config-if)# no shutdown
SW4(config-if)# exit
SW4(config)# ip default-gateway 172.16.203.1

! Configure trunk to Branch2
SW4(config)# interface GigabitEthernet1/0/1
SW4(config-if)# description Uplink to Branch2
SW4(config-if)# switchport mode trunk
SW4(config-if)# switchport trunk native vlan 203
SW4(config-if)# switchport trunk allowed vlan 203,204
SW4(config-if)# no shutdown
SW4(config-if)# exit

! Configure access port for PC3
SW4(config)# interface GigabitEthernet1/0/2
SW4(config-if)# description PC3 - VLAN 204
SW4(config-if)# switchport mode access
SW4(config-if)# switchport access vlan 204
SW4(config-if)# no shutdown
SW4(config-if)# exit
```

## 🔍 Verification Commands

### VRF SECURITY Verification

```bash
! On PE1 and PE2
show ip bgp vpnv4 vrf VPN_SECURITY
show ip route vrf VPN_SECURITY
```

### Connectivity Testing

```bash
! From Siege1 (CE11)
ping 172.16.12.12    ! Should reach Branch1 (CE12)
ping 172.16.21.21    ! Should reach Siege2 (CE21)
ping 172.16.22.22    ! Should reach Branch2 (CE22)

! From PC0 at Siege1
ping 172.16.210.1    ! Should reach HSRP gateway
ping 172.16.202.1    ! Should reach Branch1 gateway
ping 172.16.204.1    ! Should reach Branch2 gateway
```