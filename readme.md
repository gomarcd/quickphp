## quickphp

This docker stack contains PHP, Composer, NodeJS, Nginx & MariaDB. This is great for a quickly reproducible Laravel environment without needing to manage these components individually on the host machine.

Most systems with [Docker & Docker Compose](https://docs.docker.com/engine/install/ubuntu/) already installed should be able to just follow these instructions.

Just git clone repo to get started.

## Configure

Make any desired changes to `Dockerfile`, `docker-compose.yml` or `nginx/conf.d/app.conf`.

Note in `nginx/conf.d/app.conf`:

`fastcgi_pass app:9000;` "app" **must** correspond to service name in `docker-compose.yml`:

```
  app:
    build:
     context: .
     dockerfile: Dockerfile
```

## PHP dependencies

To include additional PHP dependencies, edit the `Dockerfile` and add to the `&& docker-php-ext-install pdo_mysql` line.

## Build

We will only start app container for now:

`docker compose up -d app --build`

Create new Laravel project:

`docker compose exec app composer create-project laravel/laravel ./`

## Permissions:

Set permissions:

```
sudo chown -R www-data:www-data app
sudo chmod -R 775 app/storage
sudo nano app/storage/logs/laravel.log
sudo chown www-data:www-data app/storage/logs/laravel.log
```

## Database

If using database container, create `.env` based on the `.env.example` file and fill out credentials, which need to match in `app/.env`.

If you don't want to use MariaDB, you can remove it from `docker-compose.yml` and use SQLite instead by adding the following to `app/.env`:

```
DB_CONNECTION=sqlite
DB_DATABASE=/var/www/database/db.sqlite
```

Then do:

`sudo touch app/database/db.sqlite && sudo chown www-data:www-data app/database/db.sqlite`.

Run database migration:

`docker compose exec app php artisan migrate`

## Launch project

Start other containers:

`docker compose up -d`

Your new Laravel app is now available at `http://localhost:8080`.

## Optional starter kits

Your project is ready to be developed.

You can optionally install [starter kits](https://laravel.com/docs/10.x/starter-kits)

As an example, you can install Breeze with:

```
docker compose exec app composer require laravel/breeze --dev
docker compose exec app php artisan breeze:install
docker compose exec app php artisan migrate
docker compose exec app npm install
docker compose exec app npm run build
```
