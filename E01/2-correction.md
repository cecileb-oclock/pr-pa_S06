- on regarde les données, capitalCities.js => tableau/objet/propriétés (nom et timezone/fuseau horaire) => important de regarder le format des données, tabeau donc possibilité de boucle

Ce fichier met à disposition le tableau pour le monde extérieur avec module.exports

- on regarde views/index.html => page d'accueil de notre site

- app.js => c'est là qu'on va travailler
  
# 1

`npm init`
=> `npm init -y` pour répondre oui à tout
=> on obtient fichier package.json

COMMIT Initialisation du projet avec 'npm init -y'

# 2

Ajouter une nouvelle dépendance à notre projet ? `npm install` ou `npm i`

on peut aller voir dans la doc du module la commande extacte avec le nom du module (Google "npm" "dayjs")

`npm install express`


on peut aller voir dans la doc du module la commande extacte avec le nom du module (Google "npm" "dayjs" => bloc en haut à droite)

`npm install dayjs`

Rappel : package.json, node_modules, package-lock.json

Fichier .gitignore => `node_modules/` (on ne modifiera pas ces fichiers, et c'est facile de les re-télécharger si besoin)

COMMIT Installation des dépendances express et dayjs npm i express dayjs

On va vérifier si on arrive à lancer notre projet, `console.log` dans app.js, comment on lance avec node ? `node app.js`, `nodemon app.js`, `node app.js --watch` et nodedev ? TODO à vérifier et adapter à la promo

Rappel un serveur ne s'arrête jamais, il rend un service, il attend les demandes "pour toujours". Pour prendre en compte les modifications de code en direct, outil de watch pour que le serveur rende le service de la dernière façon indiquée dans le code

# express (on change un peu l'ordre => mais c'était un recap car changement de formateur)

Rappel avec l'exemple hello world de express : https://expressjs.com/en/starter/hello-world.html

Port : un peu comme un bureau qui correspond à un service : la comptabilité, le support...

app.listen se lance une seule fois. 

Récup le code et le placer dans app.js, on peut montrer que ça s'est mis à jour avec le message

# 4

on peut partir du code exemple hello world, cf plus haut

https://expressjs.com/en/5x/api.html#res.sendFile

```js
// => dans le get /
res.sendFile(__dirname + './views/index.html');
// __dirname c'est le chemin du dossier dans lequel le fichier exécuté (app.js) se trouve
// "variable magique" remplie automatiquement
// console.log(__dirname);
```

TODO verif code

COMMIT Mise en place de la page d'accueil : pour chaque requeête en GET sur / on sert le contenu d'un fichier HTML statique, views/index.html

=> route statique : on sait quand on écrit le programe quel sera le contenu, et il sera toujours pareil.

# 5

on pourrait faire une route pour chaque ville, mais on peut aussi créer une seule route qui saura gérer toutes les villes.

c'est au moment de l'exécution (quand un client demandera les infos pour une ville) qu'on saura quelle ville il faut

+ extensible (si ajout de villes dans les données), mutualisation de code
  
Il faut un paramètre dans l'URL, pour savoir quelle ville est demandée


```js
/*
Définition d'une route pour l'affichage de chacune des différentes capitales
On a besoin de savoir quelle est la ville concernée :
- on déclare un paramètre dans l'URL (on décide de l'appeler cityName)
- on récupère la valeur correspondant à ce paramètre dans la fonction de callback grâce à req.params.cityName

Exemple : quand l'URL demandée est /city/lyon , req.params.cityName vaudra lyon
*/
app.get('/city/:cityName', (req, res) => {
  res.send('<h1>Page capitale ' + req.params.cityName + '</h1>);

  // équivalent à
  /*
  const cityName = req.params.cityName;
  res.send('<h1>Page capitale ' + cityName + '</h1>);
  */
})
```

Option : GET /city/*, et <h1>, mais il faut savoir quelle ville on veut => paramètre


Note : c'est moi qui choisis le nom

En fait il faut tout le HTML HEAD etc 

=> 

```js
res.send(`<!DOCTYPE html>........${cityName} .......`);
```

On fera mieux après 😉

COMMIT Route dynamique pour afficher chaque capitale

Démo, cliquer sur les liens de la page d'accueil

# afficher la réponse que si la capitale existe

Récupérer le contenu d'un module externe, capitalCities.js

```js
const capitalCities = require('./my_modules/capitalCities');
```

"que si la capitale existe" => que si c'est présent dans le tableau

parcourir le tableau : est-ce que la première capitale c'est PAris ? oui.
est-ce que la caitale c'est Marseille? 1 non 2 non, etc

=> c'est notre algorithme

=> boucle pour parcourir le tableau. Si une capitale est ajoutée, pas besoin de modifier le code, juste les données

Algorithme :
- récupérer le nom de la ville à afficher
- parcourir le tableau des capitales (for, for of, forEach)
  - si la ville demandée est trouvée
    - alors on renvoie la réponse avec les informations de la ville trouvée (Note c'est pas vraiment les infos pour le moment, juste le nom)
  - si après parcours de l'ensemble des villes on n'a pas trouvé
    - alors on envoie un code réponse 404 Not Found
  
Attention, il faudra comparer en minucules (Paris / paris)

```js
const cityNameToFind = req.params.cityName;

for (const capital of capitalCities) {
  console.log(capitalCity);
}
```

TODO prévoir une variable pour le résultat, pour la ville trouvée (mieux que return, s'approcher de l'utilisation de find)

Note : on choisit le nom de la variable pour un élément

Qu'est-ce qui nous intéresse dans l'objet => name ? Comment comparer ?

```js
  const capitalName = capital.name;

  // en minuscules
  // https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase
  const capitalNameLowered = capitalName.toLowerCase();
  console.log(capitalNameLowered);

  if (capitalNameLowered === cityNameToFind) {
    // ici le res.send qu'on avait avant

    // rien d'autre à faire, on termine la fonction
    return;
  }
```

Gérer le cas de la ville pas trouvée ?

fichier views/404.html à créer => car affichage toujours pareil, "page non trouvée"

après la boucle :

```js
// fin de la boucle
}

// si on arrive là c'est qu'on n'a pas trouvé, sinon le return se serait appliqué

res.sendFile(__dirname + '/views/404.html');

```

COMMIT 

# faire mieux avec find

présenter find. Fléchées OK ?

```js
let foundCapitalCity = capitalCities.find((capital) => capital.name.toLowerCase() === capitalNameLowered;
// une ville, ou undefined
```


# dayjs


Récupérer le timezone de la ville trouvée : 
dans le html renvoyée, ajouter `<p>timezone ${foundCapitalCity.tz}</p>`

Calcul avec dayjs => cf doc dayjs https://day.js.org/docs/en/installation/node-js , récup l'import

`const dayjs = require('dayjs')`


l'heure de "maintenant" :

```js
// dans le html
${dayjs.format()}
```

Plugin pour utiliser le timezone : https://day.js.org/docs/en/plugin/timezone

```js
var timezone = require("dayjs/plugin/timezone"); 
// import timezone from 'dayjs/plugin/timezone'

dayjs.extend(utc);
dayjs.extend(timezone);
```

https://day.js.org/docs/en/display/format

```js
const timeToDisplay = dayjs().tx(foundCapitalCity.tz).format('HH:mm');
```

TODO vérifier qu'on met bien en place un footer avec un copyright (faire exprès de laisser 2019) et un header avec un h1, pour partie 5. Il faut ça aussi sur la page 404.