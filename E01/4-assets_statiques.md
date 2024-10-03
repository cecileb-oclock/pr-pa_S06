# Fournir des assets statiques avec Express 

https://expressjs.com/fr/starter/static-files.html
=> lire un peu

méthode use, et express.static on lui fournit le dossier qui contient l'ensemble des ressources statiques de notre serveur

On a un dossier qui contient tout ? => assets (aussi views, mais en fait nous on va les dynamiser ces vues par la suite)

=> on enlève les 2 routes CSS et IMG (on imagine plein de routes, une par fichier)

```js
// on configure express pour servir l'ensemble des fichiers statiques contenus dans le dossier assets
// une requête en GET sur /css/style.css permettra d'obtenir en réponse le contenu de assets/css/style.css
app.use(express.static('assets'));
```

Si on a besoin d'une deuxième image, on la pose dans le dossier, et on ajoute <img src=....> dans le HTML servir. Pas besoin d'ajouter une route !

Note : on pourrait avoir plusieurs lignes sur ce modèle si on a plusieurs dossiers qui contiennent des ressources statiques

En fait, c'est comme un dossier virtuel, raccourci vers un dossier réel.

Démo possible : montrer qu'il y a plusieurs requêtes dans Network dans les dev tools

Option : mettre en place un fichier JS front, avec un console.log qu'on voit dans le navigateur et pas dans le terminal. Et on ajoute un querySelector pour bien clarifier, avec un addEventListener.

