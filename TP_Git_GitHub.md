# TP : Maîtriser Git et GitHub – Du Débutant à l'Avancé

## Contexte

Ce TP utilise le projet **tp-api-nodejs** (API REST Node.js + MongoDB) réalisé lors du TP précédent.
Vous allez apprendre à versionner, collaborer et gérer ce projet avec Git et GitHub.

## Objectifs pédagogiques

- Comprendre les concepts fondamentaux de Git
- Maîtriser les commandes essentielles
- Travailler avec GitHub (remote, pull requests, collaboration)
- Gérer les branches, les conflits et les workflows avancés

## Durée : 3 heures

---

# 📋 PARTIE 1 : Installation et Configuration

## Étape 1.1 : Installer Git

1. Télécharger Git : https://git-scm.com/download/win
2. Installer avec les options par défaut
3. Ouvrir **Git Bash** (ou PowerShell) et vérifier :

```bash
git --version
```

**❓ Question :** Quelle version de Git s'affiche ? `git version ____.__.__`

> 💡 **Git** est un système de contrôle de version distribué. Il permet de suivre l'historique des modifications d'un projet, de collaborer à plusieurs, et de revenir en arrière en cas d'erreur.

---

## Étape 1.2 : Configurer votre identité

Git a besoin de savoir **qui** fait les modifications. Ces informations seront attachées à chaque commit.

```bash
git config --global user.name "Votre Nom Complet"
git config --global user.email "votre.email@ecole.tn"
```

Vérifiez la configuration :

```bash
git config --global --list
```

**Résultat attendu :**
```
user.name=Votre Nom Complet
user.email=votre.email@ecole.tn
```

> 💡 `--global` applique cette configuration à **tous** vos projets. Sans `--global`, cela s'applique uniquement au projet courant.

---

## Étape 1.3 : Configurer l'éditeur par défaut (optionnel)

```bash
git config --global core.editor "code --wait"
```

> 💡 Cela configure VS Code comme éditeur par défaut pour Git (messages de commit, résolution de conflits, etc.).

---

## Étape 1.4 : Créer un compte GitHub

1. Allez sur https://github.com
2. Créez un compte (ou connectez-vous)
3. Notez votre nom d'utilisateur GitHub : `______________`

---

# 🏁 PARTIE 2 : Initialiser un dépôt Git

## Étape 2.1 : Se placer dans le projet

Ouvrez un terminal et naviguez vers votre projet du TP précédent :

```bash
cd tp-api-nodejs
```

> ⚠️ Si vous n'avez pas le projet précédent, créez-le rapidement :
> ```bash
> mkdir tp-api-nodejs
> cd tp-api-nodejs
> npm init -y
> npm install express mongoose dotenv
> ```

---

## Étape 2.2 : Initialiser Git

```bash
git init
```

**Résultat attendu :**
```
Initialized empty Git repository in .../tp-api-nodejs/.git/
```

Vérifiez que le dossier `.git` a été créé :

```bash
ls -la
```

> 💡 **`git init`** crée un dossier caché `.git/` qui contient tout l'historique du projet. **Ne supprimez jamais ce dossier !**

---

## Étape 2.3 : Vérifier l'état du dépôt

```bash
git status
```

**Résultat attendu :**
```
On branch main
No commits yet
Untracked files:
  .env
  config/
  controllers/
  models/
  node_modules/
  package.json
  ...
```

> 💡 **`git status`** montre :
> - Sur quelle branche vous êtes
> - Les fichiers modifiés, ajoutés ou non suivis
> - **C'est la commande la plus utilisée !** Prenez l'habitude de la lancer souvent.

---

## Étape 2.4 : Créer le fichier `.gitignore`

Certains fichiers ne doivent **jamais** être versionnés. Créez un fichier `.gitignore` :

```gitignore
# Dépendances Node.js (trop volumineux, installable via npm install)
node_modules/

# Variables d'environnement (contient des données sensibles)
.env

# Logs
*.log

# Fichiers système
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/
```

Vérifiez l'effet :

