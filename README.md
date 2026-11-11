# Infrastructure réseau multi-sites — Amiens & Paris

## Présentation du projet

Ce projet consiste à concevoir, déployer et administrer une infrastructure réseau multi-sites simulant l'environnement informatique d'une entreprise disposant de deux implantations géographiques : **Amiens et Paris**.

L'objectif est de mettre en place une infrastructure permettant l'interconnexion sécurisée des deux sites, l'administration centralisée des utilisateurs et des postes de travail, ainsi que l'accès aux ressources réseau partagées.

Le laboratoire repose sur des machines virtuelles et utilise pfSense, Ubuntu Server et Windows Server.

## Objectifs

- Concevoir une architecture réseau composée de deux sites indépendants.
- Mettre en place le routage entre les différents réseaux.
- Établir une liaison VPN IPsec site-à-site.
- Déployer Active Directory pour centraliser la gestion des utilisateurs et des ordinateurs.
- Configurer les services DNS et DHCP.
- Administrer les postes clients à l'aide des stratégies de groupe (GPO).
- Mettre en place des profils itinérants.
- Centraliser les ressources partagées et gérer les permissions d'accès.
- Automatiser le mappage des lecteurs réseau selon les services.

## Architecture réseau

L'infrastructure est organisée autour de deux sites interconnectés par un tunnel VPN IPsec.

| Élément | Site d'Amiens | Site de Paris |
|---|---|---|
| Réseau LAN | `192.168.10.0/24` | `192.168.20.0/24` |
| Passerelle | `192.168.10.1` | `192.168.20.1` |
| Pare-feu | pfSense | pfSense |
| DHCP | Ubuntu Server | pfSense |
| Active Directory | Windows Server | Accès au contrôleur de domaine d'Amiens |
| DNS | Windows Server | DNS centralisé à Amiens |

**Serveurs principaux :**

| Machine | Adresse IPv4 | Fonction |
|---|---|---|
| `srv-dhcp` | `192.168.10.10` | DHCP Amiens |
| `srv-ad` | `192.168.10.20` | Active Directory et DNS |

**Schéma général :**

```text
                       INTERNET / WAN
                           |
                 +---------+---------+
                 |                   |
              WAN|                   |WAN
          +------+-----+       +-----+------+
          |  pfSense   |=======|  pfSense   |
          |  AMIENS    | IPsec |   PARIS    |
          +------+-----+       +-----+------+
                 |                   |
          LAN :  |                   |  LAN :
      192.168.10.0/24             192.168.20.0/24
                 |                   |
       +---------+---------+         |
       |         |         |         |
   +---+---+ +---+----+ +--+------+  +----------+
   | srv-ad| |srv-dhcp| | Client  |  | Client   |
   | AD/DNS| | Ubuntu | | Windows |  | Windows  |
   +-------+ +--------+ +---------+  +----------+
       |         |          |              |
    .10.20    .10.10       DHCP           DHCP
                          Ubuntu         pfSense
```

*Schéma simplifié de l'architecture logique. Les détails des interfaces WAN, du tunnel et des routes sont présentés dans la documentation.*

## Technologies utilisées

| Technologie | Utilisation |
|---|---|
| VMware Workstation | Virtualisation des machines |
| pfSense | Pare-feu et routage |
| VPN IPsec | Interconnexion sécurisée des sites |
| Ubuntu Server | Hébergement du serveur DHCP d'Amiens |
| ISC DHCP Server | Attribution automatique des adresses IPv4 à Amiens |
| Windows Server | Administration du domaine |
| Active Directory | Gestion centralisée des utilisateurs et ordinateurs |
| DNS | Résolution des noms du domaine |
| DHCP pfSense | Attribution automatique des adresses IPv4 à Paris |
| GPO | Administration et sécurisation des postes |
| SMB | Partage de fichiers sur le réseau |

## Services et fonctionnalités déployés

### 1. Réseau et interconnexion

