# Repo for project in IMT2007

**Keep in mind that the Packet Tracer .pkt file is binary, so be very careful about merging. Ideally "merges" should be done manually.**

Currently only planned to contain Packet Tracer implementation. 

## Head Quarters implementation

### Useful links

* video about access layer: https://www.youtube.com/watch?v=LZZLRwzKOAk

### Notes

* Remember `nonegotiate`
* Remember acces-lists on access layer

### Questions

* use "Hot Standby Router Protocol"?

## Learning Centre implementation

### Notes

* Chose "ISR4321" router only because it seems to support VPN site-to-site commands (not fully tested yet)
* Network equipment names are prefixed with `LC_` to indicate that they belong in the learning centre.
* Installed module for Gigabit Ethernet on switch
* Writing user-defined names in upper case to show they are user-defined
* Assuming we will never have more than 254 in any of our subnets, 192.168.x.0/24 is a convenient and pretty subnet 
    * 244 if excluding 1 to 10 from dhcp
    * **FOR SIMPLICITY**
* In this setup only guests cannot get a wired connection. They will have to rely on Wi-Fi
* Remember to use cross-over between switches..
* About `WLC_1`
    * `LC_WLC1` given static IP of 192.168.11.2
    * Chose "WLC2504" because it has a console interface
    * Username/password for WLC1: cisco/NTNUiG1
    * Passphrase for "staff" WLAN: ciscocisco
    * Currently staff network is in management VLAN (?)
    * Virtual IP address set to "192.0.2.1". (default)
    * Currently the configuration for the WLC is not stored here :(
    * Should have "service port" and "distribution port" like in book at page 235

### Questions

* How to handle DHCP when not on router?
* How to handle NAT? (left unconfigured right now)
* How to store config for WLC??
* Redundant WLC ports? (or only for HQ?)

### TODO

* Configure WAN to HQ
* Set hostnames
* Port security
* Make WLANs belong to correct VLAN (, configure trunking on switch connecting APs?)

### Config

gateway router (edge? idk)
```cisco
ip dhcp excluded-address 192.168.11.1 192.168.11.10
ip dhcp excluded-address 192.168.12.1 192.168.12.10
ip dhcp excluded-address 192.168.13.1 192.168.13.10
ip dhcp excluded-address 192.168.14.1 192.168.14.10
ip dhcp excluded-address 192.168.15.1 192.168.15.10

ip dhcp pool VLAN_MANAGEMENT
 network 192.168.11.0 255.255.255.0
 default-router 192.168.11.1
ip dhcp pool VLAN_GUEST
 network 192.168.12.0 255.255.255.0
 default-router 192.168.12.1
ip dhcp pool VLAN_STAFF
 network 192.168.13.0 255.255.255.0
 default-router 192.168.13.1
ip dhcp pool VLAN_LAB
 network 192.168.14.0 255.255.255.0
 default-router 192.168.14.1
ip dhcp pool VLAN_DMZ
 network 192.168.15.0 255.255.255.0
 default-router 192.168.15.1


interface GigabitEthernet0/0/0.11
 encapsulation dot1Q 11
 ip address 192.168.11.1 255.255.255.0
interface GigabitEthernet0/0/0.12
 encapsulation dot1Q 12
 ip address 192.168.12.1 255.255.255.0
interface GigabitEthernet0/0/0.13
 encapsulation dot1Q 13
 ip address 192.168.13.1 255.255.255.0
interface GigabitEthernet0/0/0.14
 encapsulation dot1Q 14
 ip address 192.168.14.1 255.255.255.0
interface GigabitEthernet0/0/0.15
 encapsulation dot1Q 15
 ip address 192.168.15.1 255.255.255.0
```

switch S1
```cisco
vlan 11
 name MANAGEMENT
vlan 12
 name GUEST
vlan 13
 name STAFF
vlan 14
 name LAB
vlan 15
 name DMZ

interface GigabitEthernet9/1
 switchport trunk allowed vlan 11-15
 switchport mode trunk
interface FastEthernet0/1
 switchport trunk allowed vlan 11-15
 switchport mode trunk
interface FastEthernet1/1
 switchport access vlan 14
 switchport mode access
interface FastEthernet2/1
 switchport access vlan 15
 switchport mode access
! ABOVE: only ever need one port...
! BELOW: might need more ports in the future, will let grown from 
!        left to right, and right to left
interface FastEthernet3/1
 switchport access vlan 12
 switchport mode access
interface FastEthernet8/1
 switchport access vlan 13
 switchport mode access
! Following are the ports that are set to not trunk, and shutdown
! TODO: port security
interface FastEthernet4/1
 switchport mode access
 shutdown
interface FastEthernet5/1
 switchport mode access
 shutdown
interface FastEthernet6/1
 switchport mode access
 shutdown
interface FastEthernet7/1
 switchport mode access
 shutdown
```

switch S2
```cisco
! IN MY MIND I SHOULD'NT HAVE TO DO THIS. But if I leave it out I 
! get "VLAN mismatch"
interface FastEthernet0/1
 switchport access vlan 13
interface FastEthernet1/1
 switchport access vlan 13
interface FastEthernet2/1
 switchport access vlan 13
interface FastEthernet3/1
 switchport access vlan 13
interface FastEthernet4/1
 switchport access vlan 13
interface FastEthernet5/1
 switchport access vlan 13
interface FastEthernet6/1
 switchport access vlan 13
interface FastEthernet7/1
 switchport access vlan 13
interface FastEthernet8/1
 switchport access vlan 13
```


switch S3
(same as S2, but with VLAN 14)
```cisco
! IN MY MIND I SHOULD'NT HAVE TO DO THIS. But if I leave it out I 
! get "VLAN mismatch"
interface FastEthernet0/1
 switchport access vlan 14
interface FastEthernet1/1
 switchport access vlan 14
interface FastEthernet2/1
 switchport access vlan 14
interface FastEthernet3/1
 switchport access vlan 14
interface FastEthernet4/1
 switchport access vlan 14
interface FastEthernet5/1
 switchport access vlan 14
interface FastEthernet6/1
 switchport access vlan 14
interface FastEthernet7/1
 switchport access vlan 14
interface FastEthernet8/1
 switchport access vlan 14
```

switch S4
(same as S2, but with VLAN 11)
```cisco
! IN MY MIND I SHOULD'NT HAVE TO DO THIS. But if I leave it out I 
! get "VLAN mismatch"
interface FastEthernet0/1
 switchport trunk allowed vlan 11-15
 switchport mode trunk

! WLC 
interface FastEthernet1/1
 switchport trunk allowed vlan 11-15
 switchport mode trunk
 switchport trunk native vlan 11

! LAPTOP
interface FastEthernet2/1
 switchport access vlan 11
 switchport mode access


! NOT USED
interface FastEthernet3/1
 switchport trunk allowed vlan 11-15
 switchport mode trunk
interface FastEthernet4/1
 switchport trunk allowed vlan 11-15
 switchport mode trunk
interface FastEthernet5/1
 switchport trunk allowed vlan 11-15
 switchport mode trunk
interface FastEthernet5/1
 switchport trunk allowed vlan 11-15
 switchport mode trunk
interface FastEthernet6/1
 switchport trunk allowed vlan 11-15
 switchport mode trunk

! AP
interface FastEthernet7/1
 switchport access vlan 11
 switchport mode access

! AP
interface FastEthernet8/1
 switchport access vlan 11
 switchport mode access

! AP
interface FastEthernet9/1
 switchport access vlan 11
 switchport mode access
```
