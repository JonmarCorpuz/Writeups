```Cisco IOS

! --------------------------------------------
! ---------- CONFIGURATIONS DE BASE ----------
! --------------------------------------------

enable
configure terminal

hostname Switch1
banner motd #Banner#
no ip domain lookup
ip domain-name crosemontSw1.qc
lldp run
cdp run

! SORTIR DU PRIVILEGED EXEC MODE
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

interface vlan 10
ip address 10.10.10.1 255.255.255.0
no shutdown
interface vlan 15
ip address 10.10.15.1 255.255.255.0
no shutdown

! empty
interface vlan 20
ip address 10.10.20.1 255.255.255.0
no shutdown
interface vlan 25
ip address 10.10.25.1 255.255.255.0
no shutdown

interface vlan 30
ip address 10.10.30.1 255.255.255.0
no shutdown

interface vlan 40
ip address 10.10.40.1 255.255.255.0
no shutdown

interface vlan 888
ip address 10.10.88.1 255.255.255.0
no shutdown

end

! --------------------------------------------
! ------ CONFIGURATIONS DES POOLS DHCP -------
! -------------------------------------------- 

enable
configure terminal

ip dhcp pool VLAN-A
network 10.10.10.0 255.255.255.0
default-router 10.10.10.10
ip dhcp excluded-address 10.10.10.1 10.10.10.10

ip dhcp pool VLAN-D
network 10.10.20.0 255.255.255.0
default-router 10.10.20.10
ip dhcp excluded-address 10.10.20.1 10.10.20.10

ip dhcp pool VLAN-B
network 10.10.30.0 255.255.255.0
default-router 10.10.30.10
ip dhcp excluded-address 10.10.30.1 10.10.30.10

ip dhcp pool VLAN-C
network 10.10.40.0 255.255.255.0
default-router 10.10.40.10
ip dhcp excluded-address 10.10.40.1 10.10.40.10

ip dhcp pool VLAN-A-Voix
network 10.10.15.0 255.255.255.0
default-router 10.10.15.10
ip dhcp excluded-address 10.10.15.1 10.10.15.10

ip dhcp pool VLAN-D-Voix
network 10.10.25.0 255.255.255.0
default-router 10.10.25.10
ip dhcp excluded-address 10.10.25.1 10.10.25.10

end

! --------------------------------------------
! -------- CONFIGURATIONS DES PORTS ----------
! -------------------------------------------- 

enable
configure terminal

interface GigabitEthernet 0/1
no shutdown
description Vers-R2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888
no switchport
ip address 192.168.10.2 255.255.255.0
ip route 0.0.0.0 0.0.0.0 192.168.10.1 5

no shutdown
description Vers-R1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888
no switchport
ip address 192.168.20.2 255.255.255.0
ip route 0.0.0.0 0.0.0.0 192.168.20.1 10

description Vers-Sw2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888
no shutdown

description Vers-Sw3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888
no shutdown

interface range FastEthernet 0/3 - 24
description Vide
switchport mode access
switchport access vlan 999
shutdown

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
