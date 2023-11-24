# Checklist

## Configuration de base
- [x] Hostname
- [x] Bannière
- [x] Créer un nom de domaine

## Configuration des ports
- [x] Activation des ports utilisés
- [x] Désactivation des ports non utilisés
- [x] Description des ports
- [ ] Laisser que les VLANs necéssaire passer pour chaque port (trunk)

## Configuration des VLANs
- [x] Une interface de gestion qui peut être accessible par SSH

## Configuration des adresses IP
- [x] PCs obtiennent leurs adresses de manière dynamique
- [x] Téléphones IP obtiennent leurs adresses de manière dynamique
- [x] Toutes les passerelles sont configurées sur une interface VLAN
- [x] Création d'un serveur DHCP pour chaque batiment
- [x] Création d'un pool DHCP pour les VLANs de voix
- [x] Création des pools DHCP pour les VLANs de données
- [x] Activation des DHCP Relay Agent (`ip helper-address`)

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
- [x] Activer le routage
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
hostname Switch1

! CREER UNE BANNIERE
banner motd #Banner#

! empty
no ip domain lookup

! empty
ip domain-name crosemontSw1.qc

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

! SORTIR DU MODE DE CONFIGURATION GLOBALE 
end

! --------------------------------------------
! ------ CONFIGURATIONS DES POOLS DHCP -------
! -------------------------------------------- 

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! CREER ET CONFIGURER LES POOLS DHCP POUR LES VLANs DE DATA
ip dhcp pool VLAN-A
network 10.10.10.0 255.255.255.0
default-router 10.10.10.1
ip dhcp excluded-address 10.10.10.1 10.10.10.10

ip dhcp pool VLAN-D
network 10.10.20.0 255.255.255.0
default-router 10.10.20.1
ip dhcp excluded-address 10.10.20.1 10.10.20.10

ip dhcp pool VLAN-B
network 10.10.30.0 255.255.255.0
default-router 10.10.30.1 
ip dhcp excluded-address 10.10.30.1 10.10.30.10

ip dhcp pool VLAN-C
network 10.10.40.0 255.255.255.0
default-router 10.10.40.1 
ip dhcp excluded-address 10.10.40.1 10.10.40.10

! CREER ET CONFIGURER DES POOLS DHCP POUR LES VLANs DE VOIX
ip dhcp pool VLAN-A-Voix
network 10.10.15.0 255.255.255.0
default-router 10.10.15.1
ip dhcp excluded-address 10.10.15.1 10.10.15.10

ip dhcp pool VLAN-D-Voix
network 10.10.25.0 255.255.255.0
default-router 10.10.25.1
ip dhcp excluded-address 10.10.25.1 10.10.25.10

! SORTIR DU MODE DE CONFIGURATION GLOBALE 
end

! --------------------------------------------
! -------- CONFIGURATIONS DES PORTS ----------
! -------------------------------------------- 

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! OUVRIR LES PORTS DONT ON A BESOIN
interface GigabitEthernet 0/1
description Vers-R2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888
no switchport
ip address 192.168.10.2 255.255.255.0
ip route 0.0.0.0 0.0.0.0 192.168.10.1 5
no shutdown

interface GigabitEthernet 0/2
description Vers-R1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888
no switchport
ip address 192.168.20.2 255.255.255.0
ip route 0.0.0.0 0.0.0.0 192.168.20.1 10
no shutdown

interface FastEthernet 0/1
description Vers-Sw2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888
no shutdown

interface FastEthernet 0/2
description Vers-Sw3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 777
switchport trunk allowed vlan 10,15,20,25,30,40,777,888
no shutdown

! FERMER LES PORTS DONT ON N'A PAS BESOIN
interface range FastEthernet 0/3 - 24
description Vide
switchport mode access
switchport access vlan 999
shutdown

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
