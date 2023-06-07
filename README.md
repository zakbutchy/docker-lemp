# LEMP by Docker

## Overview

* [php:8.1-fpm-buster][php]
* [composer:2.5][composer]
* [node:20-buster][node]
* [nginx:1.20-alpine][nginx]
* [mysql/mysql-server:8.0][mysql]
* [phpmyadmin/phpmyadmin][phpmyadmin]
* [mailpit][mailpit]

[php]:https://hub.docker.com/_/php
[composer]:https://hub.docker.com/_/composer
[node]:https://hub.docker.com/_/node
[nginx]:https://hub.docker.com/_/nginx
[mysql]:https://hub.docker.com/r/mysql/mysql-server
[phpmyadmin]:https://hub.docker.com/_/phpmyadmin
[mailpit]:https://hub.docker.com/r/axllent/mailpit


## Build
```sh
cd project_path
# remove .git
rm -rf .git

make build
make up
```

index.html
```bash
open http://localhost:8080/index.html
```


## How to create Laravel project

1.Install Laravel(latest)
```sh
make create-laravel-project
```

If you want to change the version.
```sh
# Specify the version to be installed
docker compose exec app composer create-project --prefer-dist  "laravel/laravel=10.*" .

docker compose exec app php artisan key:generate
docker compose exec app php artisan storage:link
docker compose exec app chmod -R 777 storage bootstrap/cache
make ps
docker compose exec app php artisan -V
```

phpunit.xml
```
    <php>
        <env name="APP_ENV" value="testing" force="true"/>
        <env name="DB_CONNECTION" value="mysql" force="true"/>
        <env name="DB_HOST" value="db-testing" force="true"/>
        <env name="DB_PORT" value="3306" force="true"/>
        <env name="DB_DATABASE" value="laravel_local" force="true"/>
        <env name="DB_USERNAME" value="phper" force="true"/>
        <env name="DB_PASSWORD" value="secret" force="true"/>
        <env name="BCRYPT_ROUNDS" value="4"/>
        <env name="CACHE_DRIVER" value="array"/>
        <env name="MAIL_DRIVER" value="array"/>
        <env name="QUEUE_CONNECTION" value="sync"/>
        <env name="SESSION_DRIVER" value="array"/>
    </php>
```

2.Install Packages
```bash
make install-recommend-packages
```

3.Vite hot reload
`src/vite.config.js`
```
  server: {
    host: true,
    hmr: {
      host: 'localhost'
    },
    watch: {
      usePolling: true
    }
  }
```

4.Access

```bash
# Go to Laravel welcome page
open http://localhost:8080

# PhpMyAdmin
open http://localhost:8888/

# mailpit
open http://localhost:8025/

# Vite
open http://localhost:5173/
```

## Setup
- Edit `.env` , `config/app.php` and more...
- Delete test_db and create a database for the new project.


## Xdebug

Append `pathMappings` to configurations in launch.json

```
{
    "name": "Listen for Xdebug",
    "type": "php",
    "request": "launch",
    "port": 9003,
    "pathMappings": {
        "var/www/html/": "${workspaceRoot}/src"
    }
},
```


## References

- [ucan-lab/docker-laravel: 🐳 Build a simple laravel development environment with docker-compose.](https://github.com/ucan-lab/docker-laravel)
- [hellomyzn/docker-laravel](https://github.com/hellomyzn/docker-laravel)