```bash
git status
```

**❓ Question :** Le dossier `node_modules/` apparaît-il encore dans la liste ? Pourquoi ?

> 💡 **`.gitignore`** dit à Git d'ignorer certains fichiers. Règles importantes :
> - `node_modules/` : Trop lourd et recréable avec `npm install`
> - `.env` : Contient des mots de passe et informations sensibles
> - Ne commitez **JAMAIS** de mots de passe ou clés API !

---

## Étape 2.5 : Comprendre les 3 zones de Git

```
┌──────────────────┐    git add     ┌──────────────────┐   git commit    ┌──────────────────┐
│  Working Dir     │ ────────────►  │  Staging Area    │ ────────────►   │  Repository      │
│  (vos fichiers)  │                │  (zone de        │                 │  (historique)     │
│                  │  git restore   │   préparation)   │  git reset      │                  │
│                  │ ◄──────────── │                  │ ◄────────────   │                  │
└──────────────────┘                └──────────────────┘                 └──────────────────┘
```

> 💡 **Trois zones :**
> 1. **Working Directory** : Vos fichiers tels qu'ils sont sur le disque
> 2. **Staging Area (Index)** : Zone de préparation – les fichiers prêts à être commitiés
> 3. **Repository** : L'historique sauvegardé (les commits)

---

# 📸 PARTIE 3 : Premiers Commits

## Étape 3.1 : Ajouter des fichiers à la staging area

Commençons par ajouter **un seul fichier** pour comprendre :

```bash
git add package.json
git status
```

**Résultat attendu :**
```
Changes to be committed:
  new file:   package.json

Untracked files:
  .gitignore
  config/
  controllers/
  ...
```

> 💡 **`git add <fichier>`** déplace un fichier vers la staging area. Il est prêt à être "photographié" (commitié).

---

## Étape 3.2 : Premier commit !

```bash
git commit -m "Initialisation du projet: ajout de package.json"
```

**Résultat attendu :**
```
[main (root-commit) abc1234] Initialisation du projet: ajout de package.json
 1 file changed, 20 insertions(+)
 create mode 100644 package.json
```

> 💡 **`git commit`** prend une "photo" (snapshot) de tout ce qui est dans la staging area.
> - `-m "message"` : Ajoute un message descriptif
> - Chaque commit a un **identifiant unique** (hash SHA-1), par ex: `abc1234`

**❓ Question :** Quel est le hash de votre premier commit ? `____________`

---

## Étape 3.3 : Ajouter les fichiers de configuration

```bash
git add .gitignore
git commit -m "Ajout du fichier .gitignore"
```

```bash
git add config/database.js
git commit -m "Ajout de la configuration de la base de données MongoDB"
```

> 💡 **Bonne pratique :** Faites des commits **petits et fréquents** avec des messages clairs. Un commit = une modification logique.

---

## Étape 3.4 : Ajouter le modèle

```bash
git add models/Etudiant.js
git commit -m "Ajout du modèle Etudiant avec schéma Mongoose"
```

---

## Étape 3.5 : Ajouter le contrôleur

```bash
git add controllers/etudiantController.js
git commit -m "Ajout du contrôleur CRUD pour les étudiants"
```

---

## Étape 3.6 : Ajouter les routes et le serveur

```bash
git add routes/etudiantRoutes.js server.js
git commit -m "Ajout des routes et du serveur Express"
```

> 💡 Vous pouvez ajouter **plusieurs fichiers** en une seule commande `git add`.

---

## Étape 3.7 : Vérifier l'historique

```bash
git log
```

**Résultat attendu :**
```
commit 5e6f7g8 (HEAD -> main)
Author: Votre Nom <votre.email@ecole.tn>
Date:   ...
    Ajout des routes et du serveur Express

commit 4d5e6f7
Author: Votre Nom <votre.email@ecole.tn>
Date:   ...
    Ajout du contrôleur CRUD pour les étudiants
...
```

Version compacte :

```bash
git log --oneline
```

