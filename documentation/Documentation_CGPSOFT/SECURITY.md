# Securité sur Monecgp

Une partie de la sécurité appliquée dépend du context de déploiement.
A l'initialisation du projet faudra créér ou éditer certains des fichiers listés ici.

## I) Le htaccess

```shell
cd ../commun-cgpoweb/src
nano .htaccess
```

### Ce qu'il doit y avoir dedans :

#### - 1 "..."

```shell
    Options -Indexes

    php_value max_execution_time 3600
    php_value memory_limit 128M
    php_value max_input_time 3600
    php_value post_max_size 16M
    php_value upload_max_filesize 10M

    ErrorDocument 404 https://www.monecgp.fr/
```

#### - 2 Protection des fichiers de configuration

Empecher l'accès via un navigateur au fichiers commencant par "."

```shell
    # Deny access to filenames starting with dot(.)
<FilesMatch "^\.">
  Order allow,deny
  Deny from all
</FilesMatch>
```

#### - 3 Contre la Verbosité (Xdebug)

```shell
# PHP error handling pour cacher les erreurs verbeuse
php_flag display_startup_errors off
php_flag display_errors off
php_flag html_errors off
php_flag log_errors on
php_flag ignore_repeated_errors off
php_flag ignore_repeated_source off
php_flag report_memleaks on
php_flag track_errors on
php_value docref_root 0
php_value docref_ext 0
php_value error_reporting -1
php_value log_errors_max_len 0
```

#### - 4 Protection de la session PHP / cookies

```shell
# surcharge du php.ini : tialle de id session, headers httpOnly et Secure du cookie
php_value session.sid_length 128

# << Previent l'observation du cookie par le javascript >>
php_value session.cookie_httponly 1

# <<Force la sécurisation en https:// empeche complement l'utilisation de cookies hors https>>
# => soit c'est sécurisé soit rien n'est accepté
# php_value session.cookie_secure 1

# << Ceci previent la capacité de copie d'un cookie >>
php_value session.use_strict_mode 1
```

## II) PHP

```php
<?php
header("X-XSS-Protection: 1");
header('X-Content-Type-Options: nosniff');
header('X-Frame-Options: SAMEORIGIN');
header('X-Powered-By: -');
```

### X-Content-Type-Options : 1

> L'entête est un marqueur utilisé par le serveur pour indiquer que les types MIME annoncés dans les en-têtes Content-Type ne doivent pas être modifiées ou et suivies.
>
> -- <cite>https://developer.mozilla.org/fr/docs/Web/HTTP/Headers/X-Content-Type-Options</cite>

### X-XSS-Protection : nosniff

Ne sert que sur les "legacy" navigateur :

> feature of Internet Explorer, Chrome and Safari that stops pages from loading when they detect reflected cross-site scripting (XSS)
>
> -- <cite>https://developer.mozilla.org/fr/docs/Web/HTTP/Headers/X-XSS-Protection</cite>

### X-Frame-Options: SAMEORIGIN

> La page ne peut être affiché que par un site de même origine. La spécification ne définit pas si les navigateurs doivent appliquer la règle à la racine (top level), au parent ou sur toute la chaine.
>
> -- <cite>https://developer.mozilla.org/fr/docs/Web/HTTP/Headers/X-Frame-Options</cite>

### X-Powered-By :

On peut y mettre ce que l'on veut pour mésinformer une personne se basant sur ceci pour de la malveillance

> "X-Powered-By" is a common non-standard HTTP response header
>
> -- <cite>https://stackoverflow.com/questions/33580671/what-does-x-powered-by-mean</cite>
