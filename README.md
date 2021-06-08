# drupal-with-docker
how to get up and running with Drupal on a Docker image


How to Docker Drupal


1. Set up mysql db
    
    ➜  ~ docker run -d --name mysqldb -e MYSQL_ROOT_PASSWORD=password mysql:latest

    verify :
    
    ➜  ~ docker ps
	
   | CONTAINER ID | IMAGE | COMMAND | CREATED | STATUS | PORTS | NAMES |
   | --- | --- | --- | --- | --- | --- | --- |
   | f70a31c5eb02 | mysql:latest | "docker-entrypoint.s…" | 44 seconds ago | Up 43 seconds | 3306/tcp, 33060/tcp | mysqldb |

2. create a container for Drupal

    ➜  ~ docker run -d --name drupal --link mysqldb -p 8080:80 -e MYSQL_USER:root -e MYSQL_PASSWORD:password drupal
	
    ➜  ~ docker ps

    | CONTAINER ID | IMAGE | COMMAND | CREATED | STATUS | PORTS | NAMES |
    | --- | --- | --- | --- | --- | --- | --- |
    |38b87f3b04d9   |drupal_dev        | "docker-php-entrypoi…"   |About a minute ago  | Up About a minute |  0.0.0.0:8080->80/tcp  | drupal|
    |f70a31c5eb02   |mysql:latest  | "docker-entrypoint.s…"  | 5 minutes ago       | Up 5 minutes       | 3306/tcp, 33060/tcp  |  mysqldb|

	http://localhost:8080
	returns http://localhost:8080/core/install.php 
	
3. Set the advanced options 
	database name: mysqldb
	database username: root
	database password: password
	host: mysqldb
	port 3306

4. now you are up and running with Drupal on Docker!



## Docker with Composer

    ➜  Sites composer create-project drupal-composer/drupal-project:9.x-dev docker_drupal --no-interaction

    this creates a directory called docker_drupal in your /Sites directory

    ➜  git clone https://github.com/wodby/docker4drupal.git docker_drupal_server

    this clones the docker server repo

    then remove git

    ➜  rm -R docker_drupal_server/.git

    and remove the docker compose override file:

    ➜  rm docker_drupal_server/docker-compose.override.yml 

    copy the files from the server folder to the docker drupal folder

    ➜ cp -R docker_drupal_server/ docker_drupal

    at this point the server settings are in the .env file

2. 
	switch to the docker drupal folder

    ➜ cd docker_drupal 

	➜ docker-compose up -d

	➜ docker ps

	| CONTAINER ID  | IMAGE                             | COMMAND                 | CREATED        | STATUS       |  PORTS                |  NAMES |
	| --- | --- | --- | --- | --- | --- | --- |
	| 37276328c510  | wodby/nginx:1.20-5.14.1           | "/docker-entrypoint.…"  | 2 minutes ago  | Up 2 minutes |  80/tcp               |  my_drupal9_project_nginx |
	| 0ae843dcb459  | wodby/drupal-php:7.4-dev-4.24.13  | "/docker-entrypoint.…"  | 2 minutes ago  | Up 2 minutes |  9000/tcp             |  my_drupal9_project_php |
	| 69799e53f5c7  | wodby/drupal-php:7.4-dev-4.24.13  | "/docker-entrypoint.…"  | 2 minutes ago  | Up 2 minutes |  9000/tcp             |  my_drupal9_project_crond |
	| 84a91aec6041  | traefik:v2.0                      | "/entrypoint.sh --ap…"  | 2 minutes ago  | Up 2 minutes |  0.0.0.0:8000->80/tcp |  my_drupal9_project_traefik |
	| ca12608e80c3  | wodby/mariadb:10.5-3.12.5         | "/docker-entrypoint.…"  | 2 minutes ago  | Up 2 minutes |  3306/tcp             |  my_drupal9_project_mariadb |
	| f4e1f70306a3  | mailhog/mailhog                   | "MailHog"               | 2 minutes ago  | Up 2 minutes |  1025/tcp, 8025/tcp   |  my_drupal9_project_mailhog |
	| 347bf8011439  | drupal                            | "docker-php-entrypoi…"  | 4 hours ago    | Up 4 hours   |  0.0.0.0:8080->80/tcp |  drupal |
	| 08f092e14699  | mysql:latest                      | "docker-entrypoint.s…"  | 4 hours ago    | Up 4 hours   |  3306/tcp, 33060/tcp  |  mysqldb |


3. you are now up and running at drupal.docker.localhost:8000/





