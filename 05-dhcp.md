# 05 — DHCP

## 1. Objectif

Le protocole DHCP fournit automatiquement aux clients leur configuration IPv4 : adresse, masque, passerelle et serveur DNS.

## 2. Configuration connue

Le service DHCP du site de Paris est assuré par **pfSense Paris**. Le client Windows de Paris a obtenu une adresse observée `192.168.20.105` dans le réseau `192.168.20.0/24`.

| Option | Valeur connue |
|---|---|
| Réseau Paris | `192.168.20.0/24` |
| Passerelle | `192.168.20.1` |
| DNS | `192.168.10.20` |
| Suffixe DNS | `lab.local` |
| Adresse du client observée | `192.168.20.105` |

La plage DHCP exacte n'est pas indiquée ici : elle doit être relevée dans pfSense.

## 3. DNS et domaine

Le point important pour un poste membre du domaine est qu'il utilise le DNS Active Directory, ici `192.168.10.20`, et non un DNS public directement. Le DNS AD sait résoudre les enregistrements nécessaires à `lab.local`.

## 4. Vérification côté client

```cmd
ipconfig /all
ipconfig /release
ipconfig /renew
ipconfig /flushdns
```

Vérifier :
- l'adresse IPv4 ;
- le masque ;
- la passerelle par défaut ;
- le serveur DNS ;
- le suffixe DNS.

## 5. Paramètres à compléter

| Paramètre | Valeur réelle |
|---|---|
| Plage DHCP Paris | À relever dans pfSense |
| Durée du bail | À relever |
| Réservations DHCP | À vérifier |
| Options DHCP supplémentaires | À relever |
| DHCP Amiens | À documenter uniquement si un service DHCP y est effectivement configuré |

Ne pas présenter un serveur DHCP Windows ou Ubuntu comme actif sans vérifier l'architecture réellement utilisée.
