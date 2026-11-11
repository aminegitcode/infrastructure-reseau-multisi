# 08 — Partages réseau et permissions

## 1. Objectif

Un partage centralisé est hébergé sur le serveur `srv-ad` afin de mettre à disposition des dossiers propres aux services IT, RH et Direction.

Les autorisations sont gérées avec des groupes de sécurité AD. La sécurité effective résulte de la combinaison des permissions de partage SMB et des permissions NTFS.

## 2. Arborescence sur le serveur

```text
C:\Partages
├── IT
├── RH
└── Direction
```

Le dossier parent est partagé sous le nom `Partages`.

Chemin UNC :
```text
\\srv-ad.lab.local\Partages
```

## 3. Groupes utilisés

| Groupe | Dossier associé |
|---|---|
| `GG_IT` | `C:\Partages\IT` |
| `GG_RH` | `C:\Partages\RH` |
| `GG_Direction` | `C:\Partages\Direction` |

Les utilisateurs sont ajoutés au groupe correspondant à leur service.

## 4. Permissions SMB

Sur le partage parent `Partages`, les groupes `GG_IT`, `GG_RH` et `GG_Direction` disposent des droits de **Lecture** et **Modification** au niveau du partage. Le groupe Administrateurs conserve le contrôle total.

Ces permissions de partage autorisent l'accès réseau général au partage, mais ne donnent pas à elles seules le droit d'ouvrir tous les sous-dossiers.

## 5. Permissions NTFS

Les permissions NTFS sont définies individuellement sur chaque dossier de service.

| Dossier | Groupe | Permission NTFS |
|---|---|---|
| `IT` | `GG_IT` | Modification |
| `RH` | `GG_RH` | Modification |
| `Direction` | `GG_Direction` | Modification |

L'héritage a été désactivé sur les dossiers concernés et les autorisations héritées ont été converties en autorisations explicites avant le nettoyage des entrées trop larges. Les entrées système et administrateurs ont été conservées, ainsi que `CREATOR OWNER` selon la configuration réalisée.

> Vérifier les ACL finales sur chaque dossier avant publication : les permissions effectives sont déterminées par toutes les entrées applicables, les appartenances aux groupes et les éventuelles autorisations héritées/explicites.

## 6. Résultat des tests

Depuis le client Windows de Paris :
- le partage `\\srv-ad.lab.local\Partages` est accessible ;
- les trois dossiers peuvent être visibles dans l'explorateur ;
- un utilisateur ne peut ouvrir que le dossier autorisé à son groupe ;
- l'accès aux dossiers non autorisés est refusé.

La visibilité d'un nom de dossier ne signifie donc pas que l'utilisateur possède le droit de lire son contenu.

## 7. Bonnes pratiques

- Attribuer les droits aux groupes, pas directement à chaque utilisateur.
- Appliquer le principe du moindre privilège.
- Éviter d'accorder `Tout le monde` ou `Utilisateurs du domaine` en modification sur les dossiers sensibles.
- Tester avec un compte de chaque service.
- Documenter les ACL effectives et conserver une sauvegarde des données.
