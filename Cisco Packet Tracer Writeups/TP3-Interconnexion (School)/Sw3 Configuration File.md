```Cisco IOS
! --------------------------------------------
! ---------- CONFIGURATIONS DE BASE ----------
! --------------------------------------------

enable
configure terminal

hostname Switch3
banner motd #Banner#
no ip domain lookup
ip domain-name crosemontSw3.qc
lldp run
cdp run

end

! --------------------------------------------
! -------- CONFIGURATIONS DU ROUTAGE ---------
! --------------------------------------------

enable
configure terminal

ip routing

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
ip address 10.10.88.3 255.255.255.0
no shutdown

end

! --------------------------------------------
! --- CONFIGURATION D'UN DHCP RELAY AGENT ----
! -------------------------------------------- 

enable
configure terminal

interface vlan 10
ip helper-address 10.10.10.10

interface vlan 15
ip helper-address 10.10.15.10

! empty
interface vlan 20
ip helper-address 10.10.20.10
interface vlan 25
ip helper-address 10.10.25.10

interface vlan 30
ip helper-address 10.10.30.10

interface vlan 40
ip helper-address 10.10.40.10

end

! --------------------------------------------
! --------- CONFIGURATIONS DES PORTS ---------
! -------------------------------------------- 

enable
configure terminal

interface GigabitEthernet 0/1
no switchport
description Vers-Sw1
ip address 192.168.10.22 255.255.255.252
no shutdown

interface range FastEthernet 0/1 - 20
description Vide
switchport mode access
switchport access vlan 999
shutdown

interface range FastEthernet 0/23 - 24
description Vide
switchport mode access
switchport access vlan 999
shutdown

interface range GigabitEthernet 0/2
description Vide
switchport mode access
switchport access vlan 999
shutdown

end

! --------------------------------------------
! ---------- CONFIGURATIONS DU HSRP ----------
! -------------------------------------------- 

enable
configure terminal

interface vlan 10
ip address 10.10.10.3 255.255.255.0
standby 10 ip 10.10.10.1
standby 10 priority 100
standby 10 preempt
no shutdown

interface vlan 15
ip address 10.10.15.3 255.255.255.0
standby 15 ip 10.10.15.1
standby 15 priority 100
standby 15 preempt
no shutdown

interface vlan 20
ip address 10.10.20.3 255.255.255.0
standby 20 ip 10.10.20.1
standby 20 priority 100
standby 20 preempt
no shutdown

interface vlan 25
ip address 10.10.25.3 255.255.255.0
standby 25 ip 10.10.25.1
standby 25 priority 100
standby 25 preempt
no shutdown

interface vlan 30
ip address 10.10.30.3 255.255.255.0
standby 30 ip 10.10.30.1
standby 30 priority 100
standby 30 preempt
no shutdown

interface vlan 40
ip address 10.10.40.3 255.255.255.0
standby 40 ip 10.10.40.1
standby 40 priority 100
standby 40 preempt
no shutdown

interface vlan 888
ip address 10.10.88.3 255.255.255.0
standby 88 ip 10.10.88.1
standby 88 priority 100
standby 88 preempt
no shutdown

end

! --------------------------------------------
! ------ CONFIGURATIONS DU ETHERCHANNEL ------
! --------------------------------------------

enable
configure terminal

interface range FastEthernet 0/21 - 22
channel-group 3 mode active

interface Port-channel3
switchport trunk encapsulation dot1q
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

line aux 0
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
