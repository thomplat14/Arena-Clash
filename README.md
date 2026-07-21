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

- **Chasse aux gemmes** (3v3) — première équipe à 10 gemmes.
- **Élimination** (3v3 ou 5v5) — pas de résurrection ; mode spectateur automatique une fois éliminé.
- **Ballon Brawl** (3v3 ou 5v5) — pousser le ballon dans le but adverse, première équipe à 2 buts.
- **Duel en ligne** (1 contre 1) — voir ci-dessous.
- **Tournoi en ligne** (1 contre 1, plusieurs manches) — voir ci-dessous.

## Duel et tournoi en ligne : comment ça marche

Ces deux modes connectent deux joueurs directement entre eux (pair-à-pair, WebRTC), sans passer par un serveur de jeu à héberger :

1. Un joueur clique sur **Créer une partie** : un code à 5 caractères s'affiche.
2. Il envoie ce code à son adversaire par n'importe quel moyen (message, appel...).
3. L'autre joueur clique sur **Rejoindre une partie** et saisit le code.
4. Une fois connectés, les deux joueurs arrivent dans un **salon** : chacun voit le personnage choisi par l'autre et clique sur **Prêt !**. La partie démarre dès que les deux l'ont fait.
5. En mode **Tournoi**, le duel se rejoue en plusieurs manches (le premier à 2 victoires remporte le tournoi) ; le score s'affiche dans le HUD et sur l'écran de fin de manche, avec un bouton **Manche suivante** jusqu'à la victoire finale.

**Détails techniques et limites à connaître :**
- La mise en relation initiale passe par le service public gratuit [PeerJS](https://peerjs.com/) (chargé depuis `cdnjs.cloudflare.com`) — nécessite donc une connexion Internet, y compris si le jeu est par ailleurs installé et utilisable hors ligne.
- Une fois connectés, les données de la partie (positions, tirs, score) circulent **directement entre les deux appareils**, sans transiter par un serveur à moi.
- Le joueur qui **crée** la partie fait autorité sur la simulation (déplacements, dégâts, collisions) ; l'autre reçoit l'état du jeu en temps réel. En cas de connexion instable, c'est donc surtout l'adversaire du créateur qui peut ressentir un léger décalage.
- Si la connexion est coupée (dans le salon ou en cours de partie), un message de déconnexion s'affiche des deux côtés.
- Ces modes sont prévus pour du 1 contre 1 uniquement. Un vrai 3v3/5v5 en ligne demanderait un serveur de jeu hébergé séparément (hors du cadre "fichiers statiques sur GitHub Pages").

## Contrôles

- Joystick gauche (fixe) : déplacement analogique.
- Joystick droit (fixe) : viser et tirer (une ligne de visée s'affiche pendant le tir).
- Clavier (test sur ordinateur) : ZQSD/flèches pour se déplacer.
- Le jeu se joue à l'horizontale ; un écran invite à tourner l'appareil si besoin.
