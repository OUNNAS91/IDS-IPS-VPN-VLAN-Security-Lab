# Segmentation réseau avec VLAN

## Présentation
Dans ce projet, un VLAN (Virtual Local Area Network)** a été mis en place afin de réaliser une segmentation du réseau.

Un VLAN permet de séparer logiquement un réseau en plusieurs réseaux distincts, même lorsque les machines utilisent la même infrastructure physique ou virtuelle.

L'objectif est d'améliorer l'organisation et la sécurité du réseau.

## Qu'est-ce qu'un VLAN ?
VLAN signifie **Virtual Local Area Network**.

En termes simples, un VLAN permet de créer un réseau virtuel séparé à l'intérieur d'une infrastructure réseau.

                 Réseau
                    │
          ┌─────────┼─────────┐
          │         │         │
          ▼         ▼         ▼
       VLAN 10   VLAN 20   VLAN 30
       Tests    Utilisateurs Serveurs

Chaque VLAN peut avoir son propre réseau IP.

Cette séparation permet notamment de limiter les communications entre les différents groupes de machines.

## Objectif du VLAN dans ce laboratoire
Dans notre laboratoire, le **VLAN 10** est utilisé comme réseau de test.

Il permet de séparer les machines utilisées pour les tests de sécurité du reste de l'infrastructure.

## Réseau utilisé
Le réseau associé au VLAN 10 utilisé dans le laboratoire est :

192.168.10.0/24

L'adresse de pfSense sur ce réseau est :

192.168.10.1

Le masque réseau est :

255.255.255.0

## Fonctionnement
Le principe peut être représenté simplement comle ceci :

pfSense permet de gérer le trafic entre les différents réseaux.

Les règles de sécurité peuvent ensuite déterminer quels flux sont autorisés ou bloqués.

## Relation avec le VPN
Le VLAN 10 est également utilisé pour tester l'accès d'un client connecté au VPN.

Le client Windows 11 reçoit une adresse VPN :

10.8.0.2

Le tunnel VPN permet ensuite au client d'accéder au réseau VLAN 10.  

## Test de connectivité
Après avoir établi la connexion OpenVPN, un test de communication avec le VLAN 10 a été réalisé.

Depuis Windows 11, la commande suivante a été utilisée :

powershell
ping 192.168.10.1


Le test a obtenu une réponse.

Cela confirme que le client connecté au VPN peut atteindre l'adresse de pfSense située sur le réseau VLAN 10.

## Résultat
La mise en place du VLAN 10 permet de disposer d'un réseau logique séparé pouvant être utilisé pour les tests de sécurité.

Le test réalisé confirme également que le réseau VLAN peut être atteint depuis le client connecté au VPN.

## Sécurité
La segmentation réseau permet de réduire la surface d'exposition d'une infrastructure.

En séparant les réseaux, il devient possible de :

limiter les communications ;
appliquer des règles différentes selon les réseaux ;
isoler les machines de test ;
contrôler les accès entre les VLAN ;
réduire les risques de propagation d'une attaque.

Le VLAN ne remplace cependant pas un pare-feu ou un IDS/IPS. Il constitue une couche supplémentaire de sécurité.

## Résumé

| Élément    | Valeur              |
| ---------- | ------------------- |
| VLAN       | VLAN 10             |
| Réseau     | 192.168.10.0/24     |
| pfSense    | 192.168.10.1        |
| Masque     | 255.255.255.0       |
| Client VPN | 10.8.0.2            |
| Test       | `ping 192.168.10.1` |
| Résultat   | Réponse reçue       |

Le VLAN 10 est donc fonctionnel et peut être utilisé avec les autres mécanismes de sécurité du laboratoire.