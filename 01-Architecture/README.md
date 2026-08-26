# Architecture du laboratoire

## Présentation
Cette partie présente l'architecture du laboratoire utilisé pour mettre en œuvre les mécanismes de sécurité réseau.

Le projet repose sur trois éléments principaux :

**VLAN** : segmentation du réseau ;
**OpenVPN** : accès distant sécurisé ;
**Suricata IDS/IPS** : détection et prévention des intrusions.

Le pare-feu **pfSense** constitue le point central de l'infrastructure.

## Schéma de l'architecture
Le schéma ci-dessous représente l'organisation générale du laboratoire.

(architecture.png)

## Rôle des différents éléments

### pfSense

pfSense constitue le point central du laboratoire.

Il permet notamment de gérer :

les interfaces réseau ;
la segmentation VLAN ;
le serveur VPN OpenVPN ;
la surveillance du trafic avec Suricata.

La configuration générale du pare-feu a été réalisée dans un projet précédent.

### VLAN 10

Le VLAN 10 permet de créer un réseau séparé.

Il est utilisé dans ce laboratoire afin de mettre en pratique la **segmentation réseau**.

La segmentation permet notamment de limiter les communications entre différents réseaux et d'isoler certaines machines.

### OpenVPN

OpenVPN permet de créer un **tunnel VPN sécurisé** entre un client et pfSense.

Dans notre laboratoire, un utilisateur nommé `vpnuser` a été créé.

Lors de la connexion, le client VPN reçoit une adresse appartenant au réseau :

10.8.0.0/24

Le client Windows 11 a reçu l'adresse :

10.8.0.2


La connexion a été vérifiée depuis pfSense avec l'état :

Status : up

### Suricata

Suricata est utilisé comme système **IDS/IPS**.

Il analyse le trafic réseau afin d'identifier des activités suspectes.

En mode IDS, Suricata peut générer des alertes.

En mode IPS, il peut également bloquer certains trafics détectés comme malveillants.

Le jeu de règles **Emerging Threats Open** a été utilisé dans le laboratoire.

### Windows 11

Windows 11 est utilisé comme machine cliente pour réaliser différents tests.

La machine permet notamment de :

tester la connectivité réseau ;
se connecter au VPN ;
vérifier l'adresse IP attribuée par OpenVPN ;
générer du trafic permettant de vérifier le fonctionnement de Suricata.

## Objectif de l'architecture

L'objectif est de mettre en place plusieurs couches de sécurité complémentaires :

                 Réseau
                   │
                   ▼
             Segmentation
                VLAN 10
                   │
                   ▼
             Accès sécurisé
                OpenVPN
                   │
                   ▼
             Surveillance
               Suricata
                   │
                   ▼
             IDS / IPS

Cette architecture permet de comprendre comment différents mécanismes peuvent être combinés afin d'améliorer la sécurité d'une infrastructure réseau.

## Résumé

| Élément               | Fonction                          |
| --------------------- | --------------------------------- |
| pfSense               | Point central de l'infrastructure |
| VLAN 10               | Segmentation du réseau            |
| OpenVPN               | Accès distant sécurisé            |
| VPN-CA                | Autorité de certification du VPN  |
| vpnuser               | Utilisateur VPN                   |
| Suricata              | IDS/IPS                           |
| Emerging Threats Open | Règles de détection               |
| Windows 11            | Machine de test                   |

Les configurations détaillées de chaque technologie sont présentées dans les sections suivantes du projet.
