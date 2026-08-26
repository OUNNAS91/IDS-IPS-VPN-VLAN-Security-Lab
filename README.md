# Présentation du projet
Ce projet consiste à mettre en place un environnement de laboratoire permettant d'étudier et de mettre en œuvre plusieurs mécanismes de sécurité réseau :

la segmentation du réseau avec les **VLAN** ;
la sécurisation des accès distants avec un **VPN OpenVPN** ;
la détection et la prévention des intrusions avec **Suricata IDS/IPS**.

Le projet est réalisé dans un environnement virtualisé avec **VirtualBox** et **pfSense**.

La configuration du pare-feu pfSense de base a été réalisée dans un projet précédent. Ce projet se concentre donc sur les mécanismes de sécurité complémentaires : **VLAN, VPN et IDS/IPS**.

## Objectifs
Les objectifs du projet sont les suivants :

comprendre le principe de la segmentation réseau ;
créer un VLAN ;
comprendre le fonctionnement d'un VPN ;
mettre en place un serveur VPN avec OpenVPN ;
authentifier un utilisateur VPN ;
utiliser des certificats pour sécuriser les connexions ;
vérifier qu'un client distant peut accéder au réseau interne ;
installer et configurer Suricata ;
utiliser Suricata comme IDS/IPS ;
générer et observer des événements de sécurité ;
réaliser des tests permettant de valider la configuration.

## Technologies utilisées

| Technologie           | Utilisation                       |
| --------------------- | --------------------------------- |
| VirtualBox            | Virtualisation des machines       |
| pfSense               | Infrastructure réseau et sécurité |
| VLAN                  | Segmentation du réseau            |
| OpenVPN               | Accès distant sécurisé            |
| Suricata              | IDS/IPS                           |
| Windows 11            | Machine cliente et tests          |
| Emerging Threats Open | Règles de détection Suricata      |

## Architecture du laboratoire
L'architecture générale du projet est organisée autour de pfSense.

L'architecture détaillée sera présentée dans le dossier **Architecture**.

## Segmentation réseau avec VLAN

### Qu'est-ce qu'un VLAN ?
Un VLAN (Virtual Local Area Network) permet de créer plusieurs réseaux logiques à partir d'une même infrastructure physique.

L'objectif est notamment de séparer les machines et les flux réseau.

Par exemple :

Réseau physique
      │
      ├── VLAN 10 → réseau de test
      │
      ├── VLAN 20 → utilisateurs
      │
      └── VLAN 30 → serveurs

Dans ce laboratoire, un **VLAN 10** a été utilisé pour créer un réseau séparé.

### Objectif du VLAN
La segmentation permet notamment de :

limiter les communications entre différents réseaux ;
isoler certaines machines ;
appliquer des règles de sécurité différentes ;
réduire l'impact potentiel d'une compromission.

La configuration détaillée du VLAN est documentée dans :

`02-VLAN/README.md`

## VPN avec OpenVPN

### Qu'est-ce qu'un VPN ?

Un VPN (Virtual Private Network) crée un **tunnel sécurisé** entre deux machines ou deux réseaux.

Dans ce projet, OpenVPN permet à une machine Windows 11 de se connecter à pfSense de manière sécurisée.

Schéma simplifié :

Windows 11
    │
    │ 🔐 Tunnel VPN
    │
    ▼
 pfSense
    │
    ▼
Réseau interne

### Serveur OpenVPN
Un serveur OpenVPN a été configuré sur pfSense.

L'authentification utilise :

un compte utilisateur local ;
un certificat utilisateur ;
une autorité de certification (CA).

### Réseau VPN

Le réseau virtuel utilisé par OpenVPN est :

10.8.0.0/24

Lors de la connexion, le client Windows 11 a reçu :

10.8.0.2

Cela confirme que le client a reçu une adresse à l'intérieur du réseau VPN.

### Test de connexion

La connexion VPN a été vérifiée depuis pfSense.

Le client apparaît avec :

Common Name : vpnuser
Virtual Address : 10.8.0.2
Status : up

Un test de communication avec le réseau VLAN 10 a également été réalisé avec succès.

La configuration détaillée du VPN est documentée dans :

`03-VPN-OpenVPN/README.md`

## IDS/IPS avec Suricata

### Qu'est-ce qu'un IDS ?

IDS signifie :

**Intrusion Detection System**

Un IDS surveille le trafic réseau afin de détecter des comportements suspects.

Lorsqu'une activité suspecte est détectée, l'IDS génère une alerte.

### Qu'est-ce qu'un IPS ?

IPS signifie :

**Intrusion Prevention System**

Un IPS va plus loin qu'un IDS.

Il peut détecter une activité malveillante et prendre une action pour **bloquer le trafic**

### Suricata

Suricata a été installé sur pfSense afin de fournir les fonctions IDS/IPS.

Le jeu de règles **Emerging Threats Open** a également été installé et mis à jour.

Ces règles permettent à Suricata de reconnaître différentes signatures correspondant à des activités réseau suspectes.

La configuration détaillée est documentée dans :

`04-IDS-IPS-Suricata/README.md`

## Tests réalisés

Plusieurs tests ont été effectués afin de vérifier le fonctionnement de l'environnement.

### Test VPN

Le client Windows 11 a obtenu :

10.8.0.2

Le serveur OpenVPN affichait :

Status : up

### Test du réseau VLAN

Depuis le client connecté au VPN, un test vers :

192.168.10.1

a obtenu une réponse.

Cela confirme que le client VPN peut atteindre le réseau VLAN 10.

### Test IDS/IPS

Des vérifications de fonctionnement de Suricata ont été réalisées afin de confirmer :

le fonctionnement du service ;
le chargement des règles ;
la génération et la consultation des événements de sécurité.

Les résultats détaillés seront présentés dans le dossier **05-Tests**.

## Sécurité mise en place

Le projet permet de combiner plusieurs mécanismes de défense :

                  Sécurité réseau
                       │
       ┌───────────────┼───────────────┐
       │               │               │
       ▼               ▼               ▼
     VLAN             VPN           IDS / IPS
       │               │               │
   Isolation       Chiffrement      Détection
   réseau          des échanges     et blocage


Chaque mécanisme possède un rôle différent.

### VLAN

Le VLAN permet de séparer les réseaux.

### VPN

Le VPN permet de sécuriser les communications à distance.

### IDS/IPS

Suricata permet de détecter et éventuellement bloquer les activités suspectes.

La combinaison de ces mécanismes permet d'obtenir une architecture plus sécurisée qu'un simple réseau non segmenté.

## 10. Limites du laboratoire

Ce projet est réalisé dans un environnement virtuel de laboratoire.

Les principales limites sont :

ressources matérielles limitées sur la machine hôte ;
nombre limité de machines virtuelles exécutées simultanément ;
environnement destiné principalement à l'apprentissage ;
absence de trafic utilisateur réel ;
certaines fonctionnalités peuvent être simplifiées par rapport à une infrastructure professionnelle.

L'objectif est avant tout de comprendre les principes et de mettre en pratique les technologies de sécurité réseau.

## Conclusion
Ce projet a permis de mettre en pratique plusieurs mécanismes essentiels de sécurisation d'un réseau.

La segmentation avec les VLAN permet d'isoler les différents réseaux.

OpenVPN permet de créer un accès distant sécurisé grâce à un tunnel chiffré et à l'utilisation de certificats.

Suricata apporte une couche supplémentaire de sécurité en permettant de détecter et de prévenir certaines activités suspectes.

L'ensemble constitue une base permettant de poursuivre vers une architecture de sécurité plus complète intégrant notamment un **SIEM et une supervision centralisée**.
