# Carnet d'entraînement

Carnet de séances pour la salle (calisthénie et musculation), pensé pour iPhone.
Le site est une simple page statique hébergée sur GitHub Pages ; les données sont
enregistrées dans Firebase (Firestore), un compte par personne.

## Fichiers

| Fichier | Rôle |
|---|---|
| `index.html` | Le site complet (interface + logique). |
| `icon.png` | Icône pour l'écran d'accueil de l'iPhone. |
| `firestore.rules` | Règles de sécurité à coller dans Firebase. |

## Mise en route (environ 15 minutes)

Les noms de menus de la console Firebase peuvent varier légèrement.

### 1. Créer le projet Firebase

1. Va sur <https://console.firebase.google.com> et crée un projet (Google Analytics n'est pas nécessaire).
2. Le forfait gratuit (Spark) suffit largement pour deux personnes.

### 2. Créer les comptes

Sur le site, on se connecte avec un **nom d'utilisateur** et un mot de passe. Firebase, lui,
identifie chaque compte par une adresse : le site ajoute donc tout seul `@<authDomain>` derrière
le nom d'utilisateur. Cette adresse est purement technique, aucune boîte mail n'existe derrière.

1. Menu **Authentication** → **Sign-in method** → active **E-mail/Mot de passe**.
2. Onglet **Users** → **Add user**. Pour le nom d'utilisateur `matteo`, l'identifiant à saisir est
   `matteo@carnet-entrainement-14c07.firebaseapp.com` (minuscules, sans accent ni espace).

Le site n'a volontairement pas de page d'inscription : seuls les comptes créés ici peuvent se connecter.

### 3. Créer la base

1. Menu **Firestore Database** → **Create database** → mode **production** → une région en Europe.
2. Onglet **Rules** : remplace tout par le contenu de `firestore.rules`, puis **Publish**.

Ces règles font que chaque personne connectée ne lit et n'écrit que ses propres données.

### 4. Relier le site au projet

1. **Project settings** (roue dentée) → **Your apps** → icône **Web** `</>` → enregistre une application.
2. Firebase affiche un bloc `firebaseConfig`. Ses six valeurs vont dans `FIREBASE_CONFIG`, tout en haut du script de `index.html` (déjà fait pour ce projet).

### 5. Mettre en ligne sur GitHub Pages

1. Crée un dépôt GitHub et dépose `index.html` et `icon.png` à la racine.
2. Dans le dépôt : **Settings** → **Pages** → **Deploy from a branch** → branche `main`, dossier `/ (root)`.
3. Après une minute, le site est à l'adresse `https://TON-PSEUDO.github.io/NOM-DU-DEPOT/`.
4. Dans Firebase : **Authentication** → **Settings** → **Authorized domains** → ajoute `TON-PSEUDO.github.io`.

### 6. Sur l'iPhone

Ouvre l'adresse dans Safari, connecte-toi, puis **Partager** → **Sur l'écran d'accueil**.
Le carnet s'ouvre alors comme une appli. Au premier lancement, charge un programme
tout prêt (calisthénie ou musculation) ou crée tes séances.

## Bon à savoir

- **La clé `apiKey` est publique par conception.** Elle identifie le projet, elle ne donne
  aucun accès : ce sont les règles Firestore et la connexion qui protègent les données.
  GitHub peut envoyer une alerte « clé Google détectée » ; c'est attendu pour Firebase.
- **Hors connexion.** Une séance terminée sans réseau reste en attente sur le téléphone
  et part vers la base dès que la connexion revient.
- **Mot de passe oublié.** Il se réinitialise depuis la console Firebase (Authentication → Users).

## Où sont rangées les données

```
users/{uid}/routines/{id}   une séance type : nom, ordre, exercices
users/{uid}/workouts/{id}   un entraînement terminé : date, durée, séries validées
```

Chaque exercice a un mode de mesure : `reps` (répétitions), `poids` (kg × répétitions),
`assist` (kg d'assistance × répétitions) ou `temps` (secondes).
