# Getting set up
All projects are cloned into the same directory where laradock lives.
## Info
Caddy is the webserver for used for development. Most projects will already have a route configured in the caddy/caddy/Caddyfile file. Feel free to check out that file for what some of the routes are, eg. 127.0.0.1 loveusoap.test
When there is a new project the route needs to be added to the caddyfile if its not there already. 
## etc/hosts
Most projects have already been added to the caddyfile so it doesn't need much updating. However in order for laradock to route your site correctly you need to change your etc/hosts file locally. Currently, this is how my file looks:
```##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1	    localhost
127.0.0.1       loveusoap.test
127.0.0.1       thecambium.test
127.0.0.1	    cambium.test
127.0.0.1       bestratedtransport.test
127.0.0.1       asiamarketentry.test
127.0.0.1       acn.test
127.0.0.1       asiamarketentry.test
127.0.0.1       asiamarketentry-au.test
127.0.0.1       asiamarketentry-sg.test
127.0.0.1       industrypropertygroup.test
127.0.0.1       staircaseconstructions.test 
127.0.0.1       aluline.test
127.0.0.1       strapi.test

127.0.0.1       mysql

127.0.0.1       elasticsearch

127.0.0.1       redis

127.0.0.1       kibana

127.0.0.1       php-worker

127.0.0.1       puppeteer

127.0.0.1       mailhog

127.0.0.1       minio

127.0.0.1       mssql

127.0.0.1       adminer

127.0.0.1       meilisearch

127.0.0.1       minio.test

127.0.0.1       adminer.test

127.0.0.1       mailhog.test

127.0.0.1       meilisearch.test


255.255.255.255	broadcasthost
::1             localhost

# Added by Docker Desktop
# To allow the same kube context to work on the host and the container:
127.0.0.1 kubernetes.docker.internal
# End of section
```
Feel free to copy and paste. Remember to save as sudo.

## project .env files
In the .env files of your projects there are consistent configs that are the same throughout. 
## mysql
``DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=loveusoap
DB_USERNAME=root
DB_PASSWORD=root``
The only configuration that changes between projects is the DB_DATABASE. 
## redis
For redis to work here is how it should be configured. I havn't been able to reconfigure things for it to not require a password.
```
REDIS_HOST=redis
REDIS_PASSWORD=secret_redis
REDIS_PORT=6379
```
## The commands:
Build. The command below is for building the containers before running them. You know need to build again when you make changes to its configurations. docker-compose build {specify container}
`docker-compose build workspace`
Up. This command spins up the containers for development.
`docker-compose up -d caddy mysql adminer redis laravel-horizon selenium workspace php-worker mailhog`
Down.
`docker-compose down`
Restart.
`docker-compose restart`
Workspace
`workspace`

## To debug
Almost no project uses the same setups so for sure there will be some that I haven't configured their images yet. Laradock provides nearly everything out of the box, it's just a matter of going to laradock .env file, ctrl-f the image you need and set it to true from false.
## Changing php versions and node versions
To switch between node versions laradock provides nvm out of the box.

To switch between php versions, go to laradock .env file and you will see php version. Just change this to whichever you need. Generally I go into the php-fpm and workspace container's Dockerfile and ensure the php version is set to what you need as well.
Make sure docker isn't running while you do this otherwise it won't change until you stop it and rebuild.
- run `docker-compose down` if the container is running
- run `docker-compose build workspace php-fpm`
- run `docker-compose up -d caddy mysql adminer redis laravel-horizon selenium workspace php-worker mailhog`
## info
I created an alias for the workspace container so you can just type 'workspace' in the terminal and it will jump into the workspace container and cd into the directory with all your projects. When you clone a project the workflow usually goes as follows:

- git clone {project}
- ensure the project is in caddyfile and etc/hosts file (the page can be reached)
- type `workspace` into the terminal
- cd into the project you choose
- npm install
- composer install
- php artisan migrate