**Résultat attendu :**
```
5e6f7g8 (HEAD -> main) Ajout des routes et du serveur Express
4d5e6f7 Ajout du contrôleur CRUD pour les étudiants
3c4d5e6 Ajout du modèle Etudiant avec schéma Mongoose
2b3c4d5 Ajout de la configuration de la base de données MongoDB
1a2b3c4 Ajout du fichier .gitignore
0z1a2b3 Initialisation du projet: ajout de package.json
```

Version graphique :

```bash
git log --oneline --graph --all
```

**❓ Question :** Combien de commits avez-vous au total ? `____`

---

# 🌐 PARTIE 4 : Travailler avec GitHub

## Étape 4.1 : Créer un dépôt sur GitHub

1. Allez sur https://github.com
2. Cliquez sur **"New repository"** (bouton vert "+" en haut à droite)
3. Remplissez :
   - **Repository name :** `tp-api-nodejs`
   - **Description :** `TP API REST avec Node.js et MongoDB`
   - **Visibility :** Public
   - ⚠️ **NE cochez PAS** "Add a README file" (on a déjà un dépôt local)
4. Cliquez sur **"Create repository"**

---

## Étape 4.2 : Connecter le dépôt local au dépôt distant

GitHub vous affiche des commandes. Exécutez :

```bash
git remote add origin https://github.com/VOTRE_USERNAME/tp-api-nodejs.git
```

Vérifiez la connexion :

```bash
git remote -v
```

**Résultat attendu :**
```
origin  https://github.com/VOTRE_USERNAME/tp-api-nodejs.git (fetch)
origin  https://github.com/VOTRE_USERNAME/tp-api-nodejs.git (push)
```

> 💡 **`origin`** est le nom conventionnel du dépôt distant principal. `remote` = dépôt hébergé en ligne.

---

## Étape 4.3 : Pousser le code sur GitHub

```bash
git branch -M main
git push -u origin main
```

> 💡 **Explication :**
> - `branch -M main` : Renomme la branche courante en `main`
> - `push` : Envoie les commits vers le dépôt distant
> - `-u origin main` : Lie la branche locale `main` à `origin/main` (une seule fois)

**Allez sur GitHub** et rafraîchissez la page de votre dépôt. Vos fichiers sont en ligne ! 🎉

**❓ Question :** Voyez-vous les mêmes fichiers que localement ? Le dossier `node_modules/` est-il présent sur GitHub ? Pourquoi ?

---

## Étape 4.4 : Ajouter un README sur GitHub

1. Sur GitHub, cliquez sur **"Add a README"**
2. Écrivez :

```markdown
# TP API Node.js + MongoDB

API REST de gestion d'étudiants développée avec Node.js, Express et MongoDB.

## Installation

git clone https://github.com/VOTRE_USERNAME/tp-api-nodejs.git
cd tp-api-nodejs
npm install

## Lancement

npm run dev
```

3. Commitez directement sur GitHub (bouton vert "Commit changes")

---

## Étape 4.5 : Récupérer les modifications distantes

Le README a été créé sur GitHub mais n'existe pas localement. Récupérez-le :

```bash
git pull origin main
```

Vérifiez :

```bash
ls
cat README.md
```

> 💡 **`git pull`** récupère les modifications depuis le dépôt distant et les fusionne avec votre branche locale. C'est l'inverse de `git push`.

---

# 🌿 PARTIE 5 : Les Branches

## Étape 5.1 : Comprendre les branches

```
                  feature-search
                 /               \
    main:  ──●──●──●──────────────●──●──  (historique principal)
                    \            /
                     feature-stats
```

> 💡 **Une branche** est une ligne de développement indépendante. Elle permet de :
> - Travailler sur une fonctionnalité sans casser le code principal
> - Plusieurs développeurs travaillent en parallèle
> - Tester des idées sans risque

---

## Étape 5.2 : Voir les branches existantes

```bash
git branch
```

**Résultat attendu :**
```
* main
```

> 💡 L'étoile `*` indique la branche sur laquelle vous êtes.

---

## Étape 5.3 : Créer une branche pour la fonctionnalité de recherche

