il manquerait quoi à notre site, pour en faire un vrai site ? => le CSS

on peut ajouter `<link rel="stylesheet" href="/assets/css/style.css">`

mais ça ne marche pas il faut que le serveur puisse renvoyer ce fichier CSS

=> prévoir une route qui renvoie le contenu du fichier quand n demande GET sur /assets/css/style.css

```js
app.get('/assets/css/style.css', (req, res) => {
  res.sendFile(__dirname + '/assets/css/style.css');
})
```

Note : l'URL pourrait être différente de l'emplacement du fichier : => `get('/css/style.css'`

démo dans le navigateur, avec l'URL du fichier CSS, puis la page d'accueil du site

---

Donc une route pour chaque fichier statique ? Pour les images aussi ?

TODO prévoir une image pour montrer la route pour l'image ?

Note : on peut montrer l'utilisation du css aussi sur la page 404

Et tant qu'à faire, mutualiser des choses entre les pages...