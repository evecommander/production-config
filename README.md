# Eve Commander Production
This repo contains the required files to run Eve Commander in a production setting. At the moment, this includes a Docker Compose setup but may also include a Kubernetes setup in the future...

## Setting Up The Proxy
This project uses an Nginx reverse proxy to serve the containers and this first needs to be set up. To spin up the proxy, simply run `docker-compose up -d` while in the proxy directory.

### Creating a Shared Network
Now, we need to create a network for our proxy to talk to the containers by running `docker network create service-tier` and connect the proxy with `docker network connect service-tier {nginx-proxy container ID}`

## Docker Compose
Before running the Docker Compose file, make sure that the following environment variables are set:
 - PGSQL_DATABASE: The name of the database for Postgres to initialize on create
 - PGSQL_USERNAME: The name of the database user for Postgres to set up
 - PGSQL_PASSWORD: The password for Postgres to use for the default user
 - LETSENCRYPT_EMAIL: The email address for Let's Encrypt to use when generating SSL certificates 
 - PHP_ENV_PATH: The path to the .env file for PHP/Laravel to use
 - BRANCH: The name of the branch to use when fetching containers
 - DOMAIN_NAME_BASE: The base domain name that will be used for each service
 - DOMAIN_NAME_PREFIX: The prefix that will be added to each domain name
 
After that, it should be as easy as running `docker-compose up -d` from within this directory (or some other command for starting the services)

Don't forget to initialize any new deployments with 

`docker exec -it {php-fpm container ID} sh -c "php artisan optimize && php artisan migrate --seed"`

## Enabling SSL
The proxy used for this project also makes it very easy to enable SSL for the services. Simply use the SSL enabled compose file as an override file like so:
`docker-compose -f docker-compose.yml -f docker-compose-ssl.yml up`

Make sure that ALL environment variables are filled when using SSL (except maybe `DOMAIN_NAME_PREFIX`) as Let's Encrypt has a rate limit on its usage.