```bash
git branch feature-search
```

Vérifiez :

```bash
git branch
```

**Résultat attendu :**
```
  feature-search
* main
```

---

## Étape 5.4 : Basculer sur la nouvelle branche

```bash
git checkout feature-search
```

**Résultat attendu :**
```
Switched to branch 'feature-search'
```

> 💡 **Raccourci :** Créer + basculer en une seule commande :
> ```bash
> git checkout -b nom-de-branche
> ```

Vérifiez :

```bash
git branch
```

**Résultat attendu :**
```
* feature-search
  main
```

---

## Étape 5.5 : Faire des modifications sur la branche

Ouvrez `controllers/etudiantController.js` et ajoutez cette fonction **à la fin** du fichier, juste avant la dernière accolade (si elle existe) ou à la toute fin :

```javascript
// Recherche avancée avec filtres multiples
exports.advancedSearch = async (req, res) => {
    try {
        const { nom, filiere, anneeMin, anneeMax, moyenneMin } = req.query;
        let filter = { actif: true };

        if (nom) filter.nom = new RegExp(nom, 'i');
        if (filiere) filter.filiere = filiere;
        if (anneeMin || anneeMax) {
            filter.annee = {};
            if (anneeMin) filter.annee.$gte = parseInt(anneeMin);
            if (anneeMax) filter.annee.$lte = parseInt(anneeMax);
        }
        if (moyenneMin) filter.moyenne = { $gte: parseFloat(moyenneMin) };

        const etudiants = await Etudiant.find(filter);

        res.status(200).json({
            success: true,
            count: etudiants.length,
            filters: req.query,
            data: etudiants
        });
    } catch (error) {
        res.status(500).json({
            success: false,
            message: 'Erreur serveur',
            error: error.message
        });
    }
};
```

---

## Étape 5.6 : Commiter sur la branche

```bash
git add controllers/etudiantController.js
git status
```

**❓ Question :** Sur quelle branche êtes-vous (indiqué en haut du `git status`) ?

```bash
git commit -m "Ajout de la recherche avancée avec filtres multiples"
```

---

## Étape 5.7 : Comparer les branches

```bash
git log --oneline
```

Ce commit est visible ici (branche `feature-search`).

Retournez sur `main` et vérifiez :

```bash
git checkout main
git log --oneline
```

**❓ Question :** Le commit "Ajout de la recherche avancée" apparaît-il sur `main` ? Pourquoi ?

> 💡 Chaque branche a son propre historique. Le commit n'existe que sur `feature-search` pour l'instant.

---

## Étape 5.8 : Fusionner (merge) la branche dans `main`

Assurez-vous d'être sur `main` :

```bash
git checkout main
git merge feature-search
```

**Résultat attendu :**
```
Updating abc1234..def5678
Fast-forward
 controllers/etudiantController.js | 25 +++++++++++++++++++++++++
 1 file changed, 25 insertions(+)
```

Vérifiez :

```bash
git log --oneline --graph --all
```

> 💡 **`git merge`** intègre les modifications d'une branche dans la branche courante.
> - **Fast-forward** : Fusion simple, pas de divergence
> - **Merge commit** : Si les deux branches ont divergé, Git crée un commit de fusion

---

## Étape 5.9 : Supprimer la branche fusionnée

Une fois fusionnée, la branche n'est plus nécessaire :

```bash
git branch -d feature-search
git branch
```

**Résultat attendu :**
```
Deleted branch feature-search (was def5678).
* main
```

---

## Étape 5.10 : Pousser les changements

```bash
git push origin main
```

---

# ⚡ PARTIE 6 : Gérer les Conflits

## Étape 6.1 : Comprendre les conflits

Un conflit survient quand **deux branches modifient la même ligne** du même fichier. Git ne sait pas quelle version garder.

> 💡 Les conflits sont **normaux** et font partie du quotidien du développeur. Pas de panique !

---

## Étape 6.2 : Simuler un conflit

**Créer une branche A :**

```bash
git checkout -b branche-a
```