- Configuration des réseaux locaux d'Amiens et Paris.
- Configuration des interfaces LAN et WAN sur pfSense.
- Routage entre les deux sites.
- Mise en place d'un tunnel VPN IPsec site-à-site.
- Communication entre les réseaux distants.

### 2. Services DHCP et DNS

- Déploiement d'un serveur DHCP indépendant sous Ubuntu Server à Amiens.
- Configuration du service DHCP intégré à pfSense à Paris.
- Attribution automatique des paramètres IPv4 aux clients.
- Utilisation du DNS Active Directory centralisé sur `srv-ad`.
- Configuration du suffixe DNS `lab.local`.

### 3. Active Directory

- Création du domaine `lab.local`.
- Organisation des unités d'organisation (OU).
- Création et gestion des utilisateurs.
- Création des groupes de sécurité.
- Intégration des postes clients au domaine.

### 4. Stratégies de groupe (GPO)

- Configuration des fonds d'écran selon le site et le service.
- Mise en place d'une politique de mots de passe.
- Configuration du verrouillage des comptes.
- Restrictions sur les supports amovibles.
- Configuration du verrouillage automatique des sessions.
- Restriction du Panneau de configuration pour le service RH.
- Attribution automatique des lecteurs réseau.

### 5. Profils itinérants

Mise en place de profils itinérants afin de permettre aux utilisateurs de retrouver leur environnement de session lorsqu'ils se connectent depuis différents postes du domaine.

### 6. Partages réseau et permissions

Création d'un espace de stockage centralisé :

`\\srv-ad.lab.local\Partages`

Organisation des dossiers :

```text
Partages/
├── IT/
├── RH/
└── Direction/
```

Les accès sont contrôlés à l'aide de groupes de sécurité Active Directory et de permissions NTFS et SMB.

### 7. Mappage automatique des lecteurs

La GPO `GPO_Lecteurs_Reseau` attribue automatiquement les lecteurs selon l'appartenance aux groupes de sécurité :

| Groupe | Lecteur | Ressource |
|---|---|---|
| `GG_IT` | `I:` | IT |
| `GG_RH` | `R:` | RH |
| `GG_Direction` | `T:` | Direction |

## Structure du dépôt

```text
infrastructure-reseau-multisite/
│
├── README.md
│
├── documentation/
│   ├── 01-architecture-reseau.md
│   ├── 02-installation-pfsense.md
│   ├── 03-routage-inter-sites.md
│   ├── 04-vpn-ipsec.md
│   ├── 05-dhcp.md
│   ├── 06-gpo-et-administration.md
│   ├── 07-active-directory-et-dns.md
│   ├── 08-profils-itinerants.md
│   ├── 09-partages-reseau.md
│   ├── 10-mappage-lecteurs-reseau.md
│   └── 11-tests-et-validation.md
│
├── schemas/
│   ├── architecture-reseau.png
│   ├── architecture-reseau.drawio
│   └── flux-reseau.png
│
├── captures/
│   ├── pfsense/
│   ├── vpn-ipsec/
│   ├── routage/
│   ├── dhcp/
│   │   ├── amiens/
│   │   └── paris/
│   ├── active-directory/
│   ├── dns/
│   ├── gpo/
│   ├── profils-itinerants/
│   └── partages-reseau/
│
└── .gitignore
```

## Documentation

La documentation technique détaillée est disponible dans le dossier [`documentation/`](documentation/).

Elle présente les différentes étapes de configuration, les choix techniques, les commandes utilisées, les problèmes rencontrés et les tests de validation.

## Tests et validation

Les tests réalisés portent notamment sur :

- La connectivité entre les deux sites.
- Le fonctionnement du tunnel VPN IPsec.
- L'attribution automatique des adresses IP.
- La résolution DNS et la jonction au domaine.
- L'application des stratégies de groupe.
- L'accès aux profils itinérants.
- Le contrôle des permissions sur les dossiers partagés.
- Le fonctionnement des lecteurs réseau depuis les deux sites.




---

*Projet réalisé dans le cadre de l'apprentissage de l'administration des réseaux, des systèmes et de la cybersécurité.*
