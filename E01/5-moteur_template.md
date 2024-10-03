actuellement 2 façons différentes de gérer le html : fichiers statiques (index.html, mais qui en fait sera dynamisé par la suite), fichiers dynamiques où on génère le html dans le vcallback de la route.

Un moteur de template, c'est un outil qui permet de générer la couche "vue, ou présentation" de l'application (c'est à dire le HTML dans une application) à partir de 2 choses : un gabarit, un modèle (template en anglais) et des données. On fusionne les 2 pour avoir une page qui s'appuie sur le modèle en utilisant les données (par exemple une ville, ou une autre ville).

On va installer puis configurer le moteur de template.

https://expressjs.com/en/guide/using-template-engines.html

Ce n'est pas celui qu'on va utiliser, mais ça permet de voir le principe : 
- dire à express quel est le moteur de template à utiliser => app.set('view engine', 'xxxx') . On va utiliser EJS
- dire à express où est le dossier qui contient les templates => app.set('views', 'dossier')

https://ejs.co/

`npm install ejs`

et dans app.js (avant app.use, de préférence)

```js
// configuration de express pour qu'il utilise le moteur de template ejs
// - moteur de template à utiliser
app.set('view engine', 'ejs');
// - le dossier dans lequel se situent les templates
app.set('views', './views');
```

note : pas besoin de require, c'est express qui fera tout seul le require quand on lui dit quel est le moteur de template à utiliser


COMMIT installation et configuration du moteur de template ejs

Utilisons ça : on va utiliser une méthode render sur res => route /

```js
app.get('/', (req, res) => {
  res.render('index');
})
```

On regarde... "pas trouvé" => parce qu'il faut l'extension .ejs sur le fichier index.html

Note : pas besoin de __dirname, comme on a indiqué le dossier des templates.

Du coup notre fichier "index.ejs" a obtenu des possibiltiés supplémentaires !

Note : pug c'est quand vous aurez beaucoup l'habitude...

Il y aurait pas un autre endroit où on pourrait faire ça, utiliser un fichier de template ejs ? => sur notre route paramétrée (mais un peu plus tard car il faut des données), sur notre page 404

COMMIT Utilisation d'EJS pour nos fichiers HTML statiques : index et 404

Ca va nous apporter quoi ? Un premier avantage : 
- éviter les répétitions (header, footer sont souvent pareils), on pourra sortir dans un autre fichier les éléments communs, et les inclure au lieu de se répéter. On appelle ça des partials.
  
Nouveau dossier partials dans le dossier views, fichiers header.ejs et footer.ejs

On sort le "header" dans le fichier (y compris balise head), notre fichier index devient :

```ejs
<%- include ('./partials/header') %>
<main>
.....
</main>
<%- include ('./partials/footer') %>
```

=> des briques pour construire nos fichiers HTML

Si on veut changer quelque chose sur toutes les pages (sur un élément commun à toutes les pages), on change dans un seul fichier => année du copyright dans le footer

Note : NE PAS chercher dans la doc ejs, trop compliqué pour le moment car plein d'autres possibilités

Commentaires avec EJS : `<%# include permet de rendre un template externe dans ce template %>`

Note / TODO : pourquoi balises soulignées en jaune ?

COMMIT Utilisation de include pour le header et le footer

---

On va s'occuper du HTML qu'on générait sous forme de chaîne de caractère :

Nouvelle vue city.ejs dans le dossier views. On y déplace tout le code HTML qui était en chaîne de caractères. Pour le moment on met en dur les infos de la ville, xxxxxx, on verra après comment gérer ça. Et on utlise des includes pour le haut et le bas du fichier.

`res.render('city');` est la seule instruction du callback du get

Note : on n'aura plus de fichiers html dans notre serveur, on mettra tout en template.

Maintenant les parties dynamiques : la fonction render peut prendre un deuxième argument, qui permet de transmettre les données => un objet avec toutes les données dont le template aura besoin

```js
res.render('city', {
  cityTime: foundCapitalCityTime,
  // city est un objet qui contient nom et timezone
  city: foundCapitalCity
});
```

Dans city.ejs, on ajoute un tag à la place des xxxx :

```
<%= city.tz %>
```









1'04 sur vidéo 3 (sur 2h)

