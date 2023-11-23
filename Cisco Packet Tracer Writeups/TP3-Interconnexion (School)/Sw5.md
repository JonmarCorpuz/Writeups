# Checklist

## Configuration de base
- [x] Hostname
- [x] Bannière
- [x] Créer un nom de domaine

## Configuration des ports
- [ ] Portfast
- [ ] BPDU Guard
- [x] Activation des ports utilisés
- [x] Désactivation des ports non utilisés
- [x] Description des ports
- [x] Laisser que les VLANs necéssaire passer pour chaque port (trunk)
- [x] LLDP est activé
- [x] LLDP est capable de transmit
- [x] CDP est activé
- [ ] EtherChannel

## Configuration des VLANs
- [x] Une interface de gestion qui peut être accessible par SSH

## Configuration des adresses IP
- [ ] PCs obtiennent leurs adresses de manière dynamique
- [ ] Téléphones IP obtiennent leurs adresses de manière dynamique
- [ ] Toutes les passerelles sont configurées sur une interface VLAN
- [ ] Création d'un serveur DHCP pour chaque batiment
- [ ] Création d'un pool DHCP pour les VLANs de voix
- [ ] Création d'un pool DHCP pour les VLANs de données
- [ ] Activation des DHCP Relay Agent (`ip helper-address`)

## Configuration de sécurité
- [x] Mot de passe pour le User EXEC Mode (`enable`)
- [x] Mot de passe pour le Privileged EXEC Mode (`configure terminal`)
- [x] Mot de passe pour l'accès à distance (`ssh`)
- [x] Encrypter les mots de passes dans le fichier de configuration
- [ ] Apprentissage des adresses MAC automatique
- [ ] Maximum de deux adresses MAC par port
- [ ] Bloquer les trames non autorisées et consigner les événements dans le journal

## Configuration du routage
- [ ] Routes statiques (Routes principales et routes flottantes)
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
hostname Switch1A

! CREER UNE BANNIERE
banner motd #Banner#

! empty
no ip domain lookup

! empty
ip domain-name crosemont.qc

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

! empty
interface vlan 10
ip address 10.10.10.4 255.255.255.0
no shutdown
interface vlan 15
ip address 10.10.15.4 255.255.255.0
no shutdown

! empty
interface vlan 20
ip address 10.10.20.4 255.255.255.0
no shutdown
interface vlan 25
ip address 10.10.25.4 255.255.255.0
no shutdown

interface vlan 30
ip address 10.10.30.4 255.255.255.0
no shutdown

interface vlan 40
ip address 10.10.40.4 255.255.255.0
no shutdown

interface vlan 888
ip address 10.10.88.4 255.255.255.0
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
ip helper-address 10.10.10.1
interface vlan 15
ip helper-address 10.10.15.1

! empty
interface vlan 20
ip helper-address 10.10.20.1
interface vlan 25
ip helper-address 10.10.25.1

interface vlan 30
ip helper-address 10.10.30.1

interface vlan 40
ip helper-address 10.10.40.1

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
description VLAN-A
switchport mode access
switchport access vlan 10
!mls qos trust cos
switchport voice vlan 15
lldp transmit
lldp receive
no shutdown

interface FastEthernet 0/2
description VLAN-D
switchport mode access
switchport access vlan 20
!mls qos trust cos
switchport voice vlan 25
lldp transmit
lldp receive
no shutdown

interface FastEthernet 0/3
description VLAN-B
switchport mode access
switchport access vlan 30
no shutdown

interface FastEthernet 0/4
description VLAN-C
switchport mode access
switchport access vlan 40
no shutdown

!interface range FastEthernet 0/21 - 24
!description Vers-Sw2-Sw3
!switchport mode trunk
!switchport trunk native vlan 777
!switchport trunk allowed vlan 10,15,20,25,30,40,777,888
!no shutdown

! FERMER LES PORTS DONT ON N'A PAS BESOIN
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

! SORTIR DE LA LIGNE DE CONFIGURATION DU VLAN
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

! empty
interface range FastEthernet 0/21 -24
no switchport

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
channel-group 3 mode active

! empty
interface range FastEthernet 0/23 - 24
channel-group 2 mode active

! empty
interface Port-channel2
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888
no shutdown

! empty
interface Port-channel3
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
