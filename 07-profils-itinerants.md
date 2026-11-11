# 07 — Profils itinérants

## 1. Objectif

Les profils itinérants permettent de retrouver certains paramètres et éléments du profil utilisateur lorsqu'un utilisateur ouvre une session sur un autre poste du domaine, sous réserve de la configuration du profil et de la compatibilité des postes.

Dans ce laboratoire, les profils sont hébergés sur le serveur `srv-ad`.

## 2. Chemin de profil observé

Un exemple de chemin configuré dans le laboratoire est :

```text
\\srv-ad\Profils$\rayan.djabri.V6
```

Le partage `Profils$` est un partage masqué (le suffixe `$` le rend non visible lors d'une navigation classique des partages).

Le nom de dossier exact peut dépendre de la version du profil Windows. Le suffixe `.V6` est conservé ici parce qu'il a été observé dans le laboratoire ; il ne faut pas le généraliser à tous les profils.

## 3. Principe

1. L'utilisateur ouvre une session sur un poste membre du domaine.
2. Windows récupère le profil depuis le chemin réseau configuré.
3. Les paramètres pris en charge sont chargés dans la session.
4. À la fermeture de session, les changements du profil sont synchronisés vers le partage, selon les règles Windows et les éventuels conflits.

## 4. Vérifications

- Vérifier le chemin de profil dans les propriétés du compte utilisateur AD.
- Vérifier que le partage est accessible.
- Tester l'ouverture de session sur un autre poste du domaine.
- Créer un changement de test non critique et vérifier sa persistance après reconnexion.
- Consulter les journaux Windows en cas d'erreur de chargement ou de synchronisation.

## 5. Points de vigilance

- Les permissions de partage et NTFS doivent être cohérentes et sécurisées.
- Les profils itinérants peuvent être sensibles aux déconnexions réseau.
- Éviter les modifications simultanées du même profil sur plusieurs postes.
- Prévoir une sauvegarde du partage des profils.
- Ne pas publier de données personnelles contenues dans un profil utilisateur.
