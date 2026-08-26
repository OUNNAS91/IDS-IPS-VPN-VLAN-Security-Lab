# VPN OpenVPN

## Présentation

Cette partie présente la mise en place d'un serveur **OpenVPN** sur pfSense.

L'objectif est de permettre à un utilisateur distant de se connecter de manière sécurisée au réseau du laboratoire.

Le VPN ajoute une couche de sécurité supplémentaire à l'infrastructure.

## Qu'est-ce qu'un VPN ?
VPN signifie **Virtual Private Network**, ou réseau privé virtuel.

Un VPN crée un **tunnel sécurisé** entre un ordinateur et un serveur VPN.

Dans notre laboratoire :

Windows 11
    │
    │ Connexion VPN sécurisée
    ▼
pfSense
    │
    ▼
Réseau interne

Le serveur VPN est installé sur pfSense.

L'utilisateur peut ainsi accéder aux ressources autorisées du réseau à travers le tunnel VPN.

## Pourquoi utiliser un VPN ?
Un VPN permet notamment de :

sécuriser les communications ;
authentifier les utilisateurs ;
chiffrer les données échangées ;
permettre un accès distant au réseau ;
contrôler l'accès aux ressources internes.

Dans notre laboratoire, OpenVPN est utilisé pour simuler un utilisateur distant qui doit accéder à notre infrastructure.

## Mise en place du serveur OpenVPN
Le serveur OpenVPN a été configuré sur pfSense.

Les principaux paramètres utilisés sont :

| Paramètre                 | Configuration                  |
| ------------------------- | ------------------------------ |
| Mode                      | Remote Access                  |
| Authentification          | Utilisateur local + certificat |
| Protocole                 | UDP IPv4                       |
| Port                      | 1194                           |
| Interface                 | WAN                            |
| Réseau VPN                | 10.8.0.0/24                    |
| Autorité de certification | VPN-CA                         |
| Utilisateur               | vpnuser                        |

La configuration du serveur est visible dans pfSense.

(Images/01-openvpn-server.png)

## Autorité de certification
Une autorité de certification appelée `VPN-CA` a été créée.

Une **CA (Certificate Authority)** sert à créer et signer les certificats utilisés pour authentifier les différents éléments du VPN.

Dans notre cas :

VPN-CA
   │
   ├── Certificat du serveur
   │
   └── Certificat de vpnuser


La présence de `VPN-CA` est visible dans pfSense.

(Images/02-openvpn-certificate-authority.png)

## Création de l'utilisateur VPN
Un utilisateur local nommé `vpnuser` a été créé dans pfSense.

Cet utilisateur permet de s'authentifier auprès du serveur OpenVPN.

(Images/03-openvpn-user.png)

## Certificat de l'utilisateur
Un certificat nommé `vpnuser` a également été créé.

Ce certificat est associé à l'autorité de certification `VPN-CA`.

Le certificat permet de renforcer l'authentification du client VPN.

(Images/04-openvpn-user-certificate.png)

## Export de la configuration OpenVPN
Pour permettre à Windows 11 de se connecter au serveur, la configuration du client OpenVPN a été exportée depuis pfSense.

Le package **OpenVPN Client Export** permet de générer automatiquement les fichiers nécessaires à la connexion du client.

(Images/05-openvpn-client-export.png)

Le fichier d'installation Windows utilisé dans le laboratoire est :

openvpn-pfSense-UDP4-1194-vpnuser-install-2.7.4-I001-amd64.exe

Ce fichier permet d'installer la configuration nécessaire sur le poste Windows.

## Installation du client OpenVPN
Le client OpenVPN a été installé sur Windows 11.

La version utilisée pour le client est :

OpenVPN 2.7.6

Après l'installation, la configuration exportée depuis pfSense a permis d'établir la connexion avec le serveur VPN.

## Connexion au VPN
L'utilisateur `vpnuser` s'est connecté au serveur OpenVPN depuis Windows 11.

La connexion est établie avec succès.

(Images/06-openvpn-client-connected.png)

## Adresse IP attribuée par le VPN
Une fois connecté, Windows 11 reçoit une adresse IP virtuelle appartenant au réseau VPN.

L'adresse obtenue dans notre laboratoire est :

10.8.0.2

Le réseau VPN utilisé est :

10.8.0.0/24

(Images/07-openvpn-ip-vpn.png)

### Explication simple

L'adresse `10.8.0.2` n'est pas l'adresse réseau locale habituelle de Windows.

Elle représente l'adresse attribuée à Windows **à l'intérieur du tunnel VPN**.

On peut donc représenter la connexion ainsi :

Windows 11
Adresse VPN : 10.8.0.2
       │
       │ Tunnel OpenVPN
       ▼
    pfSense


## Vérification de la connexion côté pfSense
La connexion a également été vérifiée directement depuis pfSense.

Le serveur affiche :

Common Name      vpnuser
Virtual Address  10.8.0.2
Status           up

Cela confirme que l'utilisateur `vpnuser` est actuellement connecté au serveur OpenVPN.

(Images/08-openvpn-status-connected.png)

## Test de communication avec le VLAN
Après avoir établi le tunnel VPN, un test de communication avec le réseau VLAN 10 a été réalisé.

Depuis Windows 11 :

powershell
ping 192.168.10.1

Une réponse a été reçue.

(Images/09-openvpn-test-vlan10.png)

Ce test permet de vérifier que le client VPN peut communiquer avec l'adresse de pfSense située sur le réseau VLAN 10.

## Fonctionnement global

Le fonctionnement obtenu est le suivant :

                 Windows 11
                 10.8.0.2
                     │
                     │
                OpenVPN
              Tunnel sécurisé
                     │
                     ▼
                 pfSense
                     │
                     ▼
                  VLAN 10
               192.168.10.0/24
                     │
                     ▼
                192.168.10.1

L'utilisateur `vpnuser` est authentifié et reçoit l'adresse virtuelle `10.8.0.2`.

Il peut ensuite accéder aux réseaux autorisés à travers pfSense.

## Résultats
La mise en place du VPN OpenVPN est fonctionnelle.

Les éléments suivants ont été réalisés :

création de l'autorité de certification `VPN-CA` ;
création de l'utilisateur `vpnuser` ;
création du certificat utilisateur ;
configuration du serveur OpenVPN ;
installation du package OpenVPN Client Export ;
export de la configuration client ;
installation du client OpenVPN sur Windows 11 ;
connexion réussie au serveur ;
attribution de l'adresse VPN `10.8.0.2` ;
vérification de la connexion côté pfSense ;
test de communication avec le réseau VLAN 10.

## Résumé

| Élément           | Valeur                  |
| ----------------- | ----------------------- |
| Technologie       | OpenVPN                 |
| Serveur           | pfSense                 |
| Mode              | Remote Access           |
| Protocole         | UDP IPv4                |
| Port              | 1194                    |
| CA                | VPN-CA                  |
| Utilisateur       | vpnuser                 |
| Réseau VPN        | 10.8.0.0/24             |
| Adresse du client | 10.8.0.2                |
| VLAN testé        | VLAN 10                 |
| Adresse testée    | 192.168.10.1            |
| Résultat          | Connexion fonctionnelle |

Le VPN OpenVPN constitue ainsi une couche d'accès distant sécurisé venant compléter la segmentation réalisée avec le VLAN 10.