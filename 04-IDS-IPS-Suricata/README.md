# IDS / IPS avec Suricata

## Présentation
Cette partie présente la mise en place d'un système **IDS/IPS** avec **Suricata** sur pfSense.

L'objectif est de surveiller le trafic réseau, de détecter des activités suspectes et, lorsque le mode IPS est utilisé, de bloquer certains trafics identifiés comme malveillants ou suspects.

Dans ce projet, Suricata complète les mécanismes de sécurité déjà mis en place avec les **VLAN** et le **VPN OpenVPN**.

## Qu'est-ce qu'un IDS ?
IDS signifie **Intrusion Detection System**.

Un IDS surveille le trafic réseau et recherche des comportements correspondant à des signatures ou règles de sécurité.

Lorsqu'une activité suspecte est détectée, une alerte est générée.

L'IDS permet donc principalement de **détecter et signaler** les activités suspectes.

## Qu'est-ce qu'un IPS ?
IPS signifie **Intrusion Prevention System**.

Contrairement à un IDS, un IPS peut prendre une action sur le trafic détecté.

Il permet de **détecter puis bloquer** certains trafics.

## Pourquoi utiliser Suricata ?
Suricata est utilisé dans ce laboratoire pour ajouter une couche de sécurité permettant :

d'analyser le trafic réseau ;
de détecter des activités suspectes ;
de générer des alertes ;
de bloquer certains trafics ;
d'enregistrer les événements dans des logs.

Suricata permet ainsi de compléter le pare-feu pfSense avec des fonctions de détection et de prévention des intrusions.

## Installation de Suricata
Suricata a été installé sur pfSense à partir du système de packages.

Après l'installation, plusieurs fonctionnalités sont disponibles dans l'interface :

Interfaces ;
Global Settings ;
Updates ;
Alerts ;
Blocks ;
Files ;
Pass Lists ;
Suppress ;
Logs View ;
Logs Mgmt.

(Images/01-suricata-interface.png)

## Configuration de l'interface
Suricata a été configuré pour surveiller le trafic réseau de l'infrastructure du laboratoire.

L'interface sélectionnée permet à Suricata d'analyser les communications qui la traversent.

Cette configuration permet d'intégrer la détection d'intrusions directement dans l'architecture pfSense.

## Emerging Threats Open
Pour permettre à Suricata de reconnaître différentes activités suspectes, un ensemble de règles de sécurité a été utilisé.

Le projet utilise **Emerging Threats Open (ET Open)**.

Les règles ont été téléchargées depuis l'interface de mise à jour de Suricata.

La mise à jour des règles a été réalisée avec succès.

(Images/02-suricata-emerging-threats.png)

## Catégories de règles
Plusieurs catégories de règles Emerging Threats sont disponibles dans Suricata.

Ces règles permettent de rechercher différents types de comportements suspects.

(screenshots/03-suricata-rules.png)

Les règles constituent donc la base de la détection effectuée par Suricata.

## Détection d'une activité suspecte
Après la configuration de Suricata, une alerte a été générée.

Elle est visible dans :

**Services → Suricata → Alerts**

(Images/04-suricata-alerts.png)

Cette alerte constitue une preuve que Suricata analyse effectivement le trafic et qu'une activité correspondant à une règle a été détectée.

## Détails de l'alerte
Les informations détaillées de l'événement permettent notamment d'identifier les éléments concernés par la détection.

Selon l'événement, on peut retrouver :

l'adresse IP source ;
l'adresse IP destination ;
le protocole ;
la classification ;
l'action associée.

(Images/05-suricata-alert-details.png)

Ces informations sont importantes pour l'analyse d'un incident de sécurité.

## Blocage IPS
La page **Blocks** permet de visualiser les adresses qui ont été bloquées par Suricata.

Dans notre laboratoire, des éléments apparaissent dans cette section.

(Images/06-suricata-ips-block.png)

Cela montre que Suricata ne fonctionne pas uniquement comme un système de détection.

Il peut également participer à la **prévention des intrusions** en bloquant certains trafics.

## Les logs Suricata
Suricata conserve également des journaux de fonctionnement et de sécurité.

Ils peuvent être consultés depuis :

**Services → Suricata → Logs View**

(Images/07-suricata-logs.png)

Les logs permettent de conserver des informations utiles pour :

analyser les alertes ;
comprendre les événements détectés ;
identifier les adresses IP concernées.

## Intégration dans l'architecture du projet

Suricata vient compléter les mécanismes de sécurité déjà réalisés.

L'architecture globale est maintenant :

                    Réseau
                       │
                       ▼
                    pfSense
                       │
              ┌────────┴────────┐
              │                 │
              ▼                 ▼
           VLAN 10          OpenVPN
              │                 │
              └────────┬────────┘
                       ▼
                   Suricata
                       │
                ┌──────┴──────┐
                ▼             ▼
              IDS            IPS
                │             │
                └──────┬──────┘
                       ▼
                     Logs

Les différentes technologies ont des rôles complémentaires :

| Technologie  | Rôle                                    |
| ------------ | --------------------------------------- |
| pfSense      | Pare-feu et routage                     |
| VLAN         | Segmentation du réseau                  |
| OpenVPN      | Accès distant sécurisé                  |
| Suricata IDS | Détection des activités suspectes       |
| Suricata IPS | Blocage de certains trafics             |
| Logs         | Conservation des événements de sécurité |

## Résultats
La mise en place de Suricata a permis de réaliser les actions suivantes :

installation de Suricata sur pfSense ;
configuration de l'interface à surveiller ;
téléchargement des règles Emerging Threats Open ;
activation de règles de détection ;
détection d'une activité générant une alerte ;
consultation des détails de l'alerte ;
blocage d'éléments par le mécanisme IPS ;
consultation des logs Suricata.

Le fonctionnement d'IDS/IPS est donc validé dans le laboratoire.

## Bilan
Suricata ajoute une couche de sécurité supplémentaire à l'infrastructure.

Le pare-feu permet principalement de contrôler les flux selon les règles configurées.

Les VLAN permettent de segmenter le réseau.

OpenVPN permet de sécuriser l'accès distant.

Suricata ajoute quant à lui la capacité à **inspecter le trafic, détecter des comportements suspects et bloquer certains événements**.