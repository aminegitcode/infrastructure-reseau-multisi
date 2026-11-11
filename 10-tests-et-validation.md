# 10 — Tests et validation

## 1. Objectif

Cette section rassemble les tests fonctionnels utilisés pour vérifier les principaux services du laboratoire.

## 2. Connectivité locale et inter-sites

Depuis le client Paris :
```cmd
ipconfig /all
ping 192.168.20.1
ping 192.168.10.20
```

Résultat attendu :
- passerelle locale joignable ;
- serveur du site Amiens joignable si ICMP est autorisé ;
- adresse, masque, passerelle et DNS cohérents avec le réseau Paris.

## 3. DNS et domaine

```cmd
nslookup srv-ad.lab.local 192.168.10.20
whoami
echo %USERDOMAIN%
gpresult /r
```

Résultat observé : le client Paris a pu résoudre le serveur AD/DNS et rejoindre le domaine `lab.local`.

## 4. Partages réseau

Depuis l'explorateur Windows ou la boîte Exécuter :
```text
\\srv-ad.lab.local\Partages
```

Résultat observé :
- accès au partage parent depuis Paris ;
- visibilité des dossiers de service ;
- ouverture autorisée uniquement pour le dossier correspondant aux droits de l'utilisateur.

## 5. Mappage des lecteurs

Actualiser les stratégies :
```cmd
gpupdate /force
```

Vérifier dans l'explorateur les lecteurs correspondant au groupe de l'utilisateur :
- `I:` pour `GG_IT` ;
- `R:` pour `GG_RH` ;
- `T:` pour `GG_Direction`.

Les tests réalisés avec les comptes des différents services ont confirmé le fonctionnement attendu.

## 6. Tableau de synthèse

| Fonctionnalité | État |
|---|---|
| Réseaux locaux Amiens et Paris | Configurés |
| VPN IPsec site-à-site | Configuré et testé |
| Communication Paris vers serveur Amiens | Testée |
| Active Directory / DNS | Opérationnels dans le périmètre testé |
| Client Paris joint au domaine | Validé |
| DHCP Paris par pfSense | Configuré |
| GPO | Appliquées sur le client testé |
| Profils itinérants | Configurés et testés dans le laboratoire |
| Partages SMB et permissions NTFS | Testés |
| Mappage des lecteurs par groupes | Testé avec les comptes des services |

## 7. Limites identifiées

- Un seul contrôleur de domaine est présent dans le périmètre documenté.
- La haute disponibilité AD/DNS n'est pas mise en œuvre.
- Les paramètres cryptographiques précis du VPN restent à relever depuis pfSense.
- La plage DHCP exacte et les durées de bail restent à documenter.
- Les sauvegardes et procédures de restauration ne sont pas décrites comme validées.

## 8. Captures recommandées

Pour rendre le dépôt convaincant, ajouter des captures réelles et expurgées :
1. topologie générale ;
2. interfaces LAN des deux pfSense ;
3. tunnel IPsec établi ;
4. DNS et configuration IPv4 du serveur ;
5. structure des OU et groupes AD ;
6. configuration DHCP de Paris ;
7. GPO `GPO_Lecteurs_Reseau` ;
8. ciblage par groupe de sécurité ;
9. permissions de partage et NTFS ;
10. explorateur du client montrant les lecteurs mappés.

Chaque capture doit être accompagnée d'une phrase expliquant ce qu'elle démontre.
