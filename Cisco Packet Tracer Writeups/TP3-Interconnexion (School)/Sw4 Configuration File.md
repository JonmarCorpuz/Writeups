```Cisco IOS
! --------------------------------------------
! ---------- CONFIGURATIONS DE BASE ----------
! --------------------------------------------

enable
configure terminal

hostname Switch4
banner motd #Banner#
no ip domain lookup
ip domain-name crosemontSw4.qc
lldp run
cdp run

end

! --------------------------------------------
! --------- CONFIGURATIONS DES VLANs ---------
! -------------------------------------------- 

enable
configure terminal

vlan 50
name VLAN-E
vlan 55
name VLAN-E-Voix
vlan 60
name VLAN-H
vlan 65
name VLAN-H-Voix
vlan 70
name VLAN-G
vlan 80
name VLAN-F
vlan 333
name Natif
vlan 444
name Gestion-distance
vlan 555
name Trou-noir
interface vlan 444
ip address 10.10.44.4 255.255.255.0
no shutdown

end

! --------------------------------------------
! --- CONFIGURATION D'UN DHCP RELAY AGENT ----
! -------------------------------------------- 

enable
configure terminal

interface vlan 50
ip helper-address 192.168.10.13

interface vlan 55
ip helper-address 192.168.10.13

interface vlan 60
ip helper-address 192.168.10.13

interface vlan 65
ip helper-address 192.168.10.13

interface vlan 70
ip helper-address 192.168.10.13

interface vlan 80
ip helper-address 192.168.10.13

end

! --------------------------------------------
! -------- CONFIGURATIONS DES PORTS ----------
! -------------------------------------------- 

enable
configure terminal

interface GigabitEthernet 0/1
description Vers-R3
ip address 192.168.10.26 255.255.255.252
no switchport
no shutdown

interface range FastEthernet 0/1 - 22
description Vide
switchport mode access
switchport access vlan 555
shutdown

interface GigabitEthernet 0/2
description Vide
switchport mode access
switchport access vlan 555
shutdown

end

! --------------------------------------------
! ---------- CONFIGURATIONS DU HSRP ----------
! -------------------------------------------- 

enable
configure terminal

interface vlan 50
ip address 10.10.50.2 255.255.255.0
standby 50 ip 10.10.50.1
standby 50 priority 101
standby 50 preempt
no shutdown

interface vlan 55
ip address 10.10.55.2 255.255.255.0
standby 55 ip 10.10.55.1
standby 55 priority 101
standby 55 preempt
no shutdown

interface vlan 60
ip address 10.10.60.2 255.255.255.0
standby 60 ip 10.10.60.1
standby 60 priority 101
standby 60 preempt
no shutdown

interface vlan 65
ip address 10.10.65.2 255.255.255.0
standby 65 ip 10.10.65.1
standby 65 priority 101
standby 65 preempt
no shutdown

interface vlan 70
ip address 10.10.70.2 255.255.255.0
standby 70 ip 10.10.70.1
standby 70 priority 101
standby 70 preempt
no shutdown

interface vlan 80
ip address 10.10.80.2 255.255.255.0
standby 80 ip 10.10.80.1
standby 80 priority 101
standby 80 preempt
no shutdown

interface vlan 444
ip address 10.10.44.2 255.255.255.0
standby 44 ip 10.10.44.1
standby 44 priority 101
standby 44 preempt
no shutdown

end

! --------------------------------------------
! ------ CONFIGURATIONS DU ETHERCHANNEL ------
! --------------------------------------------

enable
configure terminal

interface range FastEthernet 0/23 - 24
channel-group 2 mode active

interface Port-channel2
switchport trunk encapsulation dot1q
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
