# Récap installation de WP

## Pourquoi une install custom de Wordpress?

- On veut gérer le code tiers avec un gestionnaire de dépendances (composer pour PHP, npm pour JS) => donc on veut garder ce fonctionnement pour les plugins tiers installés sur notre projet (idem pour les thèmes, mais aussi pour le code de WordPress!)
- Facilitation du versionning => on ne veut versionner que le code spécifique au projet (notre propre code)
- Potentielle automatisation des tâches

## On considère notre projet comme une projet composer

- On peut faire un `composer init` pour démarrer le projet de 0
- :warning: Il faut penser au gitignore pour éviter du versionner les dépendances
- On déplace ou on copie index.php depuis le répértoire qui contient les sources de WP vers la racine du projet => on accède à WP directement en accedant au projet
- Ca oblige WP à chercher le fichier wp-config à la racine de notre projet

- Lorsque WP dispose d'une connexion à une BDD vide => il nous invite à lancer l'install => créer les tables de WP. On peut soit 
      - remplir le formulaire proposé par WordPress lorsqu'on l'affiche
      - utiliser WP-CLI pour générer ces tables (équivalent au formulaire)
          - On doit fournir des valeurs pour lancer l'install
```
  wp core install --url=[url du projet] --title=[nom du site] --admin_user=[username désiré pour l'admin] --admin_email=[email de l'admin] --admin_password=[password de l'admin]

# ou alors

wp core install --prompt

# la commande pose les questions une par une

# On attend en sortie:
Success: WordPress installed successfully.
```

On ajoute à notre composer.json une nouvelle plateforme pour récupérer des dépendances: wppackagist

dans composer.json:
```
"repositories":[
    {
        "type":"composer",
        "url":"https://wpackagist.org",
        "only": [
            "wpackagist-plugin/*",
            "wpackagist-theme/*"
        ]
    }
],
```