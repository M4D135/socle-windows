# Socle Windows

Dépendances lourdes communes à **Pesto 2**, **FILPRO 2**, **Domenia 2** et **GLPI Pilot**, publiées une seule fois ici.
Chaque projet ne publie plus qu'un pack **LIGHT** : ses exécutables et son dossier `wad/`, sans ces dépendances.

## Pack WebView2 à version fixe

Release `webview2-<version>` : `Socle_WebView2-<version>_Windows-x64.zip` et `SHA256SUMS.txt`.

À utiliser seulement sur un poste où **Microsoft Edge WebView2 Runtime Evergreen** n'est pas installé :

1. Comparer l'empreinte du ZIP à `SHA256SUMS.txt`.
2. Extraire le ZIP **dans le dossier `wad\` de l'application**. Il crée `wad\runtime\webview2\` ; ne rien renommer.
3. Relancer l'application.

Version actuelle : **156.0.4314.42** (x64, 257 fichiers). Provenance, empreintes et vérification des signatures :
[`webview2/156.0.4314.42/webview2-fixed.json`](webview2/156.0.4314.42/webview2-fixed.json) et l'index fichier par fichier
`webview2-files.sha256`.

## Convention commune aux applications

Ordre de recherche du moteur, identique pour les quatre outils :

1. WebView2 Evergreen installé sur le poste (HKLM, version compatible avec le SDK de l'application) ;
2. sinon `wad\runtime\webview2\msedgewebview2.exe` à côté de l'exécutable (pack de ce dépôt) ;
3. sinon un message clair : installer Evergreen, ou extraire le pack WebView2 du Socle dans `wad\`, avec le lien de la release.

Le pack LIGHT ne contient jamais `wad\runtime\webview2`. Deux règles en découlent :

- **Mise à jour** : une application qui remplace son dossier d'installation doit préserver `wad\runtime\webview2`
  (le recopier vers la nouvelle version), sinon le poste perd le moteur à chaque mise à jour.
- **Contrôle d'intégrité** : une application qui refuse les fichiers absents de son manifeste doit accepter ce dossier
  quand il correspond exactement à l'index du Socle (`webview2-<version>.files.json` : chemin, taille et SHA-256 des
  257 fichiers, publié aussi comme asset de la release, sans télécharger le ZIP). Le même ZIP sert à toutes les applications : un projet ne doit pas
publier sa propre copie du runtime. Pour changer de version, publier une nouvelle release ici, puis l'indiquer aux projets.
