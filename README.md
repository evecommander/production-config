# Eve Commander Production
This repo contains the required files to run Eve Commander in a production setting. At the moment, this includes a Docker Compose setup but may also include a Kubernetes setup in the future...

## Docker Compose
Before running the Docker Compose file, make sure that the following environment variables are set:
 - PGSQL_DATABASE: The name of the database for Postgres to initialize on create
 - PGSQL_USERNAME: The name of the database user for Postgres to set up
 - PGSQL_PASSWORD: The password for Postgres to use for the default user
 - LETSENCRYPT_EMAIL: The email address for Let's Encrypt to use when generating SSL certificates 
 - PHP_ENV: The path to the .env file for PHP/Laravel to use
 
After that, it should be as easy as running `docker-compose up` from within this directory (or some other command for starting the services)

Here is a handy alias to add for initializing the deployment:

`alias commander-init='docker exec -it php-fpm sh -c "php artisan optimize && php artisan migrate --seed"'`