```Cisco IOS
! --------------------------------------------
! ---------- CONFIGURATIONS DE BASE ----------
! --------------------------------------------

enable
configure terminal

hostname Switch5
banner motd #Banner#
no ip domain lookup
ip domain-name crosemontSw5.qc
lldp run
cdp run

end

! --------------------------------------------
! --------- CONFIGURATIONS DES VLANs ---------
! -------------------------------------------- 

enable
configure terminal

vlan 10
name VLAN-A
vlan 15
name VLAN-A-Voix
vlan 20
name VLAN-D
vlan 25
name VLAN-D-Voix
vlan 30
name VLAN-B
vlan 40
name VLAN-C
vlan 777
name Natif
vlan 888
name Gestion-distance
vlan 999
name Trou-noir

interface vlan 888
ip address 10.10.88.5 255.255.255.0
no shutdown

end

! --------------------------------------------
! --- CONFIGURATION D'UN DHCP RELAY AGENT ----
! -------------------------------------------- 

enable
configure terminal

interface vlan 10
ip helper-address 192.168.10.17
ip helper-address 192.168.10.21

interface vlan 15
ip helper-address 192.168.10.17
ip helper-address 192.168.10.21

interface vlan 20
ip helper-address 192.168.10.17
ip helper-address 192.168.10.21

interface vlan 25
ip helper-address 192.168.10.17
ip helper-address 192.168.10.21

interface vlan 30
ip helper-address 192.168.10.17
ip helper-address 192.168.10.21

interface vlan 40
ip helper-address 192.168.10.17
ip helper-address 192.168.10.21

end

! --------------------------------------------
! --------- CONFIGURATIONS DES PORTS ---------
! -------------------------------------------- 

enable
configure terminal

interface FastEthernet 0/1
description VLAN-A
switchport mode access
switchport access vlan 10
!mls qos trust cos
switchport voice vlan 15
lldp transmit
lldp receive
spanning-tree portfast
spanning-tree bpduguard enable
no shutdown

interface FastEthernet 0/2
description VLAN-D
switchport mode access
switchport access vlan 20
!mls qos trust cos
switchport voice vlan 25
lldp transmit
lldp receive
spanning-tree portfast
spanning-tree bpduguard enable
no shutdown

interface FastEthernet 0/3
description VLAN-B
switchport mode access
switchport access vlan 30
spanning-tree portfast
spanning-tree bpduguard enable
no shutdown

interface FastEthernet 0/4
description VLAN-C
switchport mode access
switchport access vlan 40
spanning-tree portfast
spanning-tree bpduguard enable
no shutdown

interface range FastEthernet 0/5-20
description Vide
switchport mode access
switchport access vlan 999
shutdown

interface range GigabitEthernet 0/1 - 2
description Vide
switchport mode access
switchport access vlan 999
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
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888
no shutdown

interface Port-channel3
!switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888
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
