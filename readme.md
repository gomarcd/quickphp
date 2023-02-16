## quickphp

This docker stack contains PHP, Composer, NodeJS, Nginx & MariaDB.

## Laravel

To use this with Laravel, first create your new Laravel project:

`docker run --rm --volume ./app:/app composer/composer create-project laravel/laravel ./`

## Set permissions

Set permissions:

```
sudo chown -R www-data:www-data app
sudo chmod -R 775 app/storage
```

## Configure

Make desired changes to `docker-compose.yml` and `nginx/conf.d/app.conf`. Note that in the app.conf's `fastcgi_pass app:9000;` **app** must correspond to the service name in `docker-compose.yml`.

Create a `.env` based on the `.env.example` file, and fill out database credentials which need to match in your Laravel's `app/.env`.

## PHP dependencies

To add PHP dependencies, edit `Dockerfile` and add to the `&& docker-php-ext-install pdo_mysql` line and then rebuild image.

## Build and start

`docker compose up -d --build`

Generate a key and run database migration:

```
docker compose exec app php artisan key:generate
docker compose exec app php artisan migrate
```

Your new Laravel app is now available at `http://localhost:8080`.