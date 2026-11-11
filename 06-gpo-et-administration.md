# 06 — Stratégies de groupe et administration

## 1. Objectif

Les stratégies de groupe (GPO) permettent d'appliquer de manière centralisée des paramètres aux utilisateurs et aux ordinateurs du domaine.

Le laboratoire comporte déjà des stratégies de groupe configurées avant la réalisation du mappage des lecteurs. Cette documentation décrit le mécanisme et la GPO de lecteurs réseau précisément identifiée, sans inventer le contenu des autres GPO.

## 2. Organisation

Les OU du domaine distinguent les sites Amiens et Paris, puis les services IT, RH et Direction. Les GPO peuvent être liées à une OU ou au domaine selon le besoin.

Une GPO liée au domaine peut s'appliquer aux utilisateurs concernés dans les deux sites, sous réserve du filtrage de sécurité, de l'héritage et de l'emplacement des comptes.

## 3. GPO de lecteurs réseau

Nom : `GPO_Lecteurs_Reseau`

Emplacement de configuration :
`Configuration utilisateur > Préférences > Paramètres Windows > Mappages de lecteurs`

Cette GPO utilise le ciblage au niveau de l'élément selon l'appartenance à un groupe de sécurité AD.

| Groupe de sécurité | Lettre | Étiquette | Chemin UNC |
|---|---:|---|---|
| `GG_IT` | `I:` | IT | `\\srv-ad.lab.local\Partages\IT` |
| `GG_RH` | `R:` | Ressources Humaines | `\\srv-ad.lab.local\Partages\RH` |
| `GG_Direction` | `T:` | Direction | `\\srv-ad.lab.local\Partages\Direction` |

Action utilisée : **Mettre à jour**. Elle crée le lecteur s'il n'existe pas et met à jour sa configuration lors de l'application de la préférence.

L'option de reconnexion est activée.

## 4. Ciblage au niveau de l'élément

Chaque lecteur est associé à une condition de groupe de sécurité :
- lecteur I: : utilisateur membre de `GG_IT` ;
- lecteur R: : utilisateur membre de `GG_RH` ;
- lecteur T: : utilisateur membre de `GG_Direction`.

L'utilisateur reçoit ainsi le lecteur correspondant à son groupe.

## 5. Actualisation et vérification

Sur un poste client :
```cmd
gpupdate /force
gpresult /r
```

Une déconnexion/reconnexion peut être nécessaire pour que les changements d'appartenance aux groupes soient pris en compte dans le jeton d'ouverture de session.

## 6. Autres GPO

D'autres GPO de sécurité et de configuration ont été mises en place dans le laboratoire. Pour une documentation exhaustive, ajouter leurs noms, leur lien, leur périmètre et les paramètres exacts depuis la console Gestion de stratégie de groupe. Ne pas publier une liste de paramètres non vérifiée.