Modifiez la **première ligne** du fichier `server.js`. Remplacez :

```javascript
const express = require('express');
```

Par :

```javascript
// Branche A : Serveur Express principal
const express = require('express');
```

```bash
git add server.js
git commit -m "Branche A: ajout commentaire serveur"
```

**Retourner sur `main` et créer une branche B :**

```bash
git checkout main
git checkout -b branche-b
```

Modifiez la **même première ligne** de `server.js` :

```javascript
// Branche B : Application Express
const express = require('express');
```

```bash
git add server.js
git commit -m "Branche B: ajout commentaire application"
```

---

## Étape 6.3 : Provoquer le conflit

Fusionnez d'abord la branche A dans main :

```bash
git checkout main
git merge branche-a
```

✅ Ça fonctionne (fast-forward).

Maintenant, fusionnez la branche B :

```bash
git merge branche-b
```

**Résultat attendu :**
```
Auto-merging server.js
CONFLICT (content): Merge conflict in server.js
Automatic merge failed; fix conflicts and then commit the result.
```

🚨 **CONFLIT !**

---

## Étape 6.4 : Voir le conflit

```bash
git status
```

Ouvrez `server.js`. Vous verrez :

```
<<<<<<< HEAD
// Branche A : Serveur Express principal
=======
// Branche B : Application Express
>>>>>>> branche-b
const express = require('express');
```

> 💡 **Lecture du conflit :**
> - `<<<<<<< HEAD` : Version de la branche courante (main, qui contient branche-a)
> - `=======` : Séparateur
> - `>>>>>>> branche-b` : Version de la branche qu'on essaie de fusionner

---

## Étape 6.5 : Résoudre le conflit

Modifiez manuellement le fichier pour garder ce que vous voulez. Par exemple, combinez les deux :

```javascript
// Serveur Express principal - Application de gestion des étudiants
const express = require('express');
```

**Supprimez les marqueurs** `<<<<<<<`, `=======`, `>>>>>>>`.

---

## Étape 6.6 : Finaliser la résolution

```bash
git add server.js
git commit -m "Résolution du conflit: fusion des commentaires serveur"
```

Vérifiez :

```bash
git log --oneline --graph --all
```

**Résultat attendu :** Vous voyez un graphe avec un commit de merge.

Nettoyez les branches :

```bash
git branch -d branche-a
git branch -d branche-b
```

---

# 🤝 PARTIE 7 : Collaboration avec GitHub

## Étape 7.1 : Cloner un dépôt

Simulons le travail d'un collègue. Dans un **autre dossier** :

```bash
cd ..
git clone https://github.com/VOTRE_USERNAME/tp-api-nodejs.git tp-api-collegue
cd tp-api-collegue
```

> 💡 **`git clone`** crée une copie complète d'un dépôt distant (code + historique).

---

## Étape 7.2 : Travailler comme un collègue

Depuis le dossier `tp-api-collegue`, créez une branche et faites une modification :

```bash
git checkout -b feature-health-check
```

Ajoutez un endpoint de santé dans `server.js`, après la route d'accueil `app.get('/', ...)` :

```javascript
// Health check endpoint
app.get('/health', (req, res) => {
    res.status(200).json({
        status: 'OK',
        timestamp: new Date().toISOString(),
        uptime: process.uptime()
    });
});
```

```bash
git add server.js
git commit -m "Ajout d'un endpoint health check"
git push origin feature-health-check
```

---

## Étape 7.3 : Créer une Pull Request sur GitHub

1. Allez sur GitHub dans votre dépôt
2. Vous verrez un bandeau jaune : **"feature-health-check had recent pushes"**
3. Cliquez sur **"Compare & pull request"**
4. Remplissez :
   - **Title :** `Ajout d'un endpoint health check`
   - **Description :**
     ```
     ## Description
     Ajout d'un endpoint /health pour vérifier que le serveur fonctionne.

     ## Test
     GET http://localhost:3000/health

     ## Résultat attendu
     { "status": "OK", "timestamp": "...", "uptime": ... }
     ```
