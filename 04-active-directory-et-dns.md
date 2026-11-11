# 04 — Active Directory et DNS

## 1. Présentation

Le site d'Amiens héberge le serveur `srv-ad.lab.local`, configuré avec l'adresse statique `192.168.10.20/24`. Il fournit les services Active Directory Domain Services (AD DS) et DNS pour le domaine `lab.local`.

| Paramètre | Valeur |
|---|---|
| Nom du serveur | `srv-ad` |
| FQDN | `srv-ad.lab.local` |
| Domaine AD | `lab.local` |
| Adresse IPv4 | `192.168.10.20/24` |
| Passerelle | `192.168.10.1` |
| DNS utilisé par le domaine | `192.168.10.20` |

## 2. Rôle d'Active Directory

Active Directory centralise les objets du domaine :
- comptes utilisateurs ;
- groupes de sécurité ;
- comptes ordinateurs ;
- unités d'organisation (OU) ;
- stratégies de groupe associées.

Le domaine permet aux utilisateurs de s'authentifier sur les postes membres et d'accéder aux ressources selon leurs autorisations.

## 3. Organisation des unités d'organisation

L'organisation observée dans le laboratoire comprend les OU suivantes :

```text
lab.local
├── Amiens
│   ├── IT
│   ├── RH
│   └── Direction
├── Paris
│   ├── IT
│   ├── RH
│   └── Direction
├── Users
└── Computers
```

Cette structure permet de distinguer les utilisateurs et/ou ordinateurs par site et par service. Les objets doivent être placés dans les OU adaptées pour que les stratégies liées à ces OU s'appliquent comme prévu.

## 4. Groupes de sécurité

Les groupes globaux de sécurité suivants ont été créés pour gérer les accès par service :

| Groupe | Usage |
|---|---|
| `GG_IT` | Accès au dossier Informatique |
| `GG_RH` | Accès au dossier Ressources humaines |
| `GG_Direction` | Accès au dossier Direction |

Les comptes utilisateurs sont membres du groupe correspondant à leur service. Les droits sont attribués aux groupes plutôt qu'individuellement aux utilisateurs.

## 5. DNS

Le serveur DNS AD héberge la résolution du domaine `lab.local`. Le client de Paris utilise `192.168.10.20` comme serveur DNS afin de résoudre les enregistrements nécessaires à la découverte du domaine et à l'authentification.

Commandes de vérification :
```cmd
ipconfig /all
nslookup srv-ad.lab.local
nslookup lab.local
```

La résolution du nom `srv-ad.lab.local` doit retourner `192.168.10.20`.

## 6. Adhésion du client Paris au domaine

Le poste Windows de Paris a rejoint le domaine `lab.local`. Son adresse observée pendant les tests était `192.168.20.105`.

Après l'adhésion, l'authentification sur le domaine et l'application des stratégies de groupe ont été testées.

## 7. Contrôles utiles

Sur un poste client :
```cmd
whoami
echo %USERDOMAIN%
echo %LOGONSERVER%
gpresult /r
```

Sur le serveur, les outils d'administration AD et DNS permettent de vérifier les objets, les zones et les enregistrements.

## 8. Limite connue

Le laboratoire utilise un seul contrôleur de domaine documenté : `srv-ad`. Aucun second contrôleur de domaine n'est inclus dans le périmètre actuel. Il n'y a donc pas de redondance AD/DNS documentée.
