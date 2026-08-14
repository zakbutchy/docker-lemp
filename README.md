# LEMP by Docker

## Overview

* [php:8.4-fpm-bookworm][php]
* [composer:2][composer]
* [node:22-bookworm-slim][node]
* [nginx:1.30.4-alpine][nginx]
* [mysql:8.4][mysql]
* [redis:8.10.0-alpine][redis]
* [phpmyadmin/phpmyadmin][phpmyadmin] (disabled by default, uncomment it in docker-compose.yml to use)
* [mailpit][mailpit]

[php]:https://hub.docker.com/_/php
[composer]:https://hub.docker.com/_/composer
[node]:https://hub.docker.com/_/node
[nginx]:https://hub.docker.com/_/nginx
[mysql]:https://hub.docker.com/_/mysql
[redis]:https://hub.docker.com/_/redis
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

Use `make build-fresh` if you want to rebuild everything from scratch, ignoring the cache.

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
docker compose exec app composer create-project --prefer-dist  "laravel/laravel=11.*" .

docker compose exec app php artisan key:generate
docker compose exec app php artisan storage:link
docker compose exec app chmod -R 777 storage bootstrap/cache
make ps
docker compose exec app php artisan -V
```

2.Install Packages

```bash
make install-recommend-packages
```

3.Vite hot reload
`src/vite.config.js`

```json
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

3.Access

Run `docker compose exec app npm run dev` to start the Vite dev server (it is not started automatically).

```bash
# Go to Laravel welcome page
open http://localhost:8080

# mailpit
open http://localhost:8025/

# Vite
open http://localhost:5173/
```

PhpMyAdmin is disabled by default (uncomment it in `docker-compose.yml` to use it at `http://localhost:8888/`). This setup assumes connecting to `localhost:3306` directly from a DB client such as DBeaver instead.

Use `make redis` to connect via redis-cli.

## Setup

* Edit `.env` , `config/app.php` and more...
* Since Laravel 11, the default `.env` uses `DB_CONNECTION=sqlite`. To use the MySQL container in this environment, set `DB_CONNECTION=mysql` and `DB_HOST=mysql`, and match `DB_DATABASE`/`DB_USERNAME`/`DB_PASSWORD` with the values in the root `.env`.
* Delete test_db and create a database for the new project.

## Xdebug

Append `pathMappings` to configurations in launch.json

```json
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

* [【超入門】20分でLaravel開発環境を爆速構築するDockerハンズオン - Qiita](https://qiita.com/ucan-lab/items/56c9dc3cf2e6762672f4)
* [Laravel 9 + VITEの開発環境をdockerで実現する方法 - Qiita](https://qiita.com/hitotch/items/aa319c49d625c2a9b65e)
* [Dockerを使ってLaravelのローカル開発環境を作る(Apache版) - Qiita](https://qiita.com/ucan-lab/items/38cd04cee1f3f9e024b9s)
* [Docker環境のLaravel 9 + VITEでハマったこと - Qiita](https://qiita.com/hellomyzn/items/b7bf5c209437ed70af74)
