# Laravel boilerplate



This is a backend stack of decoupled technologies in docker containers.
If you have installed `docker-compose` only use

```bash
docker compose up --build --force-recreate -d
cp webapp/.env.example webapp/.env
docker compose exec php-fpm sh -i
php artisan key:generate
docker compose restart
```

It should run at [localhost](http://localhost:8080)

docker php nginx redis postgres

## Thanks :)