5. Cliquez sur **"Create pull request"**

> 💡 **Pull Request (PR)** : Demande de fusion d'une branche dans une autre. C'est le mécanisme principal de collaboration sur GitHub. Cela permet :
> - La revue de code par les pairs
> - La discussion sur les modifications
> - Les tests automatiques avant fusion

---

## Étape 7.4 : Revoir et fusionner la Pull Request

1. Sur GitHub, allez dans l'onglet **"Files changed"** de la PR
2. Examinez les modifications
3. Si tout est correct, cliquez sur **"Merge pull request"**
4. Puis **"Confirm merge"**

---

## Étape 7.5 : Synchroniser le dépôt original

Retournez dans votre dossier original :

```bash
cd ../tp-api-nodejs
git pull origin main
```

Vérifiez que le endpoint `/health` est maintenant dans votre `server.js`.

---

## Étape 7.6 : Forker un dépôt (contribution open source)

1. Allez sur le dépôt GitHub d'un camarade
2. Cliquez sur **"Fork"** (en haut à droite)
3. Cela crée une **copie** du dépôt sur votre compte

```bash
git clone https://github.com/VOTRE_USERNAME/depot-du-camarade.git
cd depot-du-camarade
git checkout -b ma-contribution
# ... faire des modifications ...
git add .
git commit -m "Ma contribution"
git push origin ma-contribution
```

4. Sur GitHub, créez une **Pull Request** vers le dépôt original

> 💡 **Fork** = Copie d'un dépôt sur votre compte. C'est la base de la contribution open source.

---

# 🛠️ PARTIE 8 : Commandes Avancées

## Étape 8.1 : Stash – Mettre de côté des modifications

Vous travaillez sur quelque chose mais devez changer de branche urgement :

```bash
# Modifiez un fichier quelconque (ex: ajoutez un commentaire dans server.js)
echo "// TODO: ajouter la documentation" >> server.js

git status
# Le fichier est modifié mais pas commité

# Mettre de côté les modifications
git stash
git status
# Le working directory est propre !

# Faire autre chose (changer de branche, etc.)
git checkout main

# Revenir et récupérer les modifications
git checkout main
git stash pop
git status
# Les modifications sont de retour !
```

> 💡 **`git stash`** est comme un tiroir temporaire. Utile quand vous devez changer de contexte rapidement.

Commandes stash utiles :

```bash
git stash list          # Voir tous les stash
git stash pop           # Récupérer et supprimer le dernier stash
git stash apply         # Récupérer sans supprimer
git stash drop          # Supprimer un stash
```

⚠️ N'oubliez pas d'annuler la modification de test avant de continuer :

```bash
git checkout -- server.js
```

---

## Étape 8.2 : Revenir en arrière avec `git revert`

Faisons un commit que nous allons annuler :

```bash
echo "// Ce commentaire est une erreur" >> server.js
git add server.js
git commit -m "Ajout d'un mauvais commentaire (erreur)"
git log --oneline
```

Annulons ce commit **sans perdre l'historique** :

```bash
git revert HEAD
```

Un éditeur s'ouvre pour le message de commit. Sauvegardez et fermez.

```bash
git log --oneline
```

**Résultat attendu :**
```
abc1234 Revert "Ajout d'un mauvais commentaire (erreur)"
def5678 Ajout d'un mauvais commentaire (erreur)
...
```

> 💡 **`git revert`** crée un nouveau commit qui **annule** les modifications d'un commit précédent. L'historique est préservé. C'est la méthode **sûre** pour annuler.

---

## Étape 8.3 : Voir les différences avec `git diff`

```bash
# Modifiez un fichier
echo "// Modification temporaire" >> server.js

# Voir les différences non stagées
git diff

# Voir les différences stagées
git add server.js
git diff --staged

# Voir les différences entre deux commits
git log --oneline
git diff <hash-commit-1> <hash-commit-2>
```

Annulez la modification :

```bash
git checkout -- server.js
```

---

## Étape 8.4 : Tagging – Marquer des versions

