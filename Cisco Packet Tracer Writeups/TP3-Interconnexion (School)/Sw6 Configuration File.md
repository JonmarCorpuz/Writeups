```Cisco IOS
! --------------------------------------------
! ---------- CONFIGURATIONS DE BASE ----------
! --------------------------------------------

enable
configure terminal

hostname Switch6
banner motd #Banner#
no ip domain lookup
ip domain-name crosemontSw6.qc
lldp run
cdp run

end

! --------------------------------------------
! --------- CONFIGURATIONS DES VLANs ---------
! -------------------------------------------- 

enable
configure terminal

vlan 50
name VLAN-A
vlan 55
name VLAN-A-Voix
vlan 60
name VLAN-D
vlan 65
name VLAN-D-Voix
vlan 70
name VLAN-B
vlan 80
name VLAN-C
vlan 333
name Natif
vlan 444
name Gestion-distance
vlan 555
name Trou-noir

interface vlan 444
ip address 10.10.44.6 255.255.255.0
no shutdown

end

! --------------------------------------------
! --- CONFIGURATION D'UN DHCP RELAY AGENT ----
! -------------------------------------------- 

enable
configure terminal

interface vlan 50
ip helper-address 10.10.50.10

interface vlan 55
ip helper-address 10.10.55.10

! empty
interface vlan 60
ip helper-address 10.10.60.10

interface vlan 65
ip helper-address 10.10.65.10

interface vlan 70
ip helper-address 10.10.70.10

interface vlan 80
ip helper-address 10.10.80.10

end

! --------------------------------------------
! --------- CONFIGURATIONS DES PORTS ---------
! -------------------------------------------- 

enable
configure terminal

interface FastEthernet 0/1
description VLAN-H
switchport mode access
switchport access vlan 50
!mls qos trust cos
switchport voice vlan 55
lldp transmit
lldp receive
spanning-tree portfast
spanning-tree bpduguard enable
no shutdown

interface FastEthernet 0/2
description VLAN-E
switchport mode access
switchport access vlan 60
!mls qos trust cos
switchport voice vlan 65
lldp transmit
lldp receive
spanning-tree portfast
spanning-tree bpduguard enable
no shutdown

interface FastEthernet 0/3
description VLAN-G
switchport mode access
switchport access vlan 70
spanning-tree portfast
spanning-tree bpduguard enable
no shutdown

interface FastEthernet 0/4
description VLAN-F
switchport mode access
switchport access vlan 80
spanning-tree portfast
spanning-tree bpduguard enable
no shutdown

interface range FastEthernet 0/5 - 20
description Vide
switchport mode access
switchport access vlan 555
shutdown

interface range GigabitEthernet 0/1 - 2
description Vide
switchport mode access
switchport access vlan 555
shutdown

end

! --------------------------------------------
! ------ CONFIGURATIONS DU ETHERCHANNEL ------
! --------------------------------------------

enable
configure terminal

interface range FastEthernet 0/21 - 22
channel-group 3 mode passive

interface range FastEthernet 0/23 - 24
channel-group 2 mode passive

interface Port-channel2
!switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 333
switchport trunk allowed vlan 50,55,60,65,70,80,333,444
no shutdown

interface Port-channel3
!switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 333
switchport trunk allowed vlan 50,55,60,65,70,80,333,444
no shutdown

end

! --------------------------------------------
! -------- CONFIGURATIONS DE SECURITE --------
! --------------------------------------------

enable
configure terminal

enable secret crosemont

line console 0
password crosemont
login
exit

username Admin privilege 15 secret crosemont
crypto key generate rsa general-keys modulus 2048
line vty 0 15
password crosemont
login
transport input ssh
ip ssh version 2
exit

configure terminal
service password-encryption

! --------------------------------------------
! ------ SAUVEGARDER LES CONFIGURATIONS ------
! --------------------------------------------

end
write
```
