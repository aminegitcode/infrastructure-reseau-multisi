# 09 — Mappage automatique des lecteurs réseau

## 1. Objectif

Les utilisateurs reçoivent automatiquement un lecteur réseau correspondant à leur service lorsqu'ils ouvrent une session sur un poste membre du domaine.

Le mappage est réalisé avec les Préférences de stratégie de groupe et le ciblage au niveau de l'élément.

## 2. GPO

Nom : `GPO_Lecteurs_Reseau`

Chemin :
`Configuration utilisateur > Préférences > Paramètres Windows > Mappages de lecteurs`

La GPO a été liée au domaine `lab.local` afin de couvrir les utilisateurs des deux sites. Le ciblage de chaque élément décide quel lecteur doit être appliqué à l'utilisateur.

## 3. Mappages

| Service | Groupe de sécurité | Lettre | Nom affiché | Chemin |
|---|---|---|---|---|
| Informatique | `GG_IT` | `I:` | `IT` | `\\srv-ad.lab.local\Partages\IT` |
| Ressources humaines | `GG_RH` | `R:` | `Ressources Humaines` | `\\srv-ad.lab.local\Partages\RH` |
| Direction | `GG_Direction` | `T:` | `Direction` | `\\srv-ad.lab.local\Partages\Direction` |

Paramètres utilisés :
- Action : **Mettre à jour**.
- Reconnecter : activé.
- Ciblage au niveau de l'élément : activé.
- Type de ciblage : groupe de sécurité.
- Condition : l'utilisateur est membre du groupe correspondant.

## 4. Application

Après modification de la GPO, actualiser les stratégies sur le client :
```cmd
gpupdate /force
```

Si nécessaire, fermer puis rouvrir la session utilisateur. L'utilisateur doit disposer d'un jeton d'ouverture de session actualisé pour que les nouvelles appartenances aux groupes soient prises en compte.

## 5. Tests réalisés

Les tests ont confirmé que les lecteurs attendus sont attribués aux utilisateurs selon leur groupe de sécurité et que l'accès au contenu reste soumis aux permissions du partage et aux ACL NTFS.

Le nom affiché dans l'explorateur est une étiquette conviviale. Windows peut également afficher le chemin réseau sous le nom du lecteur : cela ne signifie pas que le chemin est incorrect.

## 6. Dépannage

Si un lecteur n'apparaît pas :
1. vérifier que l'utilisateur appartient au bon groupe AD ;
2. vérifier le ciblage de l'élément dans la GPO ;
3. vérifier le chemin UNC ;
4. tester l'accès direct au chemin UNC ;
5. exécuter `gpupdate /force` ;
6. vérifier le résultat avec `gpresult /r` ;
7. fermer puis rouvrir la session.
