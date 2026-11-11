# 02 — pfSense et routage inter-sites

## 1. Objectif

Deux instances pfSense assurent la sécurité et le routage des réseaux locaux d'Amiens et de Paris. Elles servent également de points de terminaison au tunnel IPsec site-à-site.

## 2. Interfaces LAN

| Pare-feu | Réseau local | Adresse LAN |
|---|---|---|
| pfSense Amiens | `192.168.10.0/24` | `192.168.10.1` |
| pfSense Paris | `192.168.20.0/24` | `192.168.20.1` |

Chaque poste doit utiliser la passerelle de son propre site.

## 3. Routage

Le trafic à destination du réseau distant doit emprunter le tunnel IPsec. Les deux réseaux concernés sont :

- réseau local Amiens : `192.168.10.0/24` ;
- réseau local Paris : `192.168.20.0/24`.

Avec une configuration IPsec basée sur des sélecteurs de trafic, ces réseaux sont déclarés comme réseaux locaux et distants dans les paramètres de phase 2. Les routes et règles exactes dépendent du mode IPsec configuré dans le laboratoire.

## 4. Règles de filtrage

Les règles pfSense doivent autoriser uniquement les flux nécessaires entre les deux sites. Pour les services utilisés dans ce projet, les besoins peuvent notamment concerner :
- DNS vers `192.168.10.20` ;
- authentification et services Active Directory nécessaires à l'adhésion au domaine ;
- SMB vers le serveur de fichiers ;
- ICMP, uniquement lorsqu'il est utilisé pour les tests de connectivité.

Ne pas ouvrir tous les ports sans justification. Les ports AD dépendent des services activés et de la configuration du domaine.

## 5. Vérifications

Depuis un poste Paris, vérifier :
```cmd
ping 192.168.20.1
ping 192.168.10.20
nslookup srv-ad.lab.local 192.168.10.20
```

Un échec ICMP ne prouve pas nécessairement que le VPN est en panne : le filtrage peut bloquer les requêtes ping. Il faut également tester les services applicatifs concernés.

## 6. Paramètres à documenter depuis pfSense

| Paramètre | Valeur réelle à renseigner |
|---|---|
| Version pfSense Amiens | À compléter |
| Version pfSense Paris | À compléter |
| Interface WAN / réseau de transit | À compléter sans publier d'adresse publique sensible |
| Interface LAN Amiens | `192.168.10.1/24` |
| Interface LAN Paris | `192.168.20.1/24` |
| Routes statiques éventuelles | À vérifier |
| Règles LAN/IPsec | À compléter |
| NAT et exceptions éventuelles | À vérifier |

## 7. Captures à ajouter

- Tableau de bord de chaque pfSense (après masquage des informations sensibles).
- Configuration des interfaces LAN.
- Règles de pare-feu pertinentes.
- État du tunnel IPsec.
