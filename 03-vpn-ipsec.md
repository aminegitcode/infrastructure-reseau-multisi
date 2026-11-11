# 03 — VPN IPsec site-à-site

## 1. Objectif

Le VPN IPsec relie les deux réseaux privés du laboratoire. Il permet au poste du site de Paris d'accéder aux services hébergés à Amiens sans exposer directement les ressources internes sur Internet.

## 2. Réseaux concernés

| Extrémité | Réseau |
|---|---|
| Site Amiens | `192.168.10.0/24` |
| Site Paris | `192.168.20.0/24` |

Les deux pare-feu pfSense sont les extrémités du tunnel.

## 3. Principe de fonctionnement

IPsec protège les communications IP entre les deux passerelles. Dans une configuration classique IKE avec ESP :
- la **phase 1** établit une association de sécurité et authentifie les pairs ;
- la **phase 2** définit les flux protégés entre les réseaux privés.

Les paramètres cryptographiques exacts du laboratoire doivent être copiés depuis pfSense ; ils ne sont pas supposés dans cette documentation.

## 4. Résultat observé

Le tunnel site-à-site a été configuré et le client Windows de Paris a pu communiquer avec le serveur d'Amiens. L'accès aux services de domaine et aux partages réseau depuis Paris a ensuite été validé.

## 5. Tests

Depuis le client de Paris :
```cmd
ping 192.168.10.20
nslookup srv-ad.lab.local 192.168.10.20
```

Tester ensuite les services réels, par exemple l'ouverture du partage :
```text
\\srv-ad.lab.local\Partages
```

## 6. Informations à compléter

| Paramètre | Amiens | Paris |
|---|---|---|
| Adresse de pair | À renseigner de façon anonymisée | À renseigner de façon anonymisée |
| Version IKE | À relever | À relever |
| Authentification | À relever | À relever |
| Chiffrement / intégrité | À relever | À relever |
| Groupe Diffie-Hellman | À relever | À relever |
| Durée de vie phase 1 | À relever | À relever |
| Réseau local phase 2 | `192.168.10.0/24` | `192.168.20.0/24` |
| Réseau distant phase 2 | `192.168.20.0/24` | `192.168.10.0/24` |
| Durée de vie phase 2 | À relever | À relever |

**Sécurité :** ne jamais publier la clé prépartagée (PSK), les secrets, les certificats privés ou les fichiers de configuration non expurgés.

## 7. Captures à ajouter

- État « établi » du tunnel.
- Paramètres non sensibles de phase 1 et phase 2.
- Compteurs de trafic ou journaux expurgés.
