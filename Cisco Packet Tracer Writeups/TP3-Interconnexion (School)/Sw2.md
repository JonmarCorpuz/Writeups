# Checklist

## Configuration de base
- [x] Hostname
- [x] Bannière

## Configuration des ports
- [ ] Portfast
- [ ] BPDU Guard
- [x] Activation des ports utilisés
- [x] Désactivation des ports non utilisés
- [x] Description des ports
- [ ] Laisser que les VLANs necéssaire passer pour chaque port (trunk)

## Configuration des VLANs
- [ ] Une interface de gestion qui peut être accessible par SSH

## Configuration des adresses IP
- [ ] PCs obtiennent leurs adresses de manière dynamique
- [ ] Téléphones IP obtiennent leurs adresses de manière dynamique
- [ ] Toutes les passerelles sont configurées sur une interface VLAN
- [ ] Création d'un serveur DHCP pour chaque batiment
- [ ] Création d'un pool DHCP pour les VLANs de voix
- [ ] Création des pools DHCP pour les VLANs de données
- [ ] Activation des DHCP Relay Agent (`ip helper-address`)

## Configuration de sécurité
- [x] Mot de passe pour le User EXEC Mode (`enable`)
- [x] Mot de passe pour le Privileged EXEC Mode (`configure terminal`)
- [x] Mot de passe pour le Auxiliary Mode
- [x] Mot de passe pour l'accès à distance (`ssh`)
- [x] Encrypter les mots de passes dans le fichier de configuration
- [ ] Apprentissage des adresses MAC automatique
- [ ] Maximum de deux adresses MAC par port
- [ ] Bloquer les trames non autorisées et consigner les événements dans le journal

## Configuration du routage
- [ ] Activer le routage
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
!interface vlan 10
!ip address 10.10.10.2 255.255.255.0
!no shutdown
!interface vlan 15
!ip address 10.10.15.2 255.255.255.0
!no shutdown

! empty
!interface vlan 20
!ip address 10.10.20.2 255.255.255.0
!no shutdown
!interface vlan 25
!ip address 10.10.25.2 255.255.255.0
!no shutdown

!interface vlan 30
!ip address 10.10.30.2 255.255.255.0
!no shutdown

!interface vlan 40
!ip address 10.10.40.2 255.255.255.0
!no shutdown

!interface vlan 888
!ip address 10.10.88.2 255.255.255.0
!no shutdown

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
description Vers-Sw1
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888,999
no shutdown

interface FastEthernet 0/23
description Vers-Sw5
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888,999
no shutdown

interface FastEthernet 0/24
description Vers-Sw5
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888,999
no shutdown

! FERMER LES PORTS DONT ON N'A PAS BESOIN
interface range FastEthernet 0/4 - 22
description Vide
switchport mode access
switchport access vlan 999
shutdown

interface range GigabitEthernet 0/1 -2
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
interface range FastEthernet 0/1 - 3
no switchport

! SORTIR DE LA LIGNE DE CONFIGURATION DU VLAN
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

! CONFIGURER UN MOT DE PASSE POUR L'ACCESS A DISTANCE
line vty 0 15
password crosemont
login
exit

! ENCRYPTER LES MOTS DE PASSE
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
