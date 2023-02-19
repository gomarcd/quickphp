## quickphp

This docker stack contains PHP, Composer, NodeJS, Nginx & MariaDB and is great for starting a new Laravel project.

## Configure

Make desired changes to `docker-compose.yml` and `nginx/conf.d/app.conf`. Note that in the app.conf's `fastcgi_pass app:9000;` **app** must correspond to the service name in `docker-compose.yml`.

Create a `.env` based on the `.env.example` file, and fill out database credentials which need to match in your Laravel's `app/.env`.

If you don't need MariaDB, you can remove it from the `docker-compose.yml` and instead use SQLite by adding the following to `app/.env`:

```
DB_CONNECTION=sqlite
DB_DATABASE=/var/www/database/db.sqlite
```

And then doing the command `touch app/database/db.sqlite`.

## PHP dependencies

To include additional PHP dependencies, edit the `Dockerfile` and add to the `&& docker-php-ext-install pdo_mysql` line.

## Build

The nginx container includes a volume mount in the app folder, so we will only start the app container for now:

`docker compose up -d app --build`

Now create your new Laravel project:

`docker compose exec app composer create-project laravel/laravel ./`

Now that project is created, you can start the other containers:

`docker compose up -d`

Set permissions:

```
sudo chown -R www-data:www-data app
sudo chmod -R 775 app/storage
```

Generate key and run database migration:

```
docker compose exec app php artisan key:generate
docker compose exec app php artisan migrate
```

Your new Laravel app is now available at `http://localhost:8080`.

## Breeze & Blade

If you'd also like to add Breeze + Blade stack, just run:

```
docker compose exec app composer require laravel/breeze --dev
docker compose exec app php artisan breeze:install
docker compose exec app php artisan migrate
docker compose exec app npm install
docker compose exec app npm run build
```

.