# Arena Clash

Jeu de bagarre en arène 3 contre 3, jouable directement dans le navigateur (mobile ou ordinateur), avec 3 modes de jeu et 6 personnages.

## Contenu du dépôt

| Fichier | Rôle |
|---|---|
| `index.html` | Le jeu complet (HTML + CSS + JS, sans dépendance externe) |
| `manifest.json` | Manifeste PWA (nom, icônes, couleurs, mode d'affichage) |
| `sw.js` | Service worker : met le jeu en cache pour un fonctionnement hors ligne |
| `icon-192.png`, `icon-512.png` | Icônes de l'application |
| `.nojekyll` | Empêche GitHub Pages de traiter le site avec Jekyll |
| `.github/workflows/deploy.yml` | Déploiement automatique sur GitHub Pages à chaque push sur `main` |

Aucune étape de build n'est nécessaire : ce sont des fichiers statiques.

## Déployer sur GitHub Pages

1. Crée un dépôt GitHub (public) et dépose-y **tous ces fichiers en gardant la même arborescence**, en particulier le dossier `.github/workflows/`.
2. Dans le dépôt : **Settings → Pages → Build and deployment → Source**, choisis **GitHub Actions**.
3. Pousse sur la branche `main` (ou lance le workflow manuellement depuis l'onglet **Actions**).
4. Après quelques dizaines de secondes, le site est disponible à l'adresse indiquée dans l'onglet **Actions** (généralement `https://<ton-pseudo>.github.io/<nom-du-repo>/`).

### Alternative sans GitHub Actions

Si tu préfères la méthode classique : **Settings → Pages → Source → Deploy from a branch**, sélectionne `main` et le dossier `/ (root)`. Dans ce cas, le dossier `.github/workflows/` n'est pas nécessaire.

## Fonctionnement hors ligne / installation

Une fois le site hébergé en HTTPS (ce que fait GitHub Pages automatiquement), le service worker s'enregistre et permet :
- de jouer hors connexion après une première visite,
- d'installer le jeu comme une application (« Ajouter à l'écran d'accueil » sur mobile, icône d'installation dans la barre d'adresse sur ordinateur).

Cela ne fonctionne pas en ouvrant `index.html` directement depuis le disque (`file://`) — c'est une restriction des navigateurs, pas un bug. Le jeu reste jouable normalement dans ce cas, simplement sans mode hors ligne ni installation.

## Modes de jeu

- **Chasse aux gemmes** — première équipe à 10 gemmes.
- **Élimination** — pas de résurrection, il faut éliminer les 3 adversaires.
- **Ballon Brawl** — pousser le ballon dans le but adverse, première équipe à 2 buts.

## Contrôles

- Joystick gauche (fixe) : déplacement analogique.
- Joystick droit (fixe) : viser et tirer (une ligne de visée s'affiche pendant le tir).
- Clavier (test sur ordinateur) : ZQSD/flèches pour se déplacer.
- Le jeu se joue à l'horizontale ; un écran invite à tourner l'appareil si besoin.
