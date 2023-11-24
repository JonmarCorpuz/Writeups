# Checklist

## Configuration de base
- [x] Hostname
- [x] Bannière
- [x] Créer un nom de domaine

## Configuration des ports
- [x] Portfast
- [x] BPDU Guard
- [x] Activation des ports utilisés
- [x] Désactivation des ports non utilisés
- [x] Description des ports
- [x] Laisser que les VLANs necéssaire passer pour chaque port (trunk)
- [x] LLDP est activé
- [x] LLDP est capable de transmit
- [x] CDP est activé
- [x] EtherChannel

## Configuration des VLANs
- [x] Une interface de gestion qui peut être accessible par SSH

## Configuration des adresses IP
- [x] PCs obtiennent leurs adresses de manière dynamique
- [x] Téléphones IP obtiennent leurs adresses de manière dynamique
- [x] Toutes les passerelles sont configurées sur une interface VLAN
- [x] Création d'un serveur DHCP pour chaque batiment
- [x] Création d'un pool DHCP pour les VLANs de voix
- [x] Création d'un pool DHCP pour les VLANs de données
- [x] Activation des DHCP Relay Agent (`ip helper-address`)

## Configuration de sécurité
- [x] Mot de passe pour le User EXEC Mode (`enable`)
- [x] Mot de passe pour le Privileged EXEC Mode (`configure terminal`)
- [x] Mot de passe pour l'accès à distance (`ssh`)
- [x] Encrypter les mots de passes dans le fichier de configuration
- [ ] Apprentissage des adresses MAC automatique
- [ ] Maximum de deux adresses MAC par port
- [ ] Bloquer les trames non autorisées et consigner les événements dans le journal

## Configuration du routage
- [ ] Capable de `ping` toutes les machines sans problèmes

# Fichier de configuration
```Cisco IOS
! --------------------------------------------
! ---------- CONFIGURATIONS DE BASE ----------
! --------------------------------------------

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! DONNER UN HOSTNAME
hostname Switch6

! CREER UNE BANNIERE
banner motd #Banner#

! empty
no ip domain lookup

! empty
ip domain-name crosemontSw6.qc

! empty
lldp run
cdp run

! SORTIR DU PRIVILEGED EXEC MODE
end

! --------------------------------------------
! --------- CONFIGURATIONS DES VLANs ---------
! -------------------------------------------- 

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! CREER LES VLANs
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

! empty
interface vlan 50
ip address 10.10.50.6 255.255.255.0
no shutdown
interface vlan 55
ip address 10.10.55.6 255.255.255.0
no shutdown

! empty
interface vlan 60
ip address 10.10.60.6 255.255.255.0
no shutdown
interface vlan 65
ip address 10.10.65.6 255.255.255.0
no shutdown

interface vlan 70
ip address 10.10.70.6 255.255.255.0
no shutdown

interface vlan 80
ip address 10.10.80.6 255.255.255.0
no shutdown

interface vlan 444
ip address 10.10.44.6 255.255.255.0
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
interface vlan 50
ip helper-address 10.10.50.2
interface vlan 55
ip helper-address 10.10.55.2

! empty
interface vlan 60
ip helper-address 10.10.60.2
interface vlan 65
ip helper-address 10.10.65.2

interface vlan 70
ip helper-address 10.10.70.2

interface vlan 80
ip helper-address 10.10.80.2

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

! FERMER LES PORTS DONT ON N'A PAS BESOIN
interface range FastEthernet 0/5-20
description Vide
switchport mode access
switchport access vlan 555
shutdown

interface range GigabitEthernet 0/1 - 2
description Vide
switchport mode access
switchport access vlan 555
shutdown

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
interface range FastEthernet 0/21 - 22
channel-group 3 mode passive

! empty
interface range FastEthernet 0/23 - 24
channel-group 2 mode passive

! empty
interface Port-channel2
!switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 333
switchport trunk allowed vlan 50,55,60,65,70,80,333,444
no shutdown

! empty
interface Port-channel3
!switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 333
switchport trunk allowed vlan 50,55,60,65,70,80,333,444
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

! CONFIGURER L'ACCESS A DISTANCE
username Admin privilege 15 secret crosemont
crypto key generate rsa general-keys modulus 2048
line vty 0 15
password crosemont
login
transport input ssh
ip ssh version 2
exit

! ENCRYPTER LES MOTS DE PASSE
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
