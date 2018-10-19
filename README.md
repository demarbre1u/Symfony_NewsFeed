# Projet Entretien Uppler #

## Description ##

Ce projet a été réalisé à l'issu de l'entretien d'embauche de l'entreprise Uppler.

Réalisé par Allan DEMARBRE.

## Comment installer le projet ##

Pour l'installer, clonez dans un premier temps le projet : 

``
git clone git@github.com:demarbre1u/Projet_Entretien_Uppler.git
``

Une fois cloné, il est nécessaire d'installer les dépendances requises : 

``
composer install
``

Le projet est maintenant prêt à être utilisé !

### Serveur de développement ###

Pour que les modifs soient visibles en accédant à l'application servit par Nginx, il faut clear le cache :

``php bin/conole cache:clear --env=prod``

Pour créer la base de données :

``php bin/console doctrine:database:create``

Pour créer les tables :

``php bin/console doctrine:schema:update --force``

Pour supprimer les tables : 

``php bin/console doctrine:schema:drop --force``

Une fois le projet installé, vous pouvez lancer un serveur de développement grâce à la commande :

``php bin/console server:start``

### Serveur de production ###

La configuration Nginx du site :

```nginx
server {
    server_name entretien.local;
    root /var/www/html/entretien/web;
    
    location / {
        try_files $uri /app.php$is_args$args;
    }
    
    location ~ ^/(app_dev|config)\.php(/|$) {
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_param DOCUMENT_ROOT $realpath_root;
    }
    
    location ~ ^/app\.php(/|$) {
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_param DOCUMENT_ROOT $realpath_root;
        internal;
    }
    
    location ~ \.php$ {
        return 404;
    }
    
    error_log /var/log/nginx/project_error.log;
    access_log /var/log/nginx/project_access.log;
}
```

Il faut ensuite ajouter le hostname dans ``/etc/hosts`` : 

``127.0.0.1   entretien.local``
