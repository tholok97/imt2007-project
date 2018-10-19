# Repo for project in IMT2007

Currently only planned to contain Packet Tracer implementation.

## Learning Centre implementation

### Notes

* Chose "ISR4321" router only because it seems to support VPN site-to-site commands (not fully tested yet)
* Network equipment names are prefixed with `LC_` to indicate that they belong in the learning centre.
* Installed module for Gigabit Ethernet on switch
* Writing user-defined names in upper case to show they are user-defined
* Assuming we will never have more than 254 in any of our subnets, 192.168.x.0/24 is a convenient and pretty subnet 
    * 244 if excluding 1 to 10 from dhcp
    * **FOR SIMPLICITY**

### Questions

* How to handle DHCP when not on router?
* How to handle NAT? (left unconfigured right now)

### TODO

* Configure WAN to HQ
* Set hostnames
* Port security

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

top switch
```cisco
interface GigabitEthernet9/1
 switchport trunk allowed vlan 11-13
 switchport mode trunk
interface FastEthernet0/1
 switchport access vlan 11
interface FastEthernet1/1
 switchport access vlan 14
interface FastEthernet2/1
 switchport access vlan 15
! ABOVE: only ever need one port...
! BELOW: might need more ports in the future, will let grown from 
!        left to right, and right to left
interface FastEthernet3/1
 switchport access vlan 12
interface FastEthernet8/1
 switchport access vlan 13
! Following are the ports that are set to not trunk, and shutdown
! TODO: port security
exit
interface range FastEthernet4/1 - FastEthernet 7/1
 switchport mode access
 shutdown
```
