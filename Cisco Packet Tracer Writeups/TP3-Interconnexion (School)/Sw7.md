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
hostname sWITCH7

! CREER UNE BANNIERE
banner motd #Banner#

! empty
no ip domain lookup

! empty
ip domain-name crosemontSw7.qc

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

! CREER LE VLAN NATIF
vlan 333
name Natif

! CREER UN VLAN POUR LA GESTION A DISTANCE
vlan 444
name Gestion-distance

! CREER UN VLAN POUR LE TROU NOIR
vlan 555
name Trou-noir

! empty
interface vlan 50
ip address 10.10.50.7 255.255.255.0
no shutdown
interface vlan 55
ip address 10.10.55.7 255.255.255.0
no shutdown

! empty
interface vlan 60
ip address 10.10.60.7 255.255.255.0
no shutdown
interface vlan 65
ip address 10.10.65.7 255.255.255.0
no shutdown

interface vlan 70
ip address 10.10.70.7 255.255.255.0
no shutdown

interface vlan 80
ip address 10.10.80.7 255.255.255.0
no shutdown

interface vlan 444
ip address 10.10.44.7 255.255.255.0
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
! -------- CONFIGURATIONS DES PORTS ----------
! -------------------------------------------- 

! ENTRER EN USER EXEC MODE 
enable

! ENTRER EN PRIVILEGED EXEC MODE
configure terminal

! OUVRIR LES PORTS DONT ON A BESOIN
interface FastEthernet 0/1
description Vers-R3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk native vlan 333
switchport trunk allowed vlan 50,55,60,65,70,80,333,444
no shutdown

! FERMER LES PORTS DONT ON N'A PAS BESOIN
interface range FastEthernet 0/2 - 20
description Vide
switchport mode access
switchport access vlan 555
shutdown

interface range FastEthernet 0/23 - 24
description Vide
switchport mode access
switchport access vlan 555
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
interface vlan 50
ip address 10.10.50.7 255.255.255.0
standby 50 ip 10.10.50.10
standby 50 priority 100
standby 50 preempt
no shutdown
interface vlan 55
ip address 10.10.55.7 255.255.255.0
standby 55 ip 10.10.55.10
standby 55 priority 100
standby 55 preempt
no shutdown

! empty
interface vlan 60
ip address 10.10.60.7 255.255.255.0
standby 60 ip 10.10.60.10
standby 60 priority 100
standby 60 preempt
no shutdown
interface vlan 65
ip address 10.10.65.7 255.255.255.0
standby 65 ip 10.10.65.10
standby 65 priority 100
standby 65 preempt
no shutdown

! empty
interface vlan 70
ip address 10.10.70.7 255.255.255.0
standby 70 ip 10.10.70.10
standby 70 priority 100
standby 70 preempt
no shutdown

! empty
interface vlan 80
ip address 10.10.80.7 255.255.255.0
standby 80 ip 10.10.80.10
standby 80 priority 100
standby 80 preempt
no shutdown

! empty
interface vlan 444
ip address 10.10.44.7 255.255.255.0
standby 44 ip 10.10.44.10
standby 44 priority 100
standby 44 preempt
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
interface range FastEthernet 0/21 - 22
channel-group 2 mode active

! empty
interface Port-channel2
switchport trunk encapsulation dot1q
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
