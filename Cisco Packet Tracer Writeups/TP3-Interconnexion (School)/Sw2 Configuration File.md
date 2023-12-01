```Cisco IOS
! --------------------------------------------
! ---------- CONFIGURATIONS DE BASE ----------
! --------------------------------------------

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! DONNER UN HOSTNAME
hostname Switch2

! CREER UNE BANNIERE
banner motd #Banner#

! empty
no ip domain lookup

! empty
ip domain-name crosemontSw2.qc

! empty
lldp run
cdp run

! SORTIR DU PRIVILEGED EXEC MODE
end

! --------------------------------------------
! -------- CONFIGURATIONS DU ROUTAGE ---------
! --------------------------------------------

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! empty
ip routing

! SORTIR DE LA LIGNE DE CONFIGURATION DU VLAN
end

! --------------------------------------------
! --------- CONFIGURATIONS DES VLANs ---------
! -------------------------------------------- 

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! CREER LES VLANs DE DATA
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

! CREER LE VLAN NATIF
vlan 777
name Natif

! CREER UN VLAN POUR LA GESTION A DISTANCE
vlan 888
name Gestion-distance

! CREER UN VLAN POUR LE TROU NOIR
vlan 999
name Trou-noir

! empty
interface vlan 888
ip address 10.10.88.2 255.255.255.0
no shutdown

! SORTIR DU MODE DE CONFIGURATION GLOBALE 
end

! --------------------------------------------
! --- CONFIGURATION D'UN DHCP RELAY AGENT ----
! -------------------------------------------- 

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! empty
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

! SORTIR DU MODE DE CONFIGURATION GLOBALE 
end

! --------------------------------------------
! --------- CONFIGURATIONS DES PORTS ---------
! -------------------------------------------- 

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! OUVRIR LES PORTS DONT ON A BESOIN
interface GigabitEthernet 0/1
no switchport
description Vers-Sw1
no shutdown

! FERMER LES PORTS DONT ON N'A PAS BESOIN
interface range FastEthernet 0/1 - 22
description Vide
switchport mode access
switchport access vlan 999
shutdown

interface range GigabitEthernet 0/2
description Vide
switchport mode access
switchport access vlan 999
shutdown

! SORTIR DE LA LIGNE DE CONFIGURATION DU VLAN
end

! --------------------------------------------
! ---------- CONFIGURATIONS DU HSRP ----------
! -------------------------------------------- 

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! empty
interface vlan 10
ip address 10.10.10.2 255.255.255.0
standby 10 ip 10.10.10.1
standby 10 priority 101
standby 10 preempt
no shutdown
interface vlan 15
ip address 10.10.15.2 255.255.255.0
standby 15 ip 10.10.15.1
standby 15 priority 101
standby 15 preempt
no shutdown

! empty
interface vlan 20
ip address 10.10.20.2 255.255.255.0
standby 20 ip 10.10.20.1
standby 20 priority 101
standby 20 preempt
no shutdown
interface vlan 25
ip address 10.10.25.2 255.255.255.0
standby 25 ip 10.10.25.1
standby 25 priority 101
standby 25 preempt
no shutdown

! empty
interface vlan 30
ip address 10.10.30.2 255.255.255.0
standby 30 ip 10.10.30.1
standby 30 priority 101
standby 30 preempt
no shutdown

! empty
interface vlan 40
ip address 10.10.40.2 255.255.255.0
standby 40 ip 10.10.40.1
standby 40 priority 101
standby 40 preempt
no shutdown

! empty
interface vlan 888
ip address 10.10.88.2 255.255.255.0
standby 88 ip 10.10.88.1
standby 88 priority 101
standby 88 preempt
no shutdown

! SORTIR DE LA LIGNE DE CONFIGURATION DU VLAN
end

! --------------------------------------------
! ------ CONFIGURATIONS DU ETHERCHANNEL ------
! --------------------------------------------

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! empty
interface range FastEthernet 0/23 - 24
channel-group 2 mode active

! empty
interface Port-channel2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888
no shutdown

! SORTIR DE LA LIGNE DE CONFIGURATION DU ETHERCHANNEL
end

! --------------------------------------------
! -------- CONFIGURATIONS DE SECURITE --------
! --------------------------------------------

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! CONFIGURER UN MOT DE PASSE POUR LE PRIVILEGED EXEC MODE
enable secret crosemont

! CONFIGURER UN MOT DE PASSE POUR USER EXEC MODE
line console 0
password crosemont
login
exit

! CONFIGURER UN MOT DE PASSE POUR LE MODE AUXILIAIRE
line aux 0
password crosemont
login
exit

! CONFIGURER L'ACCESS A DISTANCE
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

! --------------------------------------------
! --------------- CREDENTIALS ----------------
! --------------------------------------------

! NORTEL IP PHONE -> color*set
! SWITCH ----------> crosemont
```
