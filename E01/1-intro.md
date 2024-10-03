aujourd'hui :
- correction challenge : on reparlera de npm, express, routes statiques et dynamiques. Bonus programmation fonctionnelle avec find
- nouveautés
TODO petit texte de résumé de la suite ?

Pas de challenge demain, atelier demain (semaine passée et aujourd'hui)


(juste pour moi, ne pas le dire)
- service des assets => moyen optimal de servri les assets ? = fichier statique, fichier qui n'évolue pas. Par exemple fichiers styles CSS, images, JS pour le navigateur
- vues (partie présentation = fichiers HTML générés par le serveur web pour être envoyés au client), on peut faire mieux que de construire le HTML avec des chaines de caractères :
  - moteur de template : logiciel qui s'appuie sur un modèle de page HTML et des données, et qui mélange les deux pour construire la page à fournir au client. Celui qu'on va voir c'est EJS
  - configuration d'EJS pour Express
  - res.render
  - données et template :
    - données avec res.render
    - données avec locals : app.locals, res.locals, objet.locals dans EJS
    - tags EJS : <%=%>, <%-%>, <%%>
    - include
    - logique dans les templates