```bash
# Créer un tag pour la version 1.0
git tag -a v1.0 -m "Version 1.0 : API CRUD complète"

# Voir les tags
git tag

# Voir les détails d'un tag
git show v1.0

# Pousser les tags sur GitHub
git push origin --tags
```

> 💡 Les **tags** marquent des points importants dans l'historique (versions, releases). Sur GitHub, les tags apparaissent dans la section "Releases".

---

## Étape 8.5 : Voir qui a modifié quoi avec `git blame`

```bash
git blame server.js
```

**Résultat :** Chaque ligne montre qui l'a écrite, quand, et dans quel commit.

> 💡 **`git blame`** est utile pour comprendre l'historique d'un fichier et savoir à qui poser des questions.

---

## Étape 8.6 : Cherry-pick – Prendre un commit spécifique

```bash
# Créer une branche avec un commit utile
git checkout -b feature-temporaire
echo "// Fonction utilitaire" >> server.js
git add server.js
git commit -m "Ajout d'une fonction utilitaire"
git log --oneline
# Notez le hash du commit, par ex: aaa1111

# Retourner sur main
git checkout main

# Prendre UNIQUEMENT ce commit
git cherry-pick aaa1111
```

> 💡 **`git cherry-pick`** copie un commit spécifique d'une branche vers une autre, sans fusionner toute la branche.

Nettoyez :

```bash
git branch -D feature-temporaire
```

---

# 🎯 PARTIE 9 : Défis Git

## 🏆 Défi 1 : Le workflow complet (⭐⭐)

**Objectif :** Pratiquer le workflow branche → commit → push → PR → merge.

### Consigne

1. Créer une branche `feature-tri`
2. Ajouter dans le contrôleur une fonction `getEtudiantsSorted` qui retourne les étudiants triés par moyenne décroissante
3. Ajouter la route correspondante
4. Commiter, pusher, créer une PR, fusionner

### Indices

<details>
<summary>💡 Indice Mongoose</summary>

```javascript
const etudiants = await Etudiant.find().sort({ moyenne: -1 });
```
</details>

<details>
<summary>💡 Indice commandes Git</summary>

```bash
git checkout -b feature-tri
# ... modifications ...
git add .
git commit -m "Ajout du tri par moyenne"
git push origin feature-tri
# Puis créer la PR sur GitHub
```
</details>

---

## 🏆 Défi 2 : Résoudre un conflit réel (⭐⭐⭐)

**Objectif :** Gérer un conflit entre deux développeurs.

### Consigne

1. Créer deux branches : `dev-alice` et `dev-bob`
2. Sur `dev-alice` : Modifier le message de la route d'accueil `GET /` pour afficher `"API Gestion Étudiants v2.0 - par Alice"`
3. Sur `dev-bob` : Modifier le même message pour afficher `"API Scolaire v2.0 - par Bob"`
4. Fusionner les deux branches dans `main` → résoudre le conflit

### Indices

<details>
<summary>💡 Rappel de la résolution de conflit</summary>

1. Ouvrir le fichier en conflit
2. Chercher les marqueurs `<<<<<<<`, `=======`, `>>>>>>>`
3. Choisir la version à garder (ou combiner)
4. Supprimer les marqueurs
5. `git add` + `git commit`
</details>



---

## 🏆 Défi 3 : Voyage dans le temps (⭐⭐)

**Objectif :** Explorer l'historique et naviguer entre les versions.

### Consigne

1. Utilisez `git log --oneline` pour voir l'historique
2. Utilisez `git checkout <hash>` pour revenir à votre tout premier commit
3. Observez l'état des fichiers (lesquels existent ?)
4. Revenez au présent
5. Créez un tag `v2.0` sur le commit actuel

### Indices

<details>
<summary>💡 Naviguer dans l'historique</summary>

```bash
git log --oneline
# Notez le hash du premier commit

git checkout <hash-premier-commit>
# Vous êtes en "detached HEAD"
# Observez les fichiers

git checkout main
# Retour au présent
```
</details>



---

## 🏆 Défi 4 : Le .gitignore oublié (⭐⭐⭐)

**Objectif :** Comprendre comment retirer un fichier déjà suivi par Git.

### Consigne

Imaginez que vous avez accidentellement commitié le fichier `.env` au tout début.

1. Créez un fichier `secret.txt` avec le contenu `MOT_DE_PASSE=admin123`
2. Ajoutez-le et commitez-le (simulant l'erreur)
3. Ajoutez `secret.txt` au `.gitignore`
4. Constatez que Git le suit toujours malgré le `.gitignore`
5. Résolvez le problème pour que Git arrête de suivre le fichier

### Indices

<details>
<summary>💡 Le problème du .gitignore</summary>

`.gitignore` ignore seulement les fichiers **non suivis**. Si un fichier est déjà dans l'historique Git, le `.gitignore` ne l'arrêtera pas.
</details>

<details>
<summary>💡 La commande magique</summary>

```bash
git rm --cached <fichier>
```
`--cached` supprime le fichier du suivi Git **sans le supprimer du disque**.
</details>



---

# 📊 PARTIE 10 : Tableau récapitulatif

## Commandes Git essentielles

| Commande | Description |
| --- | --- |
| `git init` | Initialiser un dépôt |
| `git status` | Voir l'état des fichiers |
| `git add <fichier>` | Ajouter à la staging area |
| `git commit -m "msg"` | Créer un commit |
| `git log --oneline` | Voir l'historique |
| `git branch` | Lister les branches |
| `git checkout -b <nom>` | Créer et basculer sur une branche |
| `git merge <branche>` | Fusionner une branche |
| `git remote add origin <url>` | Connecter un dépôt distant |
| `git push origin <branche>` | Pousser vers GitHub |
| `git pull origin <branche>` | Récupérer depuis GitHub |
| `git clone <url>` | Cloner un dépôt |
| `git stash` | Mettre de côté des modifications |
| `git revert <hash>` | Annuler un commit |
| `git diff` | Voir les différences |
| `git tag -a v1.0 -m "msg"` | Créer un tag |
| `git blame <fichier>` | Voir l'auteur de chaque ligne |
| `git cherry-pick <hash>` | Copier un commit spécifique |
| `git rm --cached <fichier>` | Arrêter de suivre un fichier |

## Workflow quotidien recommandé

```
1. git pull                    # Récupérer les dernières modifications
2. git checkout -b feature-x   # Créer une branche
3. ... coder ...               # Travailler
4. git add .                   # Préparer
5. git commit -m "message"     # Sauvegarder
6. git push origin feature-x   # Pousser
7. Créer une Pull Request      # Demander la revue
8. Merge sur GitHub             # Fusionner
9. git checkout main            # Revenir sur main
10. git pull                   # Synchroniser
11. git branch -d feature-x    # Nettoyer
```

---

# 🔧 Dépannage

| Problème | Solution |
| --- | --- |
| `fatal: not a git repository` | Exécutez `git init` ou vérifiez que vous êtes dans le bon dossier |
| `Permission denied (publickey)` | Configurez une clé SSH ou utilisez HTTPS |
| `rejected – non-fast-forward` | Faites `git pull` avant `git push` |
| `CONFLICT` lors d'un merge | Ouvrez le fichier, résolvez, `git add` + `git commit` |
| Commit sur la mauvaise branche | `git stash`, `git checkout bonne-branche`, `git stash pop` |
| Fichier commitié par erreur | `git rm --cached <fichier>` + `.gitignore` |

---

# 🎉 Félicitations !

Vous maîtrisez maintenant :
- ✅ Les fondamentaux de Git (init, add, commit, status, log)
- ✅ Les branches et la fusion (branch, checkout, merge)
- ✅ La collaboration via GitHub (push, pull, clone, fork, PR)
- ✅ La résolution de conflits
- ✅ Les commandes avancées (stash, revert, tag, blame, cherry-pick)
- ✅ Les bonnes pratiques (.gitignore, messages de commit, workflow